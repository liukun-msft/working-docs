# Only emit AMQP channels downstream when they are active #24582

## Issue Description

https://github.com/Azure/azure-sdk-for-java/issues/24582

**Behavior**

When the CBS node is closed, it'll make a request upstream for a new instance. This instance is immediately emitted even if it's not active. This should alleviate the mass of logs accumulated about retries and fix retry policy-related issues.

**logs**

```
11:39:05.524 [main] INFO  com.azure.messaging.eventhubs.implementation.EventHubConnectionProcessor - Upstream connection publisher was completed. Terminating processor.
11:39:05.524 [main] INFO  com.azure.messaging.eventhubs.implementation.EventHubConnectionProcessor - namespace[eventhubs-liuku.servicebus.windows.net] entityPath[eventhub-test]: AMQP channel processor completed. Notifying 0 subscribers.
11:39:05.525 [main] INFO  com.azure.core.amqp.implementation.ReactorConnection - connectionId[MF_e2b73b_1646192276393] signal[Disposed by client., isTransient[false], initiatedByClient[true]]: Disposing of ReactorConnection.
11:39:05.525 [main] INFO  com.azure.messaging.eventhubs.implementation.EventHubConnectionProcessor - namespace[eventhubs-liuku.servicebus.windows.net] entityPath[eventhub-test]: Channel is disposed.
11:39:05.818 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.SessionHandler - onSessionRemoteClose connectionId[eventhub-test], entityName[MF_e2b73b_1646192276393], condition[Error{condition=null, description='null', info=null}]
11:39:05.818 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.SessionHandler - onSessionRemoteClose connectionId[cbs-session], entityName[MF_e2b73b_1646192276393], condition[Error{condition=null, description='null', info=null}]
11:40:05.541 [parallel-8] INFO  com.azure.core.amqp.implementation.RequestResponseChannel - connectionId[MF_e2b73b_1646192276393] linkName[cbs] Timed out waiting for RequestResponseChannel to complete closing. Manually closing.
11:40:05.541 [parallel-7] INFO  com.azure.core.amqp.implementation.RequestResponseChannel - connectionId[MF_e2b73b_1646192276393] linkName[cbs] Timed out waiting for RequestResponseChannel to complete closing. Manually closing.
11:40:05.541 [parallel-8] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Channel is closed. Requesting upstream. 
11:40:05.542 [parallel-8] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Connection not requested, yet. Requesting one.
11:40:05.543 [parallel-8] INFO  com.azure.core.amqp.implementation.ReactorConnection - connectionId[MF_e2b73b_1646192276393] entityPath[$cbs] linkName[cbs] Emitting new response channel.
11:40:05.543 [parallel-8] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Setting next AMQP channel.
11:40:05.543 [parallel-8] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Next AMQP channel received, updating 3 current subscribers
11:40:05.543 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.SendLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Sender link was never active. Closing endpoint states.
11:40:05.545 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Receiver link was never active. Closing endpoint states.
11:40:05.545 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Channel is closed. Requesting upstream. 
11:40:05.546 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Connection not requested, yet. Requesting one.
11:40:05.546 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.ReactorConnection - connectionId[MF_e2b73b_1646192276393] entityPath[$cbs] linkName[cbs] Emitting new response channel.
11:40:05.546 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Setting next AMQP channel.
11:40:05.547 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Next AMQP channel received, updating 3 current subscribers
11:40:05.548 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.ReactorConnection - connectionId[MF_e2b73b_1646192276393] Closing executor.
11:40:05.548 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ConnectionHandler - onConnectionLocalClose connectionId[MF_e2b73b_1646192276393] hostname[eventhubs-liuku.servicebus.windows.net] errorCondition[null] errorDescription[null]
11:40:05.548 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.SendLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Sender link was never active. Closing endpoint states.
11:40:05.548 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Receiver link was never active. Closing endpoint states.
11:40:05.549 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Channel is closed. Requesting upstream. 
11:40:05.549 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Connection not requested, yet. Requesting one.
11:40:05.549 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.ReactorConnection - connectionId[MF_e2b73b_1646192276393] entityPath[$cbs] linkName[cbs] Emitting new response channel.
11:40:05.549 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Setting next AMQP channel.
11:40:05.550 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Next AMQP channel received, updating 3 current subscribers
11:40:05.550 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.SendLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Sender link was never active. Closing endpoint states.
11:40:05.550 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Receiver link was never active. Closing endpoint states.
11:40:05.551 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Channel is closed. Requesting upstream. 
11:40:05.551 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Connection not requested, yet. Requesting one.
11:40:05.551 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.ReactorConnection - connectionId[MF_e2b73b_1646192276393] entityPath[$cbs] linkName[cbs] Emitting new response channel.
11:40:05.552 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Setting next AMQP channel.
11:40:05.552 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Next AMQP channel received, updating 3 current subscribers
11:40:05.552 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.SendLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Sender link was never active. Closing endpoint states.
11:40:05.552 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Receiver link was never active. Closing endpoint states.
11:40:05.553 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Channel is closed. Requesting upstream. 
11:40:05.553 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Connection not requested, yet. Requesting one.
11:40:05.554 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.ReactorConnection - connectionId[MF_e2b73b_1646192276393] entityPath[$cbs] linkName[cbs] Emitting new response channel.
11:40:05.554 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Setting next AMQP channel.
11:40:05.554 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Next AMQP channel received, updating 3 current subscribers
11:40:05.555 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.SendLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Sender link was never active. Closing endpoint states.
11:40:05.555 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - connectionId[MF_e2b73b_1646192276393] linkName[cbs] entityPath[$cbs] Receiver link was never active. Closing endpoint states.
11:40:05.555 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Channel is closed. Requesting upstream. 
11:40:05.555 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.RequestResponseChannel:$cbs - namespace[MF_e2b73b_1646192276393] entityPath[$cbs]: Connection not requested, yet. Requesting one.
...
```

## Root cause analysis

**Repro**

Create a client(e.g. producer), send message and close it after commpleted.


**Reason**

When cbs node is closed, its `close()` method will call `requestUpStream()` to emit a new channel immediately. So that `AmqpChannelProcessor#onNext()`  is triggered and inside this method, it call cbs node `close()` method again, which cause an infinite loop of channel's opening and closing.

The `requestUpStream()` was added from PR [#22534](https://github.com/Azure/azure-sdk-for-java/pull/22534) to fix issue [#22533](https://github.com/Azure/azure-sdk-for-java/issues/22533).


**Details**

When connection is closing, it trigger `ReactorConnection#Async()` to close channels and sessions. CBS channel is closed by `RequestResponseChannel#closeAysnc()`, and use `RequestResponseChannel#onTerminalState()` to terminate sender and receiver links. 

When CBS sender and receiver links are closed. It will emit a complete signal:

```Java
private void onTerminalState() {
    ...
    endpointStates.emitComplete(((signalType, emitResult) -> onEmitSinkFailure(...)));
```

The complete signal is catched by `AmqpChannelProcessor#connectionSubscription`. Because channel status is not disposed currently, it goes to `AmqpChannelProcessor#setAndClearChannel()` which actually close this channel, and then goes to `AmqpChannelProcessor#requestUpstream()`.

```Java
endpointStatesFunction.apply(amqpChannel).subscribe(
    state -> {...},
    error -> {...},
    () -> {
        if (isDisposed()) {
            ...
        } else {
            logger.info("Channel is closed. Requesting upstream.");
            setAndClearChannel();
            requestUpstream();
        }
    });
```

Inside `AmqpChannelProcessor#requestUpstream()`, it request a new channel.

```Java
 private void requestUpstream() {
       ...
        if (!isRequested.getAndSet(true)) {
            logger.info("Connection not requested, yet. Requesting one.");
            subscription.request(1);
        }
    }

```

Because CBS channel is requested from a repeat Flux inside `ReactorConnection#createRequestResponseChannel()`, so a new channel could emit immediately and call `AmqpChannelProcessor#onNext()`.

```Java
protected AmqpChannelProcessor<RequestResponseChannel> createRequestResponseChannel(
    final Flux<RequestResponseChannel> createChannel =
        ...
            return activeChannel.thenReturn(channel);
        .doOnNext(e -> {
            ...
            .log("Emitting new response channel.");
        })
        .repeat();

```

Inside `onNext()`, the `close(oldChannel)` is called.  

```Java
public void onNext(T amqpChannel) {
    logger.info("Setting next AMQP channel.");
    ...
    close(oldChannel);

    if (oldSubscription != null) {
        oldSubscription.dispose();
    }
}
```

And `AmqpChannelProcessor#close()` will call `RequestResponseChannel#closeAysnc()`, which form an infinite loop for cbs channel. 
```Java
private void close(T channel) {
    if (channel instanceof AsyncCloseable) {
        ((AsyncCloseable) channel).closeAsync().subscribe();
    ...
```

And we could see the same logs to prove the behavior:

```
Setting next AMQP channel. // AmqpChannelProcessor#onNext()
Next AMQP channel received...
Sender link was never active. Closing endpoint states. //close()
Receiver link was never active. Closing endpoint states. //close()
Channel is closed. Requesting upstream. //AmqpChannelProcessor#connectionSubscription.onComplete()
Connection not requested, yet. Requesting one. // AmqpChannelProcessor#requestUpstream()
Setting next AMQP channel.
...

```

**Call graph**

![img](./cbs-note-close-issue.png
)


**Current Solution**

Revert the change in PR #22534 to fix this issue.

Figure out the reason for issue [#22533](https://github.com/Azure/azure-sdk-for-java/issues/22533)