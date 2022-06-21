## [Service Bus] Abandon message may disrupt the ordered processing of session based message 

**Issue link**: https://github.com/Azure/azure-sdk-for-java/issues/24064.

### Description

When use `SessionProcessorClient` to receive messages, say we try to receive message #1 and #2. If we abondon message #1, the next message is to be processed is message #2, not message #1. We can receive and process message #1 only after message #2 has been processed.   

The issue is found in `SessionProcessorClient` and not in `SessionReceiverClient` and `ProcessorClient`.

### Log Analysis

See repro code snippet in [issue#24064](https://github.com/Azure/azure-sdk-for-java/issues/24064) 

Add some customized logs and run with the latest sevice bus version (7.10.0-beta.1):

![img](./session-processor-logs-1.png)

From the log, we can see that when `SessionProcessor` receives the message #0, it immediately requests the next message #1 before message #0 fully processed. So actually we request 2 messages at first time. Therefore, if mesasge #0 is abandoned, we directly process message #1 as next message. After message #1 is processed, we then request abandoned message #0 again.

**The question becomes why we prefetch 2 messages? Although we use `publishOn(scheduler, prefetch = 1)` to receive message.**

### Code Analysis

For `SessionProcessor`, the `receiveClient` calls `ServiceBusSessionManager#receive()` to return a `receiveFlux` and uses `receiveFlux` to fetch messages. 

If `receiveClient` is a rolling receiver, `receiceFlux` will be construct by `Flux#merge`, which use `processor` to publish messages from different sessions. 

```Java
//ServiceBusSessionManager
Flux<ServiceBusMessageContext> receive() {
    ...
    receiveFlux = Flux.merge(processor, receiverOptions.getMaxConcurrentSessions());
    ...
    return receiveFlux;
}
```
 
 The `processor` will emit `Flux<ServiceBusMessageContext>` to receive messages when session and receive link is active. This is created by `ServiceBusSessionManager#getSession()` function.

```Java
private Flux<ServiceBusMessageContext> getSession(...){
    return getActiveLink().flatMap(link -> link.getSessionId()
        .map(...return new ServiceBusSessionReceiver(...)))
        .flatMapMany(sessionReceiver -> sessionReceiver.receive()...)
        .publishOn(scheduler, 1); //prefetch = 1
}
```
Here we set prefetch of publishOn is 1, so we only request 1 message every time try to receive.

The `sessionReceiver.receive()` will return `receivedMessageFlux`, which defines publish source is from `receiveLink.receive()` and some actions like `doOnsubscribe()`,, `doOnRequest()`, `map()` tp deserilaize message and `onErrorResume()`. 

```Java
//ServiceBusSessionReceiver
final Flux<ServiceBusMessageContext> receivedMessagesFlux = receiveLink
            .receive()
            .publishOn(scheduler)
            .doOnSubscribe(subscription -> {
              ...
            })
            .doOnRequest(request -> {  // request is of type long.
              ...
            })
            .takeUntilOther(cancelReceiveProcessor)
            .map(message -> {
              ...
            })
            .onErrorResume(error -> {
              ...
            })
            .doOnNext(context -> {
                ...
            });
```

After going though the code, the logic looks good and we prefetch only 1 message at a time. The reason may comes from reactor-core or we may wrongly use some `Flux` methods. To verify that, we can simplify the issue by only write the reactor code to represent session receiving prossess. See [Simplify receive logic](./working-notes.md#simplify-receive-logic).

### Root Cause Analysis

After debugging and testing, the issue occur when we use `Flux#publishOn` in combination with `Flux#merge`. The reactor will create a class `FluxPublishOn` to wrap `Flux#merge` as `Subscriber` to `Flux#publishOn`. And when recevived messages, the `FluxPublishOn` will call its `poll()` method to get message from queue and then pass to the downstream `onNext()` method.

```Java
//FluxPublishOn.class
public T poll() {
    T v = queue.poll();
    if (v != null && sourceMode != SYNC) {
        long p = produced + 1;
        if (p == limit) {
            produced = 0;
            s.request(p);
        }
        else {
            produced = p;
        }
    }
    return v;
}
```
For the implementation, we can see there is `p == limit` check block. When `p` reaches the `limit`, the `FluxPublishOn` will re-request `p` messages . 

Now we can explain why we receive 2 messages (#0 and #1) before processing message #0:

**When we received the first message (#0), the `p` becomes 1 and `limit` is 1 (prefetch == 1), so `s` (subscription) will reuqest one more message (#1) before passing the first message (#0) to downsteam `onNext()` to consume.**

This is an internal implementation of `FluxPublishOn`, that adds behavior that we don't expect. I still need to understand why they implement the `poll()` function in this way.

### Walkaround

Some walkaround can solve the issue, but these need to be tested to see if they bring new impact:

1. Add `map(message -> message)` after `publishOn(scheduler, 1)

2. Move `publishOn(scheduler, 1)` to `ServiceBusSessionReceiver`


Both of these walkaround will use a `OneQueue` instead of `PublishOnSubscriber` queue, so it will not call this `poll()` function to re-request `p` messages.

### Simplify receive logic

I write code which only using reactor to repro the issue. This can help to debug and fix more easily

```Java
@Test
public void testPublishOnWithMerge() throws InterruptedException {
    Scheduler scheduler = Schedulers.newBoundedElastic(5, 5, "receiver-");
    Sinks.Many<String> messageSinks = Sinks.many().replay().all();
    //Represent for receivedFlux in ServiceBusSessionReceiver
    Flux<String> receivedMessagesFlux = messageSinks.asFlux()
            .doOnSubscribe(subscription -> {
                logger.info("receivedMessagesFlux - doOnSubscribe called");
            })
            .doOnRequest(request -> {
                logger.info("receivedMessagesFlux - doOnRequest called, request number: " + request);
            })
            .doOnNext(message -> {
                logger.info("receivedMessagesFlux - Received message : " + message);
            })
            .log();

   //Represent for getSession in ServiceBusSessionManager
    Flux<String> sessionFlux = Mono.defer(() -> Mono.just("Active link 1 "))
            .map(link -> link + "On session")
            .flatMapMany(link -> receivedMessagesFlux)
            .publishOn(scheduler,1)
            .map(message -> message);

    //Represent for processor in ServiceBusSessionManager
    Sinks.Many<Flux<String>> processor = Sinks.many().replay().all();
    processor.tryEmitNext(sessionFlux);
    Flux<String> messageFlux = Flux.merge(processor.asFlux(),1);

    //Represent for messageFlux in ServiceBusReceiveAyncClient
    messageFlux.subscribe(subscriber);

    logger.info("-------------- emit messages ------------");
    messageSinks.emitNext("First Data", FAIL_FAST);
    messageSinks.emitNext("Second Data", FAIL_FAST);
    messageSinks.emitNext("Third Data", FAIL_FAST);


    TimeUnit.SECONDS.sleep(5);
}
```

Logs:
```
2022-06-21 15:26:33,717 INFO  [main] org.example.sinks.DoOnRequestTest: Subscriber - onSubscribe called
2022-06-21 15:26:33,719 INFO  [main] org.example.sinks.DoOnRequestTest: Subscriber - Requesting 1 more message onSubscribe
2022-06-21 15:26:33,730 INFO  [main] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnSubscribe called
2022-06-21 15:26:33,733 INFO  [main] reactor.Flux.PeekFuseable.1: | onSubscribe([Fuseable] FluxPeekFuseable.PeekFuseableSubscriber)
2022-06-21 15:26:33,736 INFO  [main] reactor.Flux.PeekFuseable.1: | request(1)
2022-06-21 15:26:33,739 INFO  [main] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnRequest called, request number: 1
2022-06-21 15:26:33,741 INFO  [main] org.example.sinks.DoOnRequestTest: -------------- emit messages ------------
2022-06-21 15:26:33,744 INFO  [receiver--2] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - Received message : First Data
2022-06-21 15:26:33,745 INFO  [receiver--2] reactor.Flux.PeekFuseable.1: | onNext(First Data)
2022-06-21 15:26:33,745 INFO  [receiver--1] reactor.Flux.PeekFuseable.1: | request(1)
2022-06-21 15:26:33,745 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnRequest called, request number: 1
2022-06-21 15:26:33,745 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - onNext called
2022-06-21 15:26:33,745 INFO  [receiver--2] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - Received message : Second Data
2022-06-21 15:26:33,745 INFO  [receiver--2] reactor.Flux.PeekFuseable.1: | onNext(Second Data)
2022-06-21 15:26:34,757 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Process finished: First Data
2022-06-21 15:26:34,757 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Requesting 1 more message onNext
2022-06-21 15:26:34,757 INFO  [receiver--1] reactor.Flux.PeekFuseable.1: | request(1)
2022-06-21 15:26:34,757 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnRequest called, request number: 1
2022-06-21 15:26:34,758 INFO  [receiver--2] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - Received message : Third Data
2022-06-21 15:26:34,758 INFO  [receiver--2] reactor.Flux.PeekFuseable.1: | onNext(Third Data)
2022-06-21 15:26:34,758 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - onNext called
2022-06-21 15:26:35,770 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Process finished: Second Data
2022-06-21 15:26:35,770 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Requesting 1 more message onNext
2022-06-21 15:26:35,770 INFO  [receiver--1] reactor.Flux.PeekFuseable.1: | request(1)
2022-06-21 15:26:35,770 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnRequest called, request number: 1
2022-06-21 15:26:35,770 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - onNext called
2022-06-21 15:26:36,785 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Process finished: Third Data
2022-06-21 15:26:36,785 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Requesting 1 more message onNext
```

