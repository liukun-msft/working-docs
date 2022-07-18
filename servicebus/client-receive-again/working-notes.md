# [BUG] Event Hub and Service Bus Producers should attempt to create a connection on send

https://github.com/Azure/azure-sdk-for-java/issues/27520

Test results:

- Sender: reconnection when send again after 35 mins when link and connection are inactive.
- Service Bus Receiver: both Sync and Async receiver can't receive message again after network issue and retry exhausted.
    

## ServiceBusReceiverAsyncClient 


