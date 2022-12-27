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

```Java
//#ServiceBusReceiveLinkProcessor#checkAndAddCredits
private void checkAndAddCredits(AmqpReceiveLink link) {
   //... calculate credits
   link.addCredits(credits).subscribe();
}
```

We will call `checkAndAddCredits()` to add credits for the new link `next`. **However, the `next` link is close for some reason (we need to figure it out), so the when we try to add credit on link, it throw out the "Cannot add credits to closed link" error.**. As we haven't give a error consumer for `addCredit()` in subscriber, reactor will catch that error, and log it as `onErrorDropped` error. 

Because there is no credits flow to the server side, the processor stop receiving messages.
 
### Verify the behavior when an error is thrown from addCredits()

We only throw an error for the scenario that we add credits for the new link, and don't catch that in subscriber:

```Java
//#ServiceBusReceiveLinkProcessor#checkAndAddCredits
    private void checkAndAddCredits(AmqpReceiveLink link) {
        if (link == null) {
            return;
        }

        synchronized (lock) {
            final int linkCredits = link.getCredits();
            final int credits = getCreditsToAdd(linkCredits);
            if (credits > 0) {
                counter.getAndIncrement();
                if(counter.get() == 1) {
                    //Here mock the link closed error
                    Mono.error(new RuntimeException("Can't add credits")).subscribe();
                } else {
                    link.addCredits(credits).subscribe();
                }
            }
        }
    }
```

```LOG
14:55:59.075 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReceiveLinkProcessor - {"az.sdk.message":"Adding credits.","prefetch":0,"requested":1,"linkCredits":0,"expectedTotalCredit":1,"queuedMessages":0,"creditsToAdd":1,"messageQueueSize":0}
14:55:59.075 [reactor-executor-1] ERROR reactor.core.publisher.Operators - Operator called default onErrorDropped
reactor.core.Exceptions$ErrorCallbackNotImplemented: java.lang.RuntimeException: Can't add credits
Caused by: java.lang.RuntimeException: Can't add credits
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.checkAndAddCredits(ServiceBusReceiveLinkProcessor.java:585)
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.onNext(ServiceBusReceiveLinkProcessor.java:276)
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.onNext(ServiceBusReceiveLinkProcessor.java:52)
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
	at org.apache.qpid.proton.reactor.impl.SelectableImpl.readable(SelectableImpl.java:118)
	at org.apache.qpid.proton.reactor.impl.IOHandler.handleQuiesced(IOHandler.java:61)
	at org.apache.qpid.proton.reactor.impl.IOHandler.onUnhandled(IOHandler.java:390)
	at org.apache.qpid.proton.engine.BaseHandler.onReactorQuiesced(BaseHandler.java:87)
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:206)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:292)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
14:55:59.075 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReactorAmqpConnection - {"az.sdk.message":"Get or create consumer.","entityPath":"test-topic/subscriptions/test-subscription"}
```

Although the error entrance is different, (here is start from ReactorSession to create a consumer link), we can still verify the processor stop receiving message when `addCredits()` throw out an error and logged by reactor.

Note: If we explictly throw an error directly inside `checkAndAddCredits`, we also get the similar error, but it doesn't have `ErrorCallbackNotImplemented`, as the error was throw out the `ServiceBusReceiveLinkProcessor#onNext()`. 

```LOG
17:01:53.995 [reactor-executor-1] ERROR reactor.core.publisher.Operators - Operator called default onErrorDropped
java.lang.IllegalStateException: Can't add credits
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.checkAndAddCredits(ServiceBusReceiveLinkProcessor.java:579)
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.onNext(ServiceBusReceiveLinkProcessor.java:273)
	at com.azure.messaging.servicebus.implementation.ServiceBusReceiveLinkProcessor.onNext(ServiceBusReceiveLinkProcessor.java:49)
	at reactor.core.publisher.FluxRepeatPredicate$RepeatPredicateSubscriber.onNext(FluxRepeatPredicate.java:85)
```
### Mitigate the problem

- A Simple Solution  

Add a error handler in the subscriber of `addCredit()`


### Why next link is closed when we try to add credits on it? 

It is possible that link is closing, but cached in session. So session pass a closing link down to the linkProcessor. We need to double check the link status before pass down to linkProcessor's `onNext()`.

From logs, we could see when linkProcessor request a new link, it got a closed link first, then we create new link. Something wrong with the reactor chain.

```
11:40:26.066 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"Could not emit deliveries.close when closing handler.","connectionId":"MF_bb2c2f_1672112412439","entityPath":"test-topic/subscriptions/test-subscription","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498"}
11:40:26.067 [reactor-executor-1] INFO  c.a.c.a.i.ReactorSession - {"az.sdk.message":"Complete. Removing receive link.","connectionId":"MF_bb2c2f_1672112412439","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","entityPath":"test-topic/subscriptions/test-subscription"}
11:40:26.072 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - Receive link endpoint states are closed. Requesting another.
11:40:26.072 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - Requesting a new AmqpReceiveLink from upstream.
11:40:26.073 [reactor-executor-1] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - {"az.sdk.message":"Created consumer for Service Bus resource.","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","entityPath":"test-topic/subscriptions/test-subscription","mode":"PEEK_LOCK","isSessionEnabled":false,"entityType":"SUBSCRIPTION"}
11:40:26.073 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - {"az.sdk.message":"Setting next AMQP receive link.","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","entityPath":"test-topic/subscriptions/test-subscription"}
11:40:26.075 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReceiveLinkProcessor - {"az.sdk.message":"Adding credits.","prefetch":0,"requested":1,"linkCredits":1,"expectedTotalCredit":1,"queuedMessages":0,"creditsToAdd":0,"messageQueueSize":0}
11:40:26.076 [boundedElastic-5] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - Receive link endpoint states are closed. Requesting another.
11:40:26.077 [boundedElastic-5] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - Requesting a new AmqpReceiveLink from upstream.
11:40:26.077 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReactorAmqpConnection - {"az.sdk.message":"Get or create consumer.","entityPath":"test-topic/subscriptions/test-subscription"}
11:40:26.077 [reactor-executor-1] DEBUG c.a.c.a.i.AzureTokenManagerProvider - {"az.sdk.message":"Creating new token manager.","audience":"amqp://servicebusliuku.servicebus.windows.net/test-topic/subscriptions/test-subscription","resource":"test-topic/subscriptions/test-subscription"}
11:40:26.079 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_bb2c2f_1672112412439","linkName":"cbs","messageId":null}
11:40:26.079 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkLocalClose","connectionId":"MF_bb2c2f_1672112412439","errorCondition":null,"errorDescription":null,"linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","entityPath":"test-topic/subscriptions/test-subscription"}
11:40:26.082 [reactor-executor-1] INFO  c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkFinal","connectionId":"MF_bb2c2f_1672112412439","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","entityPath":"test-topic/subscriptions/test-subscription"}
11:40:26.085 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_bb2c2f_1672112412439","linkName":"cbs","unsettled":0,"credits":98}
11:40:26.298 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_bb2c2f_1672112412439","linkName":"cbs","messageId":"2"}
11:40:26.298 [reactor-executor-1] INFO  c.a.c.a.i.ActiveClientTokenManager - {"az.sdk.message":"Scheduling refresh token task.","scopes":"amqp://servicebusliuku.servicebus.windows.net/test-topic/subscriptions/test-subscription"}
11:40:26.299 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_bb2c2f_1672112412439","errorCondition":null,"errorDescription":null,"entityPath":"$cbs","linkName":"cbs","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
11:40:26.300 [reactor-executor-1] INFO  c.a.c.a.i.ReactorSession - {"az.sdk.message":"Creating a new receiver link.","connectionId":"MF_bb2c2f_1672112412439","sessionName":"test-topic/subscriptions/test-subscription","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498"}
11:40:26.300 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"State UNINITIALIZED","connectionId":"MF_bb2c2f_1672112412439","entityPath":"test-topic/subscriptions/test-subscription","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498"}
11:40:26.300 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"Token refreshed.","connectionId":"MF_bb2c2f_1672112412439","entityPath":"test-topic/subscriptions/test-subscription","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","response":"ACCEPTED"}
11:40:26.301 [reactor-executor-1] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - {"az.sdk.message":"Created consumer for Service Bus resource.","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","entityPath":"test-topic/subscriptions/test-subscription","mode":"PEEK_LOCK","isSessionEnabled":false,"entityType":"SUBSCRIPTION"}
11:40:26.301 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - {"az.sdk.message":"Setting next AMQP receive link.","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","entityPath":"test-topic/subscriptions/test-subscription"}
11:40:26.301 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReceiveLinkProcessor - {"az.sdk.message":"Adding credits.","prefetch":0,"requested":1,"linkCredits":0,"expectedTotalCredit":1,"queuedMessages":0,"creditsToAdd":1,"messageQueueSize":0}
11:40:26.304 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReactorAmqpConnection - {"az.sdk.message":"Get or create consumer.","entityPath":"test-topic/subscriptions/test-subscription"}
11:40:26.304 [reactor-executor-1] INFO  c.a.c.a.i.ReactorSession - {"az.sdk.message":"Returning existing receive link.","connectionId":"MF_bb2c2f_1672112412439","linkName":"test-topic/subscriptions/test-subscription_735ba0_1672112412498","entityPath":"test-topic/subscriptions/test-subscription"}
```