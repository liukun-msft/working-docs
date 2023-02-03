##  Reason why link can not recovered

### Repro Log Analysis

When we try to receive message for a empty session queue. The link is on UNINITIALIZED status and open for waiting a response from service.

```
//Repro log when link is open and waiting for attach response
21:01:28.976 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.ReactorSession - {"az.sdk.message":"Creating a new receiver link.","connectionId":"MF_d94f27_1675429285720","sessionName":"sessionqueue","linkName":"session-_dc47bf_1675429285804"}
21:01:28.979 [reactor-executor-1] DEBUG com.azure.core.amqp.implementation.ReactorReceiver - {"az.sdk.message":"State UNINITIALIZED","connectionId":"MF_d94f27_1675429285720","entityPath":"sessionqueue","linkName":"session-_dc47bf_1675429285804"}
21:01:28.980 [reactor-executor-1] DEBUG com.azure.core.amqp.implementation.ReactorReceiver - {"az.sdk.message":"Token refreshed.","connectionId":"MF_d94f27_1675429285720","entityPath":"sessionqueue","linkName":"session-_dc47bf_1675429285804","response":"ACCEPTED"}
21:01:28.986 [reactor-executor-1] DEBUG com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkLocalOpen","connectionId":"MF_d94f27_1675429285720","entityPath":"sessionqueue","linkName":"session-_dc47bf_1675429285804","localSource":"Source{address='sessionqueue', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, distributionMode=null, filter={com.microsoft:session-filter=null}, defaultOutcome=null, outcomes=null, capabilities=null}"}
[1674059459:0] -> Attach{name='session-_dc47bf_1675429285804', handle=0, role=RECEIVER, sndSettleMode=UNSETTLED, rcvSettleMode=SECOND, source=Source{address='sessionqueue', durable=NONE, expiryPolic...}
```

As repro, we wait until timeout and service response with the DETACH frame. The link was force to CLOSED due to the timeout error, but we set the error condition to be null in code level. After that, we can see the link can't be recovered.  

```
//Repro log when link is force close by remote
[1674059459:0] <- Attach{name='session-_dc47bf_1675429285804', handle=0, role=SENDER, sndSettleMode=MIXED, rcvSettleMode=FIRST, source=null, target=null, unsettled=null, incompleteUnsettled=false, initialDeliveryCount=null, maxMessageSize=null, offeredCapabilities=null, desiredCapabilities=null, properties=null}
[1674059459:0] <- Detach{handle=0, closed=true, error=Error{condition=com.microsoft:timeout, description='The operation did not complete within the allotted timeout of 00:00:29. The time allotted to this operation may have been a portion of a longer timeout. For more information on exception types and proper exception handling...}
21:01:58.200 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkRemoteOpen","connectionId":"MF_d94f27_1675429285720","entityPath":"sessionqueue","linkName":"session-_dc47bf_1675429285804","action":"waitingForError"}
21:01:58.201 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkRemoteClose","connectionId":"MF_d94f27_1675429285720","errorCondition":null,"errorDescription":null,"linkName":"session-_dc47bf_1675429285804","entityPath":"sessionqueue"}
...
[1674059459:0] -> Detach{handle=0, closed=true, error=null}
21:01:58.207 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkFinal","connectionId":"MF_d94f27_1675429285720","linkName":"session-_dc47bf_1675429285804","entityPath":"sessionqueue"}
```

That cause the link can't be recovered, and after 5 minutes, the connection was shutdown because of inactive and can't be recover either. (We have explained why connection can't be recovered)

```
//Repro log when connection shutdown
[1674059459:0] <- Close{error=Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 300000 milliseconds and is closed by container 'LinkTracker'. TrackingId:a5f41ab242264a22930891867ea98edb_G26, SystemTracker:gateway7, Timestamp:2023-02-03T13:06:29', info=null}}
21:06:29.224 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.handler.SessionHandler - {"az.sdk.message":"onSessionRemoteClose","connectionId":"MF_d94f27_1675429285720","errorCondition":null,"errorDescription":null,"sessionName":"sessionqueue"}
...
21:06:33.268 [reactor-executor-1] INFO  com.azure.core.amqp.implementation.ReactorConnection - {"az.sdk.message":"onConnectionShutdown. Shutting down.","connectionId":"MF_d94f27_1675429285720","isTransient":false,"isInitiatedByClient":false,"shutdownMessage":"Finished processing pending tasks.","namespace":"xxx"}
//no more logs after here...
```


### Code Analysis
From code side, when the link is closed by remote (the real scenario is remote is close because of OS upgrade), so the `onLinkRemoteClose()` was called. 

```java
//LinkHandler.class
public void onLinkRemoteClose(Event event) {
    handleRemoteLinkClosed("onLinkRemoteClose", event);
}   
```

Then jump to its private function `handleRemoteLinkClosed()`, as we mock the response that error condition is null, we hardcode the condition to a empty instance. So the logic goes to `super.close()` rather than `onError(exception)`.

```java
//LinkHandler.class   
private void handleRemoteLinkClosed(final String eventName, final Event event) {
    final Link link = event.getLink();
    //Here we change to use a empty error condition to repro
    final ErrorCondition condition = new ErrorCondition();
    ...
    if (condition != null && condition.getCondition() != null) {
        ...
        onError(exception);
    } else {
        super.close();
    }
```

The super class is `Handler`, inside the `close()` function, the `endpointStates` will emit CLOSED as next signal and complete signal.

```Java
//Handler.class
public void close() {
    ...
    endpointStates.emitNext(EndpointState.CLOSED, (signalType, emitResult) -> {
        ...
    });
    endpointStates.emitComplete((signalType, emitResult) -> {
        ...
    });
}
```

Once the next signal of CLOSED was emitted, the status of created link changed from UNINTIALIZED to CLOSED. It doesn't have any effect on `getActiveLink()` operator chain as we `.takeUntil(e -> e == AmqpEndpointState.ACTIVE)`.

But once the complete signal was emitted, it will complete the `.takeUntil() + .timeout()`, and move to `.then(Mono.just(link)))`, and then pass down that CLOSED link. So the downstream subscriber can't receive any message from that CLOSED link.

```Java
//ServiceBusSessionManager.class
Mono<ServiceBusReceiveLink> getActiveLink() {
    ...
    return Mono.defer(() -> createSessionReceiveLink()
        .flatMap(link -> link.getEndpointStates()
            .takeUntil(e -> e == AmqpEndpointState.ACTIVE)
            .timeout(operationTimeout)
            .then(Mono.just(link))))
        .retryWhen(Retry.from(retrySignals -> retrySignals.flatMap(signal -> {
            ...
            if (isDisposed.get()) {
                ...
            } else if (failure instanceof TimeoutException) {
                return Mono.delay(SLEEP_DURATION_ON_ACCEPT_SESSION_EXCEPTION);
            } else if (failure instanceof AmqpException
                && ((AmqpException) failure).getErrorCondition() == AmqpErrorCondition.TIMEOUT_ERROR) {
                return Mono.delay(SLEEP_DURATION_ON_ACCEPT_SESSION_EXCEPTION);
            } else {
                return Mono.<Long>error(failure);
            }
        })));
}
```

The problem here is that case skip our `retryWhen()` as there is no error emit. We need to handle this special case when link status was change directly from UNINTIALIZED to CLOSED. 

## Fixes

Currently the ideal for the fix is to make sure we are not passing a CLOSED link , and that skip the retry policy as no error emit.

Below solutions are checking whether a link is disposed and if so, change it to an error so that it can trigger the retry:

Temp Solution 1：

```Java
        .then(Mono.fromCallable(() -> link.isDisposed() ? null : link))
        .switchIfEmpty(Mono.error(() -> new TimeoutException("Mock Timeout exception to trigger Mono.delay() condition in retry")))
        //we receive the link on reactor-executor thread, we need to publish on a non-blocking thread for retry
        .publishOn(Schedulers.boundedElastic())  
```
Temp Solution 2： Add a filter to filter out dispose link then use switchIfEmpty to change to error
```Java
        .then(Mono.just(link))
        .filter(receiveLink -> !receiveLink.isDisposed())
        .switchIfEmpty(Mono.error(() -> new TimeoutException("Mock Timeout exception to trigger Mono.delay() condition in retry")))
        .publishOn(Schedulers.boundedElastic()) 
```

Temp Solution 3：

```Java
    ...
    .then(Mono.just(link))
    .flatMap(receiveLink -> {
        if(receiveLink.isDisposed()) {
            return Mono.error(() -> new TimeoutException("Mock Timeout exception to trigger Mono.delay() condition in retry"));
        } else {
            return Mono.just(receiveLink);
        }
    })
    .publishOn(Schedulers.boundedElastic())
    ...
```

Temp Solution 4：
```Java   
    ...
    .then(Mono.fromCallable(() -> {
        if(link.isDisposed()) {
            throw new TimeoutException("Mock Timeout exception to trigger Mono.delay() condition in retry");
        } else {
            return link;
        }
    }))
    .publishOn(Schedulers.boundedElastic())
    ...
```
