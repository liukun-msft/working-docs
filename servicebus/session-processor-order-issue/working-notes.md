# [Service Bus] Abandon message may disrupt the ordered processing of session based message 

## Issue 

#### Description

https://github.com/Azure/azure-sdk-for-java/issues/24064.

The issue was found in `sessionProcessor` and not found in `sessionReceiver` and `processor`.

#### Logs Analysis

Build and run the latest sevice bus version (7.10.0-beta.1) with extract logs:

![img](./session-processor-logs-1.png)

From logs, we could see when `sessionProcessor` received the no.0 message, it immediately request the next message before no. 0 message fully processed. If no.0 is abandoned, the next message fetched from receiving queue is no.1. So the `sessionProcessor` actually request 2 messages at the first time (no.0 and no.1), and when first message in queue is abandoned, the next message is not correct. The third coming message should be the correct one.

#### Root Cause Analysis

When we build `sessionProcessor`, behind, it use `receiveFlux` to receive message form different sessions.


```Java
receiveFlux = Flux.merge(processor, receiverOptions.getMaxConcurrentSessions());
```

And inside the processor, it will emit `Flux<ServiceBusMessageContext>` to receive messages when session and receive link is active. This is created by `getSession()` function

```Java
return getActiveLink().flatMap(link -> link.getSessionId()
    .map(
        ...
        return new ServiceBusSessionReceiver(link, messageSerializer, connectionProcessor.getRetryOptions(),
            receiverOptions.getPrefetchCount(), disposeOnIdle, scheduler, this::renewSessionLock,
            maxSessionLockRenewDuration);
    })))
    .flatMapMany(sessionReceiver -> sessionReceiver.receive().doFinally(signalType -> {
    ... 
        }
    }))
    .publishOn(scheduler, 1);

```

We found the issue happened when we use `Flux#publishOn` combine with `Flux#merge`. By debug we find the `publishOn` will create a `FluxPublishOn` instance and it poll message from 


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
`poll()` is to get data from queue to pass to subscriber's `onNext(T t)`;

Base on logic if `p == limit`, the `FluxPublishOn` will request extra `p`. Here when we received the first message, the `p` change to 1, and because of `limit` is 1, so `s` (subscription) reuqest 1 more message before pass first message to `onNext()`. 

So the behavior is before we consume first message, we already request to receive the second message.

#### Simplify issue using reactor

To help for eaily debug and fix, we use rare reactor code to repro this issue.

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
2022-06-20 17:26:16,191 INFO  [main] org.example.sinks.DoOnRequestTest: Subscriber - onSubscribe called
2022-06-20 17:26:16,192 INFO  [main] org.example.sinks.DoOnRequestTest: Subscriber - Requesting 1 more message onSubscribe
2022-06-20 17:26:16,208 INFO  [main] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnSubscribe called
2022-06-20 17:26:16,210 INFO  [main] reactor.Flux.PeekFuseable.1: | onSubscribe([Fuseable] FluxPeekFuseable.PeekFuseableSubscriber)
2022-06-20 17:26:16,212 INFO  [main] reactor.Flux.PeekFuseable.1: | request(1)
2022-06-20 17:26:16,217 INFO  [main] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnRequest called, request number: 1
2022-06-20 17:26:16,217 INFO  [main] org.example.sinks.DoOnRequestTest: -------------- emit messages ------------
2022-06-20 17:26:16,220 INFO  [main] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - Received message : First Data
2022-06-20 17:26:16,220 INFO  [main] reactor.Flux.PeekFuseable.1: | onNext(First Data)
2022-06-20 17:26:16,221 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - onNext called
2022-06-20 17:26:17,227 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Process finished: First Data
2022-06-20 17:26:17,227 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Requesting 1 more message onNext
2022-06-20 17:26:17,228 INFO  [receiver--1] reactor.Flux.PeekFuseable.1: | request(1)
2022-06-20 17:26:17,228 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnRequest called, request number: 1
2022-06-20 17:26:17,228 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - Received message : Second Data
2022-06-20 17:26:17,228 INFO  [receiver--1] reactor.Flux.PeekFuseable.1: | onNext(Second Data)
2022-06-20 17:26:17,228 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - onNext called
2022-06-20 17:26:18,239 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Process finished: Second Data
2022-06-20 17:26:18,239 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Requesting 1 more message onNext
2022-06-20 17:26:18,239 INFO  [receiver--1] reactor.Flux.PeekFuseable.1: | request(1)
2022-06-20 17:26:18,240 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnRequest called, request number: 1
2022-06-20 17:26:18,240 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - Received message : Third Data
2022-06-20 17:26:18,240 INFO  [receiver--1] reactor.Flux.PeekFuseable.1: | onNext(Third Data)
2022-06-20 17:26:18,240 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - onNext called
2022-06-20 17:26:19,251 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Process finished: Third Data
2022-06-20 17:26:19,251 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: Subscriber - Requesting 1 more message onNext
2022-06-20 17:26:19,251 INFO  [receiver--1] reactor.Flux.PeekFuseable.1: | request(1)
2022-06-20 17:26:19,251 INFO  [receiver--1] org.example.sinks.DoOnRequestTest: receivedMessagesFlux - doOnRequest called, request number: 1
```

