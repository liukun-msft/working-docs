# Message lock expired issue

## Issue details

 An application with > 1 ServiceBus receiver queue and processor and send enough messages to one queue to keep it busy for > Server-side lock timeout, and during that time also send a message to one of the other queues such that once the first queue has finished processing the server lock timer on the second queue has expired.
 
**ERROR LOG**
> ERROR c.a.m.s.i.ServiceBusReactorReceiver - The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:...


Issue link: https://github.com/Azure/azure-sdk-for-java/issues/25021

Related issue summary: https://github.com/Azure/azure-sdk-for-java/issues/25259


## Anaylsis Reason

This error is thrown from ServiceBusReactorReceiver.java when get delivery remote state is Reject.
``` Java
private void updateOutcome(String lockToken, Delivery delivery) {
    DeliveryState remoteState = delivery.getRemoteState(); 
    //remoteState is Reject
    //remoteState detail is Error{condition=com.microsoft:message-lock-lost, description='The lock supplied is invalid. Either the lock expired...
    ...
```

And in AmqpErrorCondition.java describe this error condition:

```Java
    /**
     * Error condition when receiver attempts {@code complete}, {@code abandon}, {@code renewLock}, {@code deadLetter},
     * or {@code defer} on a peek-locked message whose lock had already expired.
     */
    MESSAGE_LOCK_LOST("com.microsoft:message-lock-lost"),
```

 Each message will have a peek-lock to prevent receiver to fetch new messages. But the lock have a expired time. 

**Once the lock expired time passed, if receiver are trying to complete that expired message, it will throw this error.**

For service bus processor, it will enable auto refresh lock by default and maxAutoLockRenewDuration is 5 minutes. If proccess time is over max duration, the lock will expired. Processor will fetch new message after lock expired.


## Reproduce issue

### Could not reproduce issue using two queue and one queue is busy but just complete message

Build test project base on Micronuant framework:

ServiceBusConfiguration.java

```Java
@Factory
public class ServiceBusConfiguration {

    @Inject
    private ServiceBusBroker serviceBusBroker;

    private final Consumer<ServiceBusReceivedMessageContext> processMessage = messageContext -> {
        try {
            System.out.println("message id:" + messageContext.getMessage().getMessageId());
            messageContext.complete();
        } catch (Exception ex) {
            messageContext.abandon();
        }
    };

    private final Consumer<ServiceBusErrorContext> processError = errorContext -> {
        System.err.println("Error occurred while receiving message: " + errorContext.getException());
    };

    @Singleton
    @Named("serviceBusProcessorClient1")
    public ServiceBusProcessorClient serviceBusProcessorClient1(){
        String queue1 = "test1";
        return serviceBusBroker.getServiceBusClientBuilder()
                .processor()
                .queueName(queue1)
                .processMessage(processMessage)
                .processError(processError)
                .disableAutoComplete()
                .buildProcessorClient();

    }

    @Singleton
    @Named("serviceBusProcessorClient2")
    public ServiceBusProcessorClient serviceBusProcessorClient2(){
        String queue2 = "test2";
        return serviceBusBroker.getServiceBusClientBuilder()
                .processor()
                .queueName(queue2)
                .receiveMode(ServiceBusReceiveMode.PEEK_LOCK)
                .processMessage(processMessage)
                .processError(processError)
                .disableAutoComplete()
                .buildProcessorClient();
    }
}
```

ServiceBusBroker.java
```Java 
@Singleton
public class ServiceBusBroker {

    private final ServiceBusClientBuilder serviceBusClientBuilder;

    public ServiceBusBroker(@Value("${connectString}") String connectString){
        serviceBusClientBuilder = new ServiceBusClientBuilder();
        serviceBusClientBuilder.connectionString(connectString);
    }

    public ServiceBusClientBuilder getServiceBusClientBuilder(){
        return serviceBusClientBuilder;
    }
}

```
Application.java

```Java
public class Application {
    public static void main(String[] args) {
        ApplicationContext ctx = Micronaut.run(Application.class, args);
        ServiceBusProcessorClient serviceBusProcessorClient1 = ctx.getBean(ServiceBusProcessorClient.class, Qualifiers.byName("serviceBusProcessorClient1"));
        serviceBusProcessorClient1.start();
        ServiceBusProcessorClient serviceBusProcessorClient2 = ctx.getBean(ServiceBusProcessorClient.class, Qualifiers.byName("serviceBusProcessorClient2"));
        serviceBusProcessorClient2.start();
    }
}
```

application.yml
```yaml
connectString: XXXX
```

### Reproduce issue when increasing the processing time > maxAutoLockRenewDuration

Add `sleep(6)` in `processMssage`, and because it will wait 6 mins to complete message, the lock will expired at that time if not set `maxAutoLockRenewDuration` (Default 5 mins).

```Java
  private final Consumer<ServiceBusReceivedMessageContext> processMessage = messageContext -> {
        try {
            System.out.println("message id:" + messageContext.getMessage().getMessageId());
            TimeUnit.MINUTES.sleep(6);
            messageContext.complete();
        } catch (Exception ex) {
            messageContext.abandon();
        }
    };
```

Console log (using service bus 7.5.2):

```
16:24:20.765 [parallel-2] INFO  c.a.m.s.LockRenewalOperation - token[a4139312-172c-459a-97f7-8e19f04548b7]. now[2022-02-10T16:24:20.765508500+08:00]. Starting lock renewal.

...

16:24:21.657 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[a4139312-172c-459a-97f7-8e19f04548b7]. nextExpiration[2022-02-10T08:24:50.930Z]. next: [PT29.2724659S]. isSession[false]
16:24:21.657 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[4490ea8a-cf3c-4cc7-8a66-afc134ea972f]. nextExpiration[2022-02-10T08:24:50.930Z]. next: [PT29.2724659S]. isSession[false]
16:24:21.956 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[f7c1bbac-17d5-41b1-b8e4-7a4400238be0]. nextExpiration[2022-02-10T08:24:51.227Z]. next: [PT29.2714914S]. isSession[false]

...

16:28:55.842 [parallel-16] INFO  c.a.m.s.LockRenewalOperation - token[a4139312-172c-459a-97f7-8e19f04548b7]. now[2022-02-10T16:28:55.842613500+08:00]. Starting lock renewal.
16:28:56.138 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[a4139312-172c-459a-97f7-8e19f04548b7]. nextExpiration[2022-02-10T08:29:25.408Z]. next: [PT29.2694388S]. isSession[false]
16:28:56.152 [parallel-2] INFO  c.a.m.s.LockRenewalOperation - token[f7c1bbac-17d5-41b1-b8e4-7a4400238be0]. now[2022-02-10T16:28:56.152362400+08:00]. Starting lock renewal.
16:28:56.447 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[f7c1bbac-17d5-41b1-b8e4-7a4400238be0]. nextExpiration[2022-02-10T08:29:25.705Z]. next: [PT29.2584381S]. isSession[false]
16:30:02.131 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReactorReceiver - Received delivery 'a4139312-172c-459a-97f7-8e19f04548b7' state 'Rejected{error=Error{condition=com.microsoft:message-lock-lost, description='The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:6c84b83e-7386-46e0-88db-1bdcbbadbd1c, TrackingId:3b9f634f0000003c002124c46204cba0_G18_B27, SystemTracker:servicebusliuku:Queue:test1, Timestamp:2022-02-10T08:30:01', info=null}}' doesn't match expected state 'Accepted{}'
16:30:02.131 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReactorReceiver - deliveryTag[a4139312-172c-459a-97f7-8e19f04548b7], state[{}]. Retry attempts exhausted.
16:30:02.131 [reactor-executor-1] ERROR c.a.m.s.i.ServiceBusReactorReceiver - The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:6c84b83e-7386-46e0-88db-1bdcbbadbd1c, TrackingId:3b9f634f0000003c002124c46204cba0_G18_B27, SystemTracker:servicebusliuku:Queue:test1, Timestamp:2022-02-10T08:30:01, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: test1, REFERENCE_ID: test1_4ee224_1644481437964, LINK_CREDIT: 0]
16:30:02.146 [boundedElastic-2] WARN  c.a.m.s.i.ServiceBusReactorReceiver - entityPath[test1], linkName[test1_4ee224_1644481437964], deliveryTag[a4139312-172c-459a-97f7-8e19f04548b7]. Delivery not found to update disposition.
16:30:02.146 [boundedElastic-2] ERROR c.a.m.s.i.ServiceBusReactorReceiver - Delivery not on receive link.
```
The refresh lock log are only in 5 mins(16:24:20.765 ~ 16:28:56.136) and after 6 mins (16:30:02.131), it throw out this error when application try to complete.

If we have another separated queue, the proccess message was running on other thread, so the other queue can still work fine. (need to check whether both process message have some common resource).

## Mitigate issue

1. Increase `maxAutoLockRenewDuration`

2. Upgrade Service Bus to latest version 7.5.2 (Include bug fix for auto refresh lock)
