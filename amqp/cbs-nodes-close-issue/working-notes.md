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


**

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

We manually try to close an closed CBS node twice. and both got timeout.

First time: 
```Java
...
//RequestResponseChannel
this.subscriptions = Disposables.composite(
    ...
    amqpConnection.getShutdownSignals().next().flatMap(signal -> {
        logger.verbose("Shutdown signal received.");
        return closeAsync();
    }).subscribe()
);
```
Second time: 

```Java
//ReactorConnection
final Mono<Void> cbsCloseOperation;
if (cbsChannelProcessor != null) {
    cbsCloseOperation = cbsChannelProcessor.flatMap(channel -> channel.closeAsync());
} else {
    cbsCloseOperation = Mono.empty();
}
```

These two manually calls both encounter the timeout exceptions and create a new thread to resume.

### Issue 2: CBS node close timeout exception

**Previos Reason - log view**

 `closeMono` complete signal will close the timeout. And `closeMono` is completed after link handler is completed.

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

And link handler is completed signal after `closeConnectionWork` is finished. So `cbsCloseOperation` have timeout to wait `closeConnectionWork` finished, but `closeConnectionWork` always wait `cbsCloseOperation` finished as code show below, so that is a deadlock scenario and in the end cause the timeout exception.

```Java
//ReactorConnection
Mono<Void> closeAsync(AmqpShutdownSignal shutdownSignal) {
    ...
    final Mono<Void> cbsCloseOperation;
    if (cbsChannelProcessor != null) {
        cbsCloseOperation = cbsChannelProcessor.flatMap(channel -> channel.closeAsync());
    }
    ...
    final Mono<Void> managementNodeCloseOperations = Mono.when(
        Flux.fromStream(managementNodes.values().stream()).flatMap(node -> node.closeAsync()));

    final Mono<Void> closeReactor = Mono.fromRunnable(() -> {
        ...
                dispatcher.invoke(() -> closeConnectionWork());
        ...
    });
    return Mono.whenDelayError(
        cbsCloseOperation.doFinally(signalType ->
        ...
        managementNodeCloseOperations.doFinally(signalType ->
        ...
        .then(closeReactor.doFinally(signalType ->
        .then(isClosedMono.asMono());
}
```

**Real Reason - design view**

However, `cbsCloseOperation` is call `onLinkFinal` to complete link. This is not initial design. Actually, the root cause is `onLinkRemoteClose` is never fired because of no Detach frame is sent out for CBS links. And check in code logic, the CBS session is close earlier than links, so that it directly send out End Frame and after that, when `link.close()` is called, no Detach frame is sent out since session is closed.


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

4. Try to use takeUntilOther(shutdown signal) after repeat (issue 3)
    - should work with step 5 
    - No request upstream log

5. Remove manually close cbs channel in ReactorConnection (issue 2)
    - No timeout issue

change logs of 1 + 2 + 3: [solution-logs-123](./solution-logs-123.md)


change log of 1 + 4 + 5: [solution-logs-145](./solution-logs-145.md)

(Current change) change log of 4 + 5: [solution-log-45](./solution-logs-45.md)


