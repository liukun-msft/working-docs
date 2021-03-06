# Error occurred while Service Bus refreshing token 

## Issue details

**Error log:** 
>*com.azure.core.amqp.implementation.ActiveClientTokenManager: {reactor-executor-8} Error occurred while refreshing token that is not retriable. Not scheduling refresh task. Use ActiveClientTokenManager.authorize() to schedule task again. **audience scopes period >= 0 required but it was -1473349684000000000***



Base on the error message, the error is coming from the constructor of `FluxInterval`, which is called along the path from `ActiveClientTokenManager` in below line:

```Java
 return Flux.switchOnNext(durationSource.asFlux().map(Flux::interval))
```
[Code link](https://github.com/Azure/azure-sdk-for-java/blob/main/sdk/core/azure-core-amqp/src/main/java/com/azure/core/amqp/implementation/ActiveClientTokenManager.java#L130)

Somehow the `durationSource` emited a negative refresh interval, and `FluxInteval` received as `period`, so it would throw out this error.

```Java
FluxInterval(long initialDelay, long period, TimeUnit unit, Scheduler timedScheduler) {
    if (period < 0L) {
        throw new IllegalArgumentException("period >= 0 required but it was " + period);
    }
    ...
```

So we need to ananlysis why refresh interval is negative.

## Analysis why refresh interval is a negative value

The `ActiveClientTokenManager#authorize()` will calculate the refresh interval based on the expired time of AccessToken i.e. `expireOn` below:

```java
final Duration between = Duration.between(OffsetDateTime.now(ZoneOffset.UTC), expiresOn);
final long refreshSeconds = (long) Math.floor(between.getSeconds() * 0.9);
final long refreshIntervalMS = refreshSeconds * 1000;
```
 [Code link](https://github.com/Azure/azure-sdk-for-java/blob/8a507b4922a5498ab3b8a28bc811bbf8d2102912/sdk/core/azure-core-amqp/src/main/java/com/azure/core/amqp/implementation/ActiveClientTokenManager.java#L72-L96)

There are two reasons which may cause the refresh interval to be negative:   
1. The value of `between` would be negative when `OffsetDateTime.now` is later than `expiresOn`
2. data overflow

The first one is more likely to occur since there is no date checking for `expiresOn`. So we could test with different `expiresOn` values. 

**We find when expireOn is EpochSecond (1970-01-01), it wil make the refresh interval negative and throw out the same exception.**

Test code:
```Java
public class TestDurationCalculation {
    public static void main(String[] args) {
        //When expireOn is 1970-1-1
        long epochSeconds = Long.parseLong("0");
        OffsetDateTime expireOn = Instant.ofEpochSecond(epochSeconds).atOffset(ZoneOffset.UTC);

        Duration between = Duration.between(OffsetDateTime.now(ZoneOffset.UTC), expireOn);
        long refreshSeconds = (long)Math.floor((double)between.getSeconds() * 0.9D);
        long refreshIntervalMS = refreshSeconds * 1000;
        Duration firstInterval = Duration.ofMillis(refreshIntervalMS);

        Flux.interval(firstInterval);
    }
}

```

Console log:

```
Exception in thread "main" java.lang.IllegalArgumentException: period >= 0 required but it was -1478942978000000000
	at reactor.core.publisher.FluxInterval.<init>(FluxInterval.java:51)
	at reactor.core.publisher.Flux.interval(Flux.java:1316)
	at reactor.core.publisher.Flux.interval(Flux.java:1296)
	at reactor.core.publisher.Flux.interval(Flux.java:1256)
	at issues.IcM278069794.TestDurationCalculation.main(TestDurationCalculation.java:21)
```

The negative value here is very close (I am using `OffsetDateTime.now(ZoneOffset.UTC)`) which can prove that in when `expireOn` is  `EpochSecond`, the service will encounter this error.

 ### So how could the expired date be set as EpochSecond(1970-01-01) in AccessToken? 

**Test scenario 1:** Check when service bus are using sharedAccessSignature and somehow `expirationTimeStr` is "0", `epochSecond` will be parsed as zero which make the token expired date to be 1970-01-01. 

```Java
expirationTimeStr -> {
    try {
        long epochSeconds = Long.parseLong(expirationTimeStr);
        return Instant.ofEpochSecond(epochSeconds).atOffset(ZoneOffset.UTC);
    } 
```
[Code Link](https://github.com/Azure/azure-sdk-for-java/blob/main/sdk/servicebus/azure-messaging-servicebus/src/main/java/com/azure/messaging/servicebus/implementation/ServiceBusSharedKeyCredential.java#L195-L196 )


Test: Create a customized signature which expiration time is 0, and send it out

```java
public void run() {
    AzureSasCredential credential = new AzureSasCredential(Credentials.serviceBusSignature);

    ServiceBusSenderClient sender = new ServiceBusClientBuilder()
            .credential(Credentials.serviceBusNamespace, credential)
            .sender()
            .queueName(Credentials.serviceBusQueue)
            .buildClient();

    List<ServiceBusMessage> messages = Arrays.asList(
            new ServiceBusMessage("Hello world").setMessageId("1"),
            new ServiceBusMessage("Bonjour").setMessageId("2"));

    sender.sendMessages(messages);

    sender.close();
}
```

It will recevie a 401 response:

```
AmqpException: status-code: 401, status-description: ExpiredToken: The token is expired. Expiration time: '1970-01-01 00:00:00Z'
```

Test Conclusion: the expiration time in token is not possible to be set as 0, otherwise it will not pass the authentication. 

It is very weird to got this error, maybe it because multi-thread or clock skew and the token expiration time couldn't set correctly?
