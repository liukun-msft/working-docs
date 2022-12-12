# [BUG] Service Bus IllegalStateException Exception: Cannot add credits to closed link


## Issue

https://github.com/Azure/azure-sdk-for-java/issues/31793

Some users encountered the "Cannot add credits to closed link" error, and then their processor stop receiving messages.

```LOG
[ERROR] 2022-12-07 11:25:03.707 reactor.core.publisher.Operators:315 - Operator called default onErrorDropped
reactor.core.Exceptions$ErrorCallbackNotImplemented: java.lang.IllegalStateException: Cannot add credits to closed link: xxx
Caused by: java.lang.IllegalStateException: Cannot add credits to closed link: xxxx
            at com.azure.core.amqp.implementation.ReactorReceiver.addCredits(ReactorReceiver.java:202) ~[azure-core-amqp-2.6.0.jar!/:2.6.0]
            at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.checkAndAddCredits(ServiceBusReceiveLinkProcessor.java:573) ~[azure-messaging-servicebus-7.10.0.jar!/:7.10.0]
            at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.onNext(ServiceBusReceiveLinkProcessor.java:268) ~[azure-messaging-servicebus-7.10.0.jar!/:7.10.0]
            at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.onNext(ServiceBusReceiveLinkProcessor.java:48) ~[azure-messaging-servicebus-7.10.0.jar!/:7.10.0]
            at reactor.core.publisher.FluxRepeatPredicate$RepeatPredicateSubscriber.onNext(FluxRepeatPredicate.java:86) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.MonoPeekTerminal$MonoTerminalPeekSubscriber.onNext(MonoPeekTerminal.java:180) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.SerializedSubscriber.onNext(SerializedSubscriber.java:99) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.FluxRetryWhen$RetryWhenMainSubscriber.onNext(FluxRetryWhen.java:174) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onNext(FluxOnErrorResume.java:79) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.FluxPeekFuseable$PeekFuseableSubscriber.onNext(FluxPeekFuseable.java:210) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.Operators$MonoSubscriber.request(Operators.java:1906) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.FluxPeekFuseable$PeekFuseableSubscriber.request(FluxPeekFuseable.java:144) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.request(Operators.java:2158) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.request(Operators.java:2158) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.SerializedSubscriber.request(SerializedSubscriber.java:151) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.MonoPeekTerminal$MonoTerminalPeekSubscriber.request(MonoPeekTerminal.java:139) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.request(Operators.java:2158) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.requestUpstream(ServiceBusReceiveLinkProcessor.java:451) ~[azure-messaging-servicebus-7.10.0.jar!/:7.10.0]
            at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.lambda$onError$7(ServiceBusReceiveLinkProcessor.java:352) ~[azure-messaging-servicebus-7.10.0.jar!/:7.10.0]
            at reactor.core.publisher.LambdaMonoSubscriber.onNext(LambdaMonoSubscriber.java:171) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.MonoDelay$MonoDelayRunnable.propagateDelay(MonoDelay.java:271) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.publisher.MonoDelay$MonoDelayRunnable.run(MonoDelay.java:286) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28) ~[reactor-core-3.4.8.jar!/:3.4.8]
            at java.util.concurrent.FutureTask.run(FutureTask.java:264) ~[?:?]
            at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304) ~[?:?]
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136) ~[?:?]
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635) ~[?:?]
            at java.lang.Thread.run(Thread.java:833) ~[?:?]

```

This error only happens occasionally and since we lack logs, it is not easy to find the root cause. However, according to the comments in the github issue, it may be related to the error message that links are force-detached.

```LOG
{"az.sdk.message":"Transient error occurred.","exception":"The link 'G6:60217897:topic/subscriptions/task_4d4a8c_1665483708655' is force detached. Code: consumer(link3281). Details: AmqpMessageConsumer.IdleTimerExpired: Idle timeout: 00:10:00. TrackingId:7e23e7c70011023400000cd1634543bd_G6_B72, SystemTracker:name-spacet:Topic:topic|task, Timestamp:2022-10-11T17:21:48, errorContext[NAMESPACE: name-space.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: topic/subscriptions/task, REFERENCE_ID: name-space/subscriptions/task_4d4a8c_1665483708655, LINK_CREDIT: 0]","linkName":"n/a","entityPath":"n/a","attempt":1,"retryAfter":4522}
```


## Analysis Code

Base on the error stack trace, we can go though the code to see what happening inside the sdk code.

First, the upstream of `ServiceBusReceiveLinkProcessor` throw a transient error. (Most likely the remote link is force-detached because of idle). Then the linkProcessor's `onError()` was called and client will retry to get a new link (call `requestUpstream()`).

```Java
//ServiceBusReceiveLinkProcessor#onError
 @Override
    public void onError(Throwable throwable) {
        ... 
        final int attempt = retryAttempts.incrementAndGet();
        final Duration retryInterval = retryPolicy.calculateRetryDelay(throwable, attempt);

        if (retryInterval != null && upstream != Operators.cancelledSubscription()) {
            LOGGER.atWarning()
                .addKeyValue(LINK_NAME_KEY, linkName)
                .addKeyValue(ENTITY_PATH_KEY, entityPath)
                .addKeyValue("attempt", attempt)
                .addKeyValue("retryAfter", retryInterval.toMillis())
                .log("Transient error occurred.", throwable);

            retrySubscription = Mono.delay(retryInterval).subscribe(i -> requestUpstream());

            return;
        }

        LOGGER.atWarning()
            .addKeyValue(LINK_NAME_KEY, linkName)
            .addKeyValue(ENTITY_PATH_KEY, entityPath)
            .log("Non-retryable error occurred in AMQP receive link.", throwable);
        ...

        onDispose();
    }
```

Then `requestUpstream()` will request one from upstream.

```Java
//ServiceBusReceiveLinkProcessor#requestUpstream
private void requestUpstream() {
    //... check if linkProcessor and upstream 

    LOGGER.info("Requesting a new AmqpReceiveLink from upstream.");
    upstream.request(1L);
}
```

Then the upstream (Active Conntion -> Active Session -> CreateLink) pass down the active link and calls the linkProcessor's `onNext()`.

```Java
//ServiceBusReceiveLinkProcessor#onNext
 @Override
public void onNext(ServiceBusReceiveLink next) {
    //... check linkProcessor 

    LOGGER.atInfo()
        .addKeyValue(LINK_NAME_KEY, linkName)
        .addKeyValue(ENTITY_PATH_KEY, entityPath)
        .log("Setting next AMQP receive link.");

    //... set subscription to receive messages.

    checkAndAddCredits(next);
    disposeReceiver(oldChannel);

    if (oldSubscription != null) {
        oldSubscription.dispose();
    }
}
```

We will call `checkAndAddCredits()` to add credits for the new link `next`. **However, the `next` link is close for some reason (we need to figure it out), so the `checkAndAddCredits()` throw the "Cannot add credits to closed link" error.** And this error is escaped from `onNext()` function, which breaks the reactor chain with the `onErrorDropped` error. 

Because there is no credits flow to the server side and the reactor chain is broken. The processor stop receiving messages and can't recover.
 
### Verify the behavior when an error is thrown from checkAndAddCredits()

We only throw an error for the scenario that we add credits for the new link:

```Java
//#ServiceBusReceiveLinkProcessor#checkAndAddCredits
private void checkAndAddCredits(AmqpReceiveLink link) {
    ...
    synchronized (lock) {
        final int linkCredits = link.getCredits();
        final int credits = getCreditsToAdd(linkCredits);
        if (credits > 0) {
            counter.getAndIncrement();
            if(counter.get() == 1) {
                throw new IllegalStateException("Can't add credits"); 
            } else {
                link.addCredits(credits).subscribe();
            }
        }
}
```

```LOG
17:01:53.995 [reactor-executor-1] ERROR reactor.core.publisher.Operators - Operator called default onErrorDropped
java.lang.IllegalStateException: Can't add credits
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.checkAndAddCredits(ServiceBusReceiveLinkProcessor.java:579)
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.onNext(ServiceBusReceiveLinkProcessor.java:273)
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.onNext(ServiceBusReceiveLinkProcessor.java:49)
	at reactor.core.publisher.FluxRepeatPredicate$RepeatPredicateSubscriber.onNext(FluxRepeatPredicate.java:85)
	at reactor.core.publisher.MonoPeekTerminal$MonoTerminalPeekSubscriber.onNext(MonoPeekTerminal.java:180)
	at reactor.core.publisher.SerializedSubscriber.onNext(SerializedSubscriber.java:99)
	at reactor.core.publisher.FluxRetryWhen$RetryWhenMainSubscriber.onNext(FluxRetryWhen.java:173)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onNext(FluxOnErrorResume.java:79)
	at reactor.core.publisher.FluxPeekFuseable$PeekFuseableSubscriber.onNext(FluxPeekFuseable.java:210)
	at reactor.core.publisher.Operators$MonoSubscriber.complete(Operators.java:1784)
	at reactor.core.publisher.MonoFlatMap$FlatMapInner.onNext(MonoFlatMap.java:249)
	at reactor.core.publisher.Operators$MonoSubscriber.complete(Operators.java:1784)
	at reactor.core.publisher.MonoFlatMap$FlatMapInner.onNext(MonoFlatMap.java:249)
	at reactor.core.publisher.FluxMap$MapSubscriber.onNext(FluxMap.java:120)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onNext(FluxOnErrorResume.java:79)
	at reactor.core.publisher.Operators$MonoSubscriber.complete(Operators.java:1784)
	at reactor.core.publisher.MonoIgnoreThen$ThenAcceptInner.onNext(MonoIgnoreThen.java:305)
	at reactor.core.publisher.MonoCreate$DefaultMonoSink.success(MonoCreate.java:160)
	at com.azure.core.amqp.implementation.ReactorSession.lambda$createConsumer$11(ReactorSession.java:402)
	at com.azure.core.amqp.implementation.handler.DispatchHandler.onTimerTask(DispatchHandler.java:32)
	at com.azure.core.amqp.implementation.ReactorDispatcher$WorkScheduler.run(ReactorDispatcher.java:207)
```
Although the error entrance is different, (here is start from ReactorSession to create a consumer link), we can still verify the processor stop receiving message when `checkAndAddCredits` throw out an error inside `onNext()`.

### Mitigate the problem

- A Simple Solution  

Surround `checkAndAddCredits()` or all the calls inside `onNext` with try-catch . If any error occurs, call `onError` to retry/restart processor.

```Java
try {
    checkAndAddCredits(next);
} catch (Exception e) {
    LOGGER.warning("Add credits to link failure");
    currentLink = null;
    onError(e);
}
```

### Why next link is closed when we try to add credits on it? (TODO)
