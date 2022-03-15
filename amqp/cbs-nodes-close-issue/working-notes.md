# Only emit AMQP channels downstream when they are active #24582

## Issue

https://github.com/Azure/azure-sdk-for-java/issues/24582

**Behavior**

When the CBS node (ClaimsBasedSecurityNode) is closed, it'll make a request upstream for a new channel(RequestResponseChannel). This channel is immediately emitted. As connection is inactive, it is closed then and request a upstream again. The CBS node constantly retry process cause mass of logs.

**Related issue or changes**

- Message receiving is stuck because AMQP connection is closed without an error

    https://github.com/Azure/azure-sdk-for-java/issues/22533

    https://github.com/Azure/azure-sdk-for-java/pull/22534

- Add timeout to cleaning up sender/receiver links
https://github.com/Azure/azure-sdk-for-java/pull/23381

### Logs

[Client closing logs](./logs.md)

We can define 5 phases for the closing logs:

1. Connection start to close
2. CBS node start to close
3. CBS node close timeout exception
4. CBS node constantly retry logs 
5. ReactorExecutor closed

### Call graph 

**CBS node start to close and timeout exception**

![img](./cbs-close-issue-1.jpg)

**CBS node constantly retry and ReactorExecutor closed**

![img](./cbs-close-issue-2.jpg)


## Analysis issues

### Issue 1: Two threads are created to close CBS node

**Reason**

AmpqShutdownSigal catched by `RequestResponseChannel#amqpConnection`, which cause the duplicate close action for CBS node.  

```Java
...
//RequestResponseChannel
this.subscriptions = Disposables.composite(
    ...
    //TODO (conniey): Do we need this if we already close the request response nodes when the
    // connection.closeWork is executed? It would be preferred to get rid of this circular dependency.
    amqpConnection.getShutdownSignals().next().flatMap(signal -> {
        logger.verbose("Shutdown signal received.");
        return closeAsync();
    }).subscribe()
);
```

When connection close (`ReactorConnection#closeAsync`), it will explictly close `cbsChannelProcessor`.

```Java
//ReactorConnection
Mono<Void> closeAsync(AmqpShutdownSignal shutdownSignal){
    ...
    if (cbsChannelProcessor != null) {
        cbsCloseOperation = cbsChannelProcessor.flatMap(channel -> channel.closeAsync());
    } 
    ....
 }
```
### Issue 2: CBS node close timeout exception

**Reason**

We manually close an closed CBS node.

```Java
//ReactorConnection
final Mono<Void> cbsCloseOperation;
if (cbsChannelProcessor != null) {
    cbsCloseOperation = cbsChannelProcessor.flatMap(channel -> channel.closeAsync());
} else {
    cbsCloseOperation = Mono.empty();
}
```

CBS node has already closed after we closed CBS session, so if we close the CBS node again, no `closeMono` complete signal will emit which cause the timeout Exception.

**Code Details**

When `ReactorConnection` is shutting down, it will emit `shutdownSignal`.

```Java
//ReactorConnection
Mono<Void> closeAsync(AmqpShutdownSignal shutdownSignal) {
    addShutdownSignal(logger.atInfo(), shutdownSignal).log("Disposing of ReactorConnection.");
    final Sinks.EmitResult result = shutdownSignalSink.tryEmitValue(shutdownSignal);
    ...
}
```

Then the `shutdownSignal` is catched by all `ReactorSession` on that `ReactorConnection`, and `ReactorSession` is going to closed first.

```Java
//ReactorSession
shutdownSignals.flatMap(signal ->  closeAsync("Shutdown signal received", null, false)).subscribe());
```

The session will schedule `disposeLinks` tasks. So the links on that session will be closed also.

```Java
//ReactorSession
Mono<Void> closeAsync(String message, ErrorCondition errorCondition, boolean disposeLinks) {
    ...
        return Mono.fromRunnable(() -> {
            try {
                provider.getReactorDispatcher().invoke(() -> disposeWork(errorCondition, disposeLinks));
            } 
            ...
        }).then(isClosedMono.asMono());
    }
```

After we close cbs session, it will automatically close cbs sender and receiver link. Then `RequestResponseChannel` has `receiveLinkHandler` and `sendLinkHandler` to catch up that completed signal to close CBS node.

```Java
//RequestResponseChannel
 receiveLinkHandler.getEndpointStates().subscribe(state -> {
        updateEndpointState(null, AmqpEndpointStateUtil.getConnectionState(state));
    }, error -> {
       ...
    }, () -> {
        closeAsync().subscribe();
        onTerminalState("ReceiveLinkHandler");
    }),

    sendLinkHandler.getEndpointStates().subscribe(state -> {
        updateEndpointState(AmqpEndpointStateUtil.getConnectionState(state), null);
    }, error -> {
       ...
    }, () -> {
        closeAsync().subscribe();
        onTerminalState("SendLinkHandler");
    })
```

When `RequestResponseChannel#onTerminalState` is called twice (send and receiver), it will emit `closeMono` complete signal. 

```Java
//RequestResponseChannel
private void onTerminalState(String handlerName) {
    ...
    closeMono.emitEmpty((signalType, emitResult) -> onEmitSinkFailure(...));
    ...
}
```
 `closeMono` complete signal will close the timeout.

```Java
//RequestResponseChannel
final Mono<Void> closeOperationWithTimeout = closeMono.asMono()
        .timeout(retryOptions.getTryTimeout())
        .onErrorResume(TimeoutException.class, error -> {
                ...
            });
        })
        .subscribeOn(Schedulers.boundedElastic());
```

However, then if we explictly close CBS node again, it just call `sendLink` and `receiveLink` to close, which is already closed. So that it won't trigger `receiveLinkHandler` and `sendLinkHandler` to call `onTerminalState` twice, and cause timeout. (As a workaround way is to add `onTerminalState` in closeAync, which can avoid timeout exception, but the logic is not correct). We could consider whether to have `sendLink.close()` or `receiveLink.close()` here, maybe it can be use for someone explictly call this function rather than close session.


``` Java
//RequestResponseChannel
public Mono<Void> closeAsync() {
   ...
    logger.verbose("Closing request/response channel.");
    return Mono.fromRunnable(() -> {
        try {
            // Schedule API calls on proton-j entities on the ReactorThread associated with the connection.
            provider.getReactorDispatcher().invoke(() -> {
                logger.verbose("Closing send link and receive link.");

                sendLink.close();
                receiveLink.close();
            }
            ...
```


## Issue 3: CBS node retry infinite loop which cause massive logs

**Reason** 

When CBS node (RequestResponseChannel) is closed, it will emit a complete signal, and that signal is catch by `connectionSubscription` which will request a new upstream `RequestResponseChannel`. 

But when new instance of `RequestResponseChannel` is created, it will be closed immediately since connection is shutdown. And that close action will emit a complete signal again and request upstream again. 

This back and forth process cause massive log in console. 


**Code Details** 

When CBS sender and receiver links are closed. It will emit a complete signal:

```Java
//RequestResponseChannel
private void onTerminalState() {
    ...
    endpointStates.emitComplete(((signalType, emitResult) -> onEmitSinkFailure(...)));
    ...
}
```

The complete signal is catched by `connectionSubscription`. Because channel status is not disposed currently and then goes to `requestUpstream()`.

```Java
//AmqpChannelProcessor
connectionSubscription = endpointStatesFunction.apply(amqpChannel).subscribe(
        ...
        () -> {
                ...
                requestUpstream();
        });
```

Because CBS channel is requested from a repeat Flux inside `createRequestResponseChannel()`, so a `RequestResponseChannel` instance is created.

```Java
//ReactorConnection
protected AmqpChannelProcessor<RequestResponseChannel> createRequestResponseChannel(
    final Flux<RequestResponseChannel> createChannel = ...
         .map(reactorSession -> new RequestResponseChannel(...))
        .doOnNext(e -> {...})
        .repeat();
```
However, inside `RequestResponseChannel` constructor, it call `closeAysc()` and schedule link close tasks, which close current channel after it created.

```Java
...
//RequestResponseChannel
this.subscriptions = Disposables.composite(
    ...
    //TODO (conniey): Do we need this if we already close the request response nodes when the
    // connection.closeWork is executed? It would be preferred to get rid of this circular dependency.
    amqpConnection.getShutdownSignals().next().flatMap(signal -> {
        logger.verbose("Shutdown signal received.");
        return closeAsync();
    }).subscribe()
);
```

As new channel is closed, it will emit a complete signal to request new upstream, which goes to the beginning step.

This is a infinite loop for CBS node.

### Test Solutions(not final solution)

1. Remove `RequestResponseChannel#amqpConnection` to avoid duplicate closing (issue 1, issue 3)

    - Only have one close thread
    - No infinite loop, but have one request upstream log

2. Explict Emit `closeMono` complete signal when first time close channel(issue 2)
   
    - No timeout issue
    

3. Not to emit channel complete signal when close/ remove requestUpStream() (issue 3)

    - No request upstream log


Do the above changes: [solution-logs-123](./solution-logs-123.md)

4. Try to use takeUntilOther(shutdown signal) after repeat (issue 3)
    - should work with step 5 
    - No request upstream log

5. Remove manually close cbs channel in ReactorConnection (issue 2)
    - No timeout issue

 
Current changes: [solution-logs-145](./solution-logs-145.md)
1 + 4 + 5


