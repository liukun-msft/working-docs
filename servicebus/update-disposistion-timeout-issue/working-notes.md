# Service Bus Processor update disposition timeout error

### Descritption

User encountered `Update disposition request timed out` error while setting `maxConcurrentCalls = 50` and `prefetchCount = 80`.

```
14:01:04.441 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_82b18a_1660543257822","errorCondition":null,"errorDescription":null,"entityPath":"testqueue","linkName":"testqueue_d5aa87_1660543257878","updatedLinkCredit":63,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
14:02:01.107 [parallel-14] DEBUG c.a.m.s.i.ServiceBusReactorReceiver - linkName[testqueue_d5aa87_1660543257878]: Cleaning timed out update work tasks.
14:03:01.112 [parallel-14] DEBUG c.a.m.s.i.ServiceBusReactorReceiver - linkName[testqueue_d5aa87_1660543257878]: Cleaning timed out update work tasks.
14:03:01.112 [parallel-14] ERROR c.a.m.s.i.ServiceBusReactorReceiver - Update disposition request timed out., errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue, REFERENCE_ID: testqueue_d5aa87_1660543257878, LINK_CREDIT: 63]
com.azure.core.amqp.exception.AmqpException: Update disposition request timed out., errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue, REFERENCE_ID: testqueue_d5aa87_1660543257878, LINK_CREDIT: 63]
	at com.azure.messaging.servicebus.implementation.ServiceBusReactorReceiver.lambda$cleanupWorkItems$17(ServiceBusReactorReceiver.java:368)
	at java.base/java.util.concurrent.ConcurrentHashMap.forEach(ConcurrentHashMap.java:1603)
	at com.azure.messaging.servicebus.implementation.ServiceBusReactorReceiver.cleanupWorkItems(ServiceBusReactorReceiver.java:359)
	at com.azure.messaging.servicebus.implementation.ServiceBusReactorReceiver.lambda$new$0(ServiceBusReactorReceiver.java:95)
	at reactor.core.publisher.LambdaSubscriber.onNext(LambdaSubscriber.java:160)
	at reactor.core.publisher.FluxInterval$IntervalRunnable.run(FluxInterval.java:125)
	at reactor.core.scheduler.PeriodicWorkerTask.call(PeriodicWorkerTask.java:59)
	at reactor.core.scheduler.PeriodicWorkerTask.run(PeriodicWorkerTask.java:73)
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
	at java.base/java.util.concurrent.FutureTask.runAndReset(FutureTask.java:305)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:305)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
```

Environment:

- service bus version 7.10.0
- AKS pod with **one processor**

This error only happen when update disposition task reach timeout and is cleaned up by a schedule task. Something must stuck while processing the ack disposition, because we have received an ack response.

```
[1262869450:0] <- Disposition{role=SENDER, first=295, last=null, settled=true, state=Accepted{}, batchable=false}
[1262869450:0] <- Disposition{role=SENDER, first=292, last=null, settled=true, state=Accepted{}, batchable=false}
[1262869450:0] <- Disposition{role=SENDER, first=298, last=null, settled=true, state=Accepted{}, batchable=false}
```

### Reproduce

Processor Client
```Java
ServiceBusProcessorClient processorClient = new ServiceBusClientBuilder()
        .connectionString(Credentials.serviceBusConnectionString)
        .processor()
        .queueName(Credentials.serviceBusQueue)
        .processMessage(processMessage)
        .processError(processError)
        .prefetchCount(80)
        .maxConcurrentCalls(50)
        .maxAutoLockRenewDuration(Duration.ofSeconds(0))
        .disableAutoComplete()
        .buildProcessorClient();
```

Process message 
```Java
Consumer<ServiceBusReceivedMessageContext> processMessage = messageContext -> {
    try {
        System.out.println(messageContext.getMessage().getMessageId());
        
        //Sleep 1 second to mock process progress
        TimeUnit.SECONDS.sleep(1);
        
        messageContext.complete();
    } catch (Exception ex) {
        messageContext.abandon();
    }
};
```

Important step: **add vm option: -Dreactor.schedulers.defaultBoundedElasticSize=50**

Users add this vm option to increase the boundedElasticSize on the AKS pod. if they do not explicitly add this option, the default BoundedElasticSize will be `10 * number of processors`, i.e. 10.

```Java
public static final int DEFAULT_BOUNDED_ELASTIC_SIZE = (Integer)Optional.ofNullable(
    System.getProperty("reactor.schedulers.defaultBoundedElasticSize")).map(Integer::parseInt)
.orElseGet(() -> {
    return 10 * Runtime.getRuntime().availableProcessors();
});
```

### Reason

The ServiceBusProcessorClient hangs because we have other tasks running on the `Schedulers.boundedElastic` thread pool in addition to the receive message tasks. **When we limit the size of the thread pool to the concurrency number**, it gets stuck when all threads are used in processing messages, this is because it can't create new thread for other tasks. Therefore, we need to set defaultBoundedElasticSize to a value greater than the concurrency. (I change it to at least 54 and solve the problem.)

Below are some places we use Schedulers.boundedElastic pool:

- processor client health monitor
    ```Java
    //ServiceBusProcessorClient#start
    monitorDisposable = Schedulers.boundedElastic().schedulePeriodically(() -> {
        if (this.asyncClient.get().isConnectionClosed()) {
            restartMessageReceiver(null);
        }
    }, SCHEDULER_INTERVAL_IN_SECONDS, SCHEDULER_INTERVAL_IN_SECONDS, TimeUnit.SECONDS);
    ```

- process message (maxConcurrentCalls > 1)
    ```Java
    //ServiceBusProcessorClient#receiveMessages
    receiveFlux.parallel(processorOptions.getMaxConcurrentCalls(), 1)
        .runOn(Schedulers.boundedElastic(), 1)
        .subscribe(subscribers);
    ````
- receive message (receive from ReactorReceiver, reactor-executor thread)
    ```Java
    //ServiceBusReactorReceiver
    public Flux<Message> receive() {
        return super.receive()
            .filter(message -> message != EMPTY_MESSAGE)
            .publishOn(Schedulers.boundedElastic());
    }
    ```
- receive message (receive from ServiceBusReactorReceiver, Schedulers.boundedElastic() thread)
    ```Java
    //ServiceBusReceiveLinkProcessor
    next.receive().publishOn(Schedulers.boundedElastic()).subscribe(...)
    ```

- create receive link
    ```Java
    //ServiceBusReceiveLinkProcessor
    next.getEndpointStates().subscribeOn(Schedulers.boundedElastic()).subscribe(...)
    ```


**Why are no threads completed and release thread resources to the pool?**

This is probably because a thread running on "Schedulers.boundedElastic" may create another task that also requires a thread from "Schedulers.boundedElastic" and there are no threads in the pool. This is like a deadlock, holding a thread and trying to get another thread from the pool. As a result, no threads will be completed and the thread resources released, and then the client is hung for a timeout.

**Thinking more**

Some users may not configure for defaultBoundedElasticSize, so the default value is `10 * number of processors`, which will be a limit to their maxConcurrentCalls. Maybe we need to create our own boundedElastic pool based on maxConcurrentCalls and maxConcurrentSessions.