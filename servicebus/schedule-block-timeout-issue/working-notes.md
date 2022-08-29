## Schedule Message Timeout Error

### Background

User use the shared connection for sender and receiver. They receive messages from the queue and schedule new message to same queue when received message met some conditions.

They encounted below schedule timeout error inside the receiver process message function:

```Java
java.lang.IllegalStateException: Timeout on blocking read for 245600000000 NANOSECONDS
	at reactor.core.publisher.BlockingSingleSubscriber.blockingGet(BlockingSingleSubscriber.java:123)
	at reactor.core.publisher.Mono.block(Mono.java:1731)
	at com.azure.messaging.servicebus.ServiceBusSenderClient.scheduleMessage(ServiceBusSenderClient.java:274)
```

They also met some other errors like:

```Java
com.azure.core.amqp.exception.AmqpException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. 
	at com.azure.core.amqp.implementation.ExceptionUtil.toException(ExceptionUtil.java:85)
	at com.azure.messaging.servicebus.implementation.ServiceBusReactorReceiver.updateOutcome(ServiceBusReactorReceiver.java:310)
	at com.azure.messaging.servicebus.implementation.ServiceBusReactorReceiver.decodeDelivery(ServiceBusReactorReceiver.java:228)
	at com.azure.core.amqp.implementation.ReactorReceiver.lambda$new$0(ReactorReceiver.java:86)
	at com.azure.core.amqp.implementation.handler.DispatchHandler.onTimerTask(DispatchHandler.java:34)
	at com.azure.core.amqp.implementation.ReactorDispatcher$WorkScheduler.run(ReactorDispatcher.java:202)
	at org.apache.qpid.proton.reactor.impl.SelectableImpl.readable(SelectableImpl.java:118)
	at org.apache.qpid.proton.reactor.impl.IOHandler.handleQuiesced(IOHandler.java:61)
	at org.apache.qpid.proton.reactor.impl.IOHandler.onUnhandled(IOHandler.java:390)
	at com.azure.core.amqp.implementation.handler.CustomIOHandler.onUnhandled(CustomIOHandler.java:43)
	at org.apache.qpid.proton.engine.BaseHandler.onReactorQuiesced(BaseHandler.java:87)
```

```Java
Transient error occurred. Attempt: 1. Retrying after 4511 ms.
The link 'G19:117895518:testqueue_0e1e31_1661328716628' is force detached. Code: consumer(link60049597). Details: AmqpMessageConsumer.IdleTimerExpired: Idle timeout: 00:10:00. TrackingId:db7bf3c800000040039448bd6305dd4f_G19_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T08:22:41, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue, REFERENCE_ID: testqueue_0e1e31_1661328716628, LINK_CREDIT: 0]
com.azure.core.amqp.exception.AmqpException: The link 'G19:117895518:testqueue_0e1e31_1661328716628' is force detached. Code: consumer(link60049597). Details: AmqpMessageConsumer.IdleTimerExpired: Idle timeout: 00:10:00. TrackingId:db7bf3c800000040039448bd6305dd4f_G19_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T08:22:41, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue, REFERENCE_ID: testqueue_0e1e31_1661328716628, LINK_CREDIT: 0]
	at com.azure.core.amqp.implementation.ExceptionUtil.toException(ExceptionUtil.java:85)
	at com.azure.core.amqp.implementation.handler.LinkHandler.handleRemoteLinkClosed(LinkHandler.java:111)
	at com.azure.core.amqp.implementation.handler.LinkHandler.onLinkRemoteClose(LinkHandler.java:59)
	at com.azure.core.amqp.implementation.handler.ReceiveLinkHandler.onLinkRemoteClose(ReceiveLinkHandler.java:219
```

Warning messsage:

```Java
[reactor-executor-1] WARN  c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Received delivery without pending message.","connectionId":"MF_e42b54_1661330246759","linkName":"testqueue-mgmt","messageId":"2"}
```

### Reason 

User misconfigure the queue name as the topic name of sender client. 

```Java
sender = builder.sender()
        .topicName(Credentials.serviceBusQueue)
        .buildClient();

processor = builder.processor()
        .queueName(Credentials.serviceBusQueue)
        .disableAutoComplete()
        .processMessage(processMessage)
        .processError(processError)
        .buildProcessorClient();
```

***How we notice that from logs***

The management node are created twice:

```Java
15:51:19.057 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReactorAmqpConnection - Creating management node. entityPath: [testqueue]. address: [testqueue/$management]. linkName: [testqueue-mgmt]
15:51:19.351 [reactor-executor-1] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Emitting new response channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management","linkName":"testqueue-mgmt"}
15:51:19.351 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Setting next AMQP channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:19.351 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Next AMQP channel received, updating 0 current subscribers","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:19.663 [reactor-executor-1] INFO  c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkRemoteOpen","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt:sender","entityPath":"testqueue/$management","remoteTarget":"Target{address='testqueue/$management', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, capabilities=null}"}
15:51:19.664 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Channel is now active.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}

... 

15:51:37.983 [parallel-10] INFO  c.a.m.s.i.ServiceBusReactorAmqpConnection - Creating management node. entityPath: [testqueue]. address: [testqueue/$management]. linkName: [testqueue-mgmt]
15:51:37.986 [parallel-10] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Emitting new response channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management","linkName":"testqueue-mgmt"}
15:51:37.987 [parallel-10] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Setting next AMQP channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:37.987 [parallel-10] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Next AMQP channel received, updating 0 current subscribers","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:50.646 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Channel is now active.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
```

***Why we see so many different errors***

If we use a queue name to set topic name for sender, **the sender can send message successfully as client communicate with the service via entity name and don't need entity type(i.e. queue/topic)**. 

However, if we use a shared connection for a sender and a receiver, the incorrectly configuration may cause issues as the **service bus client use both entity name and entity type to create or get management channel.**

```Java
//ServiceBusReactorAmqpConnection
public Mono<ServiceBusManagementNode> getManagementNode(String entityPath, MessagingEntityType entityType) 
```

If we configure two different entity types for one entity name, the client will create two management channels: one for receiver and one for sender. Because the shared connection can only use one management channel to receive service response, if receiver's management channel override the sender's management channel, the schedule message response is received by the receiver's management channel and we can see "Received delivery without pending message." log warning. Because the response can't be consumed by sender's management channel, the sender get stuck until it throw out the schedule message timeout error. 

![img](./schedule-timeout-issue.png)

- When sender schedule message, it will create management channel 1. When receiver renew message lock, it will create management channel 2. 
- If receiver process message fast, and don't need renew lock, then no issue, that is why this error sometimes happened.
- After two managment channel created, sender is still using management channel 1 to schedule message, but the management send link and receive link in the shared connection is replaced by management channel 2. Because the schedule message are saved in `unconfirmedSends` map inside management channel 1, so when management channel 2 receive the ack response, it can't find in its `unconfirmedSends` map and couldn't settle the schedule message.

Depends on the process message logic and time, the sender's management channel can be posible to override the receiver's management channel, and in this scenario, because of the renew lock failure, some lock expired errors will be thrown out. We will also see the link remote detached message and transient error 10 minutes after all lock renew task clean up.

### Reproduce

```Java
public class ReceiveAndScheduleMessage {
    private static ServiceBusSenderClient sender;
    private static ServiceBusProcessorClient processor;
    private static int scheduleTimes = 0;

    private static void scheduleMessage() {
        System.out.println("Start schedule message.");
        ServiceBusMessage messages = new ServiceBusMessage("Scheduled Message");
        scheduleTimes++;

        OffsetDateTime scheduleTime = OffsetDateTime.now().plusSeconds(30);
        sender.scheduleMessage(messages, scheduleTime);
        System.out.println("Sent scheduled message.");
    }

    private static final Consumer<ServiceBusReceivedMessageContext> processMessage = messageContext -> {
        System.out.println("Received message with id: " + messageContext.getMessage().getMessageId());

        try {
            // Scenario 1: sender schedule message first, then receiver renew lock for second message
            // Create sender's management channel first, then create receiver's management channel
            if (scheduleTimes >= 1) {
                TimeUnit.SECONDS.sleep(30);
            }
            scheduleMessage();

            // Scenario 2: renew lock for first message
            // Create receiver's management channel first, then create sender's management channel
//            TimeUnit.SECONDS.sleep(30);
//            scheduleMessage();

            // Scenario 3: sender schedule message first, then receiver renew lock before complete message
            // Create receiver's management channel first, then create sender's management channel
//            scheduleMessage();
//            TimeUnit.SECONDS.sleep(30);

        } catch (Exception e) {
            e.printStackTrace();
        }

        messageContext.complete();

        System.out.println("Finished process message.");
    };

    private static final Consumer<ServiceBusErrorContext> processError = errorContext -> {
        System.err.println("Error occurred while receiving message: " + errorContext.getException());
    };


    public static void main(String[] args) {
        ServiceBusClientBuilder builder = new ServiceBusClientBuilder()
                .connectionString(Credentials.serviceBusConnectionString);

        sender = builder.sender()
                .topicName(Credentials.serviceBusQueue)
                .buildClient();

        processor = builder
                .processor()
//                .maxConcurrentCalls(2)
                .disableAutoComplete()
                .queueName(Credentials.serviceBusQueue)
                .processMessage(processMessage)
                .processError(processError)
                .buildProcessorClient();

        processor.start();
    }
}
```
Repro Logs

- [Scenario 1](./scenario-1-log.md)
- [Scenario 2](./scenario-2-log.md)
- [Scenario 3](./scenario-3-log.md)


#### Lesson learned

Some questions need to be understood in the following order:

1. How many instances, how many clients, are they use shared connection?
2. Check their client configuration
    - queue/topic settings
    - concurrency
    - renew lock duration 
    - auto complete 
    - retry options
3. Check Log contents
    - Thread name 
    - Pod name (in case they merge all pods log in one file)
    - Time duration (+/- 10 mintues may not be enough)

4. Check their CPU cores, it may have thread pool issue. (Most of user deploy on 1 core pod)

User are not always provide the log as we want, and they may use some other tools to get logs, such as splunk, elastic search. We can give them some log format examples after they provided some log which is not we want.

#### Possible Enhancement

1. Throw an explict error when user misconfigured.

2. Remove entity type as an identitfier for management channel 


