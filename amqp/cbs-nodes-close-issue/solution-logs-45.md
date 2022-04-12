```
16:42:01.356 [main] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Disposing of ReactorConnection.","connectionId":"MF_10b5be_1648197717023","isTransient":false,"isInitiatedByClient":true,"shutdownMessage":"Disposed by client."}
16:42:01.357 [main] INFO  c.a.m.e.i.EventHubConnectionProcessor - {"az.sdk.message":"Channel is disposed.","entityPath":"eventhub-test"}
16:42:01.358 [main] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Setting error condition and disposing session. Shutdown signal received","connectionId":"MF_10b5be_1648197717023","errorCondition":"n/a","errorDescription":"n/a","sessionName":"eventhub-test"}
16:42:01.362 [main] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Upstream connection publisher was completed. Terminating processor.","connectionId":"MF_10b5be_1648197717023","entityPath":"$cbs"}
16:42:01.362 [main] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"AMQP channel processor completed. Notifying 0 subscribers.","connectionId":"MF_10b5be_1648197717023","entityPath":"$cbs"}
16:42:01.362 [main] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Setting error condition and disposing session. Shutdown signal received","connectionId":"MF_10b5be_1648197717023","errorCondition":"n/a","errorDescription":"n/a","sessionName":"cbs-session"}
16:42:01.363 [main] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Shutdown signal received.","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.364 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Got endpoint state.","connectionId":"MF_10b5be_1648197717023","sessionName":"eventhub-test","state":"CLOSED"}
16:42:01.365 [main] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Closing request/response channel.","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.365 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Disposing of active send and receive links due to session close.","connectionId":"MF_10b5be_1648197717023","sessionName":"eventhub-test"}
16:42:01.366 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Got endpoint state.","connectionId":"MF_10b5be_1648197717023","sessionName":"cbs-session","state":"CLOSED"}
16:42:01.367 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Disposing of active send and receive links due to session close.","connectionId":"MF_10b5be_1648197717023","sessionName":"cbs-session"}
16:42:01.367 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionLocalClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"sessionName":"eventhub-test"}
16:42:01.367 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionLocalClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"sessionName":"cbs-session"}
16:42:01.368 [main] DEBUG c.a.c.a.implementation.ReactorSender - {"az.sdk.message":"Shutdown signal received.","connectionId":"MF_10b5be_1648197717023","entityPath":"eventhub-test","linkName":"eventhub-test"}
16:42:01.368 [main] DEBUG c.a.c.a.implementation.ReactorSender - {"az.sdk.message":"Setting error condition and disposing. Connection shutdown.","connectionId":"MF_10b5be_1648197717023","entityPath":"eventhub-test","linkName":"eventhub-test","errorCondition":"n/a","errorDescription":"n/a"}
16:42:01.368 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Closing send link and receive link.","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.368 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkLocalClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"linkName":"cbs:sender"}
16:42:01.368 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkLocalClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"linkName":"cbs:receiver"}
16:42:01.368 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkLocalClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"linkName":"eventhub-test"}
16:42:01.370 [main] DEBUG c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Scheduling closeConnection work.","connectionId":"MF_10b5be_1648197717023"}
16:42:01.370 [main] DEBUG c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Closed reactor dispatcher.","connectionId":"MF_10b5be_1648197717023","signalType":"onComplete"}
16:42:01.372 [reactor-executor-1] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Closing executor.","connectionId":"MF_10b5be_1648197717023"}
16:42:01.373 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onConnectionLocalClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"hostName":"eventhubs-liuku.servicebus.windows.net"}
16:42:01.676 [reactor-executor-1] INFO  c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionRemoteClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"sessionName":"eventhub-test"}
16:42:01.676 [reactor-executor-1] INFO  c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionRemoteClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"sessionName":"cbs-session"}
16:42:01.970 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onConnectionRemoteClose","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"hostName":"eventhubs-liuku.servicebus.windows.net"}
16:42:01.972 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onTransportClosed","connectionId":"MF_10b5be_1648197717023","errorCondition":"n/a","errorDescription":"n/a","hostName":"eventhubs-liuku.servicebus.windows.net"}
16:42:01.972 [reactor-executor-1] INFO  c.a.c.a.i.handler.TransportHandler - {"az.sdk.message":"onTransportClosed","connectionId":"MF_10b5be_1648197717023","hostName":"eventhubs-liuku.servicebus.windows.net"}
16:42:01.973 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onConnectionUnbound","connectionId":"MF_10b5be_1648197717023","hostName":"eventhubs-liuku.servicebus.windows.net","state":"CLOSED","remoteState":"CLOSED"}
16:42:01.973 [reactor-executor-1] INFO  c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFinal","connectionId":"MF_10b5be_1648197717023","linkName":"eventhub-test"}
16:42:01.974 [reactor-executor-1] DEBUG c.a.c.a.implementation.ReactorSender - {"az.sdk.message":"State CLOSED","connectionId":"MF_10b5be_1648197717023","entityPath":"eventhub-test","linkName":"eventhub-test"}
16:42:01.977 [reactor-executor-1] DEBUG c.a.c.a.implementation.ReactorSender - {"az.sdk.message":"This was already disposed.","connectionId":"MF_10b5be_1648197717023","entityPath":"eventhub-test","linkName":"eventhub-test"}
16:42:01.981 [reactor-executor-1] INFO  c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionFinal.","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"sessionName":"eventhub-test"}
16:42:01.981 [reactor-executor-1] INFO  c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFinal","connectionId":"MF_10b5be_1648197717023","linkName":"cbs:sender"}
16:42:01.981 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_10b5be_1648197717023","linkName":"cbs","sendState":"CLOSED","receiveState":"ACTIVE"}
16:42:01.981 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Channel already closed.","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.982 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"SendLinkHandler disposed. Remaining: 1","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.983 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_10b5be_1648197717023","linkName":"cbs","sendState":"CLOSED","receiveState":"CLOSED"}
16:42:01.983 [reactor-executor-1] INFO  c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkFinal","connectionId":"MF_10b5be_1648197717023","linkName":"cbs:receiver"}
16:42:01.983 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Channel already closed.","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.983 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"ReceiveLinkHandler disposed. Remaining: 0","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.984 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Terminating 0 unconfirmed sends (reason: The RequestResponseChannel didn't receive the acknowledgment for the send due receive link termination.).","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.984 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"completed the termination of 0 unconfirmed sends (reason: The RequestResponseChannel didn't receive the acknowledgment for the send due receive link termination.).","connectionId":"MF_10b5be_1648197717023","linkName":"cbs"}
16:42:01.984 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Channel is disposed.","connectionId":"MF_10b5be_1648197717023","entityPath":"$cbs"}
16:42:01.984 [reactor-executor-1] INFO  c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionFinal.","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"sessionName":"cbs-session"}
16:42:01.984 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onConnectionFinal","connectionId":"MF_10b5be_1648197717023","errorCondition":null,"errorDescription":null,"hostName":"eventhubs-liuku.servicebus.windows.net"}
16:42:05.400 [reactor-executor-1] INFO  c.a.c.a.i.ReactorExecutor - {"az.sdk.message":"Processing all pending tasks and closing old reactor.","connectionId":"MF_10b5be_1648197717023"}
16:42:05.400 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorExecutor - {"az.sdk.message":"Had more tasks to process on reactor but it is shutting down.","connectionId":"MF_10b5be_1648197717023"}
16:42:05.402 [reactor-executor-1] INFO  c.a.c.a.i.ReactorDispatcher - {"az.sdk.message":"Reactor selectable is being disposed.","connectionId":"MF_10b5be_1648197717023"}
16:42:05.402 [reactor-executor-1] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"onConnectionShutdown. Shutting down.","connectionId":"MF_10b5be_1648197717023","isTransient":false,"isInitiatedByClient":false,"shutdownMessage":"connectionId[MF_10b5be_1648197717023] Reactor selectable is disposed.","namespace":"eventhubs-liuku.servicebus.windows.net"}
16:42:05.402 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorExecutor - {"az.sdk.message":"Completing close and disposing scheduler. Finished processing pending tasks.","connectionId":"MF_10b5be_1648197717023"}
16:42:05.404 [reactor-executor-1] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"onConnectionShutdown. Shutting down.","connectionId":"MF_10b5be_1648197717023","isTransient":false,"isInitiatedByClient":false,"shutdownMessage":"Finished processing pending tasks.","namespace":"eventhubs-liuku.servicebus.windows.net"}

```