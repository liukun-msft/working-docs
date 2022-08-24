```
15:51:14.187 [main] DEBUG reactor.util.Loggers - Using Slf4j logging framework
15:51:14.466 [main] DEBUG c.a.c.a.i.ReactorConnection - {"az.sdk.message":"State UNINITIALIZED","connectionId":"MF_3a9815_1661327474431"}
15:51:14.469 [main] INFO  c.a.m.s.i.ServiceBusConnectionProcessor - {"az.sdk.message":"Setting next AMQP channel.","entityPath":"N/A"}
15:51:14.469 [main] INFO  c.a.m.s.i.ServiceBusConnectionProcessor - {"az.sdk.message":"Next AMQP channel received, updating 0 current subscribers","entityPath":"N/A"}
15:51:14.471 [main] DEBUG c.a.c.a.i.ReactorConnection - {"az.sdk.message":"State UNINITIALIZED","connectionId":"MF_3c1be6_1661327474471"}
15:51:14.472 [main] INFO  c.a.m.s.ServiceBusClientBuilder - # of open clients with shared connection: 1
15:51:14.477 [main] INFO  c.a.m.s.ServiceBusClientBuilder - # of open clients with shared connection: 2
15:51:14.488 [main] INFO  c.a.m.s.ServiceBusReceiverAsyncClient - testqueue: Creating consumer for link 'testqueue_7dfe44_1661327474488'
15:51:14.494 [main] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - Requesting a new AmqpReceiveLink from upstream.
15:51:14.507 [main] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Creating and starting connection.","connectionId":"MF_3a9815_1661327474431","hostName":"servicebusliuku.servicebus.windows.net","port":5671}
15:51:14.529 [main] INFO  c.a.c.a.i.ReactorExecutor - {"az.sdk.message":"Starting reactor.","connectionId":"MF_3a9815_1661327474431"}
15:51:14.540 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onConnectionInit","connectionId":"MF_3a9815_1661327474431","hostName":"servicebusliuku.servicebus.windows.net","namespace":"servicebusliuku.servicebus.windows.net"}
15:51:14.540 [reactor-executor-1] INFO  c.a.c.a.i.handler.ReactorHandler - {"az.sdk.message":"reactor.onReactorInit","connectionId":"MF_3a9815_1661327474431"}
15:51:14.541 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onConnectionLocalOpen","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"hostName":"servicebusliuku.servicebus.windows.net"}
15:51:14.610 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onConnectionBound","connectionId":"MF_3a9815_1661327474431","hostName":"servicebusliuku.servicebus.windows.net","peerDetails":"servicebusliuku.servicebus.windows.net:5671"}
15:51:14.848 [reactor-executor-1] INFO  c.a.c.a.i.h.StrictTlsContextSpi - SSLv2Hello was an enabled protocol. Filtering out.
15:51:16.352 [reactor-executor-1] INFO  c.a.c.a.i.handler.ConnectionHandler - {"az.sdk.message":"onConnectionRemoteOpen","connectionId":"MF_3a9815_1661327474431","hostName":"servicebusliuku.servicebus.windows.net","remoteContainer":"57a2975b9284489ea1e87700552818ef_G37"}
15:51:16.353 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorConnection - {"az.sdk.message":"State ACTIVE","connectionId":"MF_3a9815_1661327474431"}
15:51:16.353 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusConnectionProcessor - {"az.sdk.message":"Channel is now active.","entityPath":"N/A"}
15:51:16.363 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Got endpoint state.","connectionId":"MF_3a9815_1661327474431","sessionName":"testqueue","state":"UNINITIALIZED"}
15:51:16.366 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionLocalOpen","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"sessionName":"testqueue"}
15:51:16.655 [reactor-executor-1] INFO  c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionRemoteOpen","connectionId":"MF_3a9815_1661327474431","sessionName":"testqueue","sessionIncCapacity":0,"sessionOutgoingWindow":2147483647}
15:51:16.655 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Got endpoint state.","connectionId":"MF_3a9815_1661327474431","sessionName":"testqueue","state":"ACTIVE"}
15:51:16.655 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReactorAmqpConnection - Get or create consumer for path: 'testqueue'
15:51:16.656 [reactor-executor-1] DEBUG c.a.c.a.i.AzureTokenManagerProvider - {"az.sdk.message":"Creating new token manager.","audience":"amqp://servicebusliuku.servicebus.windows.net/testqueue","resource":"testqueue"}
15:51:16.664 [reactor-executor-1] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Setting CBS channel.","connectionId":"MF_3a9815_1661327474431"}
15:51:16.665 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Got endpoint state.","connectionId":"MF_3a9815_1661327474431","sessionName":"cbs-session","state":"UNINITIALIZED"}
15:51:16.666 [reactor-executor-1] DEBUG c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Added a subscriber.","connectionId":"MF_3a9815_1661327474431","entityPath":"$cbs","subscribers":1}
15:51:16.667 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionLocalOpen","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"sessionName":"cbs-session"}
15:51:16.953 [reactor-executor-1] INFO  c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionRemoteOpen","connectionId":"MF_3a9815_1661327474431","sessionName":"cbs-session","sessionIncCapacity":0,"sessionOutgoingWindow":2147483647}
15:51:16.953 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Got endpoint state.","connectionId":"MF_3a9815_1661327474431","sessionName":"cbs-session","state":"ACTIVE"}
15:51:16.959 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","sendState":null,"receiveState":"UNINITIALIZED"}
15:51:16.959 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","sendState":"UNINITIALIZED","receiveState":"UNINITIALIZED"}
15:51:16.960 [reactor-executor-1] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Emitting new response channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"$cbs","linkName":"cbs"}
15:51:16.960 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Setting next AMQP channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"$cbs"}
15:51:16.960 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Next AMQP channel received, updating 1 current subscribers","connectionId":"MF_3a9815_1661327474431","entityPath":"$cbs"}
15:51:16.966 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkLocalOpen","connectionId":"MF_3a9815_1661327474431","linkName":"cbs:sender","entityPath":"$cbs","localTarget":"Target{address='$cbs', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, capabilities=null}"}
15:51:16.968 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkLocalOpen","connectionId":"MF_3a9815_1661327474431","entityPath":"$cbs","linkName":"cbs:receiver","localSource":"Source{address='$cbs', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, distributionMode=null, filter=null, defaultOutcome=null, outcomes=null, capabilities=null}"}
15:51:17.253 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","sendState":"ACTIVE","receiveState":"UNINITIALIZED"}
15:51:17.253 [reactor-executor-1] INFO  c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkRemoteOpen","connectionId":"MF_3a9815_1661327474431","linkName":"cbs:sender","entityPath":"$cbs","remoteTarget":"Target{address='$cbs', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, capabilities=null}"}
15:51:17.254 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","unsettled":0,"credits":100}
15:51:17.254 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","sendState":"ACTIVE","receiveState":"ACTIVE"}
15:51:17.254 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Channel is now active.","connectionId":"MF_3a9815_1661327474431","entityPath":"$cbs"}
15:51:17.255 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","messageId":null}
15:51:17.256 [reactor-executor-1] INFO  c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkRemoteOpen","connectionId":"MF_3a9815_1661327474431","entityPath":"$cbs","linkName":"cbs:receiver","remoteSource":"Source{address='$cbs', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, distributionMode=null, filter=null, defaultOutcome=null, outcomes=null, capabilities=null}"}
15:51:17.260 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","unsettled":0,"credits":99}
15:51:17.547 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","messageId":"1"}
15:51:17.549 [reactor-executor-1] INFO  c.a.c.a.i.ActiveClientTokenManager - {"az.sdk.message":"Scheduling refresh token task.","scopes":"amqp://servicebusliuku.servicebus.windows.net/testqueue"}
15:51:17.554 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"$cbs","linkName":"cbs","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:17.555 [reactor-executor-1] INFO  c.a.c.a.i.ReactorSession - {"az.sdk.message":"Creating a new receiver link.","connectionId":"MF_3a9815_1661327474431","sessionName":"testqueue","linkName":"testqueue_7dfe44_1661327474488"}
15:51:17.556 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"State UNINITIALIZED","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","entityPath":"testqueue"}
15:51:17.558 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"Token refreshed.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","response":"ACCEPTED"}
15:51:17.568 [reactor-executor-1] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - Created consumer for Service Bus resource: [testqueue] mode: [PEEK_LOCK] sessionEnabled? true transferEntityPath: [N/A], entityType: [QUEUE]
15:51:17.568 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - linkName[testqueue_7dfe44_1661327474488] entityPath[testqueue]. Setting next AMQP receive link.
15:51:17.580 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - prefetch: '0', requested: '1', linkCredits: '0', expectedTotalCredit: '1', queuedMessages:'0', creditsToAdd: '1', messageQueue.size(): '0'
15:51:17.588 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReactorAmqpConnection - Get or create consumer for path: 'testqueue'
15:51:17.588 [reactor-executor-1] INFO  c.a.c.a.i.ReactorSession - {"az.sdk.message":"Returning existing receive link.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","entityPath":"testqueue"}
15:51:17.591 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkLocalOpen","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","localSource":"Source{address='testqueue', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, distributionMode=null, filter=null, defaultOutcome=null, outcomes=null, capabilities=null}"}
15:51:17.889 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"State ACTIVE","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","entityPath":"testqueue"}
15:51:17.889 [reactor-executor-1] INFO  c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkRemoteOpen","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","remoteSource":"Source{address='testqueue', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, distributionMode=null, filter=null, defaultOutcome=null, outcomes=null, capabilities=null}"}
15:51:17.892 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","updatedLinkCredit":1,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:17.893 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"There are no credits to add.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","credits":"0"}
15:51:17.908 [boundedElastic-3] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - prefetch: '0', requested: '2', linkCredits: '0', expectedTotalCredit: '2', queuedMessages:'1', creditsToAdd: '1', messageQueue.size(): '0'
Received message with id: 71
Start schedule message.
15:51:17.972 [boundedElastic-2] INFO  c.a.c.i.jackson.JacksonVersion - Package versions: jackson-annotations=2.13.1, jackson-core=2.13.1, jackson-databind=2.13.1, jackson-dataformat-xml=2.13.1, jackson-datatype-jsr310=2.13.1, azure-core=1.25.0, Troubleshooting version conflicts: https://aka.ms/azsdk/java/dependency/troubleshoot
15:51:18.073 [boundedElastic-2] DEBUG c.a.m.s.i.ServiceBusReactorAmqpConnection - Get or create sender link : 'testqueue'
15:51:18.073 [boundedElastic-2] DEBUG c.a.m.s.i.ServiceBusReactorSession - Get or create sender link testqueuetestqueue for entity path: 'testqueue'
15:51:18.073 [boundedElastic-2] DEBUG c.a.c.a.i.AzureTokenManagerProvider - {"az.sdk.message":"Creating new token manager.","audience":"amqp://servicebusliuku.servicebus.windows.net/testqueue","resource":"testqueue"}
15:51:18.075 [boundedElastic-2] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","messageId":null}
15:51:18.076 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","unsettled":0,"credits":98}
15:51:18.198 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","updatedLinkCredit":1,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:18.199 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"There are no credits to add.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","credits":"0"}
15:51:18.201 [boundedElastic-3] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - prefetch: '0', requested: '2', linkCredits: '0', expectedTotalCredit: '2', queuedMessages:'1', creditsToAdd: '1', messageQueue.size(): '0'
15:51:18.480 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","messageId":"2"}
15:51:18.481 [reactor-executor-1] INFO  c.a.c.a.i.ActiveClientTokenManager - {"az.sdk.message":"Scheduling refresh token task.","scopes":"amqp://servicebusliuku.servicebus.windows.net/testqueue"}
15:51:18.481 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"$cbs","linkName":"cbs","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:18.482 [reactor-executor-1] INFO  c.a.c.a.i.ReactorSession - {"az.sdk.message":"Creating a new send link.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueuetestqueue","sessionName":"testqueue"}
15:51:18.484 [reactor-executor-1] DEBUG c.a.c.a.implementation.ReactorSender - {"az.sdk.message":"State UNINITIALIZED","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue","linkName":"testqueuetestqueue"}
15:51:18.485 [reactor-executor-1] DEBUG c.a.c.a.implementation.ReactorSender - {"az.sdk.message":"Token refreshed.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue","linkName":"testqueuetestqueue","response":"ACCEPTED"}
15:51:18.486 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkLocalOpen","connectionId":"MF_3a9815_1661327474431","linkName":"testqueuetestqueue","entityPath":"testqueue","localTarget":"Target{address='testqueue', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, capabilities=null}"}
15:51:18.767 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","updatedLinkCredit":1,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:18.768 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"There are no credits to add.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","credits":"0"}
15:51:19.055 [reactor-executor-1] DEBUG c.a.c.a.implementation.ReactorSender - {"az.sdk.message":"State ACTIVE","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue","linkName":"testqueuetestqueue"}
15:51:19.056 [reactor-executor-1] DEBUG c.a.c.a.i.AzureTokenManagerProvider - {"az.sdk.message":"Creating new token manager.","audience":"amqp://servicebusliuku.servicebus.windows.net/testqueue","resource":"testqueue"}
15:51:19.057 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReactorAmqpConnection - Creating management node. entityPath: [testqueue]. address: [testqueue/$management]. linkName: [testqueue-mgmt]
15:51:19.058 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Got endpoint state.","connectionId":"MF_3a9815_1661327474431","sessionName":"testqueue-mgmt-session","state":"UNINITIALIZED"}
15:51:19.060 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","messageId":null}
15:51:19.060 [reactor-executor-1] INFO  c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkRemoteOpen","connectionId":"MF_3a9815_1661327474431","linkName":"testqueuetestqueue","entityPath":"testqueue","remoteTarget":"Target{address='testqueue', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, capabilities=null}"}
15:51:19.061 [reactor-executor-1] DEBUG c.a.c.a.implementation.ReactorSender - {"az.sdk.message":"Credits on link.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue","linkName":"testqueuetestqueue","credits":"1000"}
15:51:19.061 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueuetestqueue","unsettled":0,"credits":1000}
15:51:19.061 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionLocalOpen","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"sessionName":"testqueue-mgmt-session"}
15:51:19.062 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","unsettled":0,"credits":97}
15:51:19.349 [reactor-executor-1] INFO  c.a.c.a.i.handler.SessionHandler - {"az.sdk.message":"onSessionRemoteOpen","connectionId":"MF_3a9815_1661327474431","sessionName":"testqueue-mgmt-session","sessionIncCapacity":0,"sessionOutgoingWindow":2147483647}
15:51:19.349 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Got endpoint state.","connectionId":"MF_3a9815_1661327474431","sessionName":"testqueue-mgmt-session","state":"ACTIVE"}
15:51:19.350 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","sendState":null,"receiveState":"UNINITIALIZED"}
15:51:19.350 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","sendState":"UNINITIALIZED","receiveState":"UNINITIALIZED"}
15:51:19.351 [reactor-executor-1] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Emitting new response channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management","linkName":"testqueue-mgmt"}
15:51:19.351 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Setting next AMQP channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:19.351 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Next AMQP channel received, updating 0 current subscribers","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:19.356 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkLocalOpen","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt:sender","entityPath":"testqueue/$management","localTarget":"Target{address='testqueue/$management', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, capabilities=null}"}
15:51:19.357 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkLocalOpen","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management","linkName":"testqueue-mgmt:receiver","localSource":"Source{address='testqueue/$management', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, distributionMode=null, filter=null, defaultOutcome=null, outcomes=null, capabilities=null}"}
15:51:19.358 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","messageId":"3"}
15:51:19.358 [reactor-executor-1] INFO  c.a.c.a.i.ActiveClientTokenManager - {"az.sdk.message":"Scheduling refresh token task.","scopes":"amqp://servicebusliuku.servicebus.windows.net/testqueue"}
15:51:19.372 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"$cbs","linkName":"cbs","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:19.663 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","sendState":"ACTIVE","receiveState":"UNINITIALIZED"}
15:51:19.663 [reactor-executor-1] INFO  c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkRemoteOpen","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt:sender","entityPath":"testqueue/$management","remoteTarget":"Target{address='testqueue/$management', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, capabilities=null}"}
15:51:19.664 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":1000}
15:51:19.664 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","sendState":"ACTIVE","receiveState":"ACTIVE"}
15:51:19.664 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Channel is now active.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:19.664 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:51:19.665 [reactor-executor-1] INFO  c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onLinkRemoteOpen","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management","linkName":"testqueue-mgmt:receiver","remoteSource":"Source{address='testqueue/$management', durable=NONE, expiryPolicy=SESSION_END, timeout=0, dynamic=false, dynamicNodeProperties=null, distributionMode=null, filter=null, defaultOutcome=null, outcomes=null, capabilities=null}"}
15:51:19.666 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":999}
15:51:19.966 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"1"}
Sent scheduled message.
15:51:19.968 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:19.969 [boundedElastic-2] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - testqueue: Update started. Disposition: COMPLETED. Lock: d45404aa-d398-4a81-a1ed-a821aee72166. SessionId: null.
15:51:20.336 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:20.337 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReactorReceiver - entityPath[testqueue], linkName[testqueue_7dfe44_1661327474488], deliveryTag[d45404aa-d398-4a81-a1ed-a821aee72166], state[Accepted{}] Received update disposition delivery.
15:51:20.337 [reactor-executor-1] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - testqueue: Update completed. Disposition: COMPLETED. Lock: d45404aa-d398-4a81-a1ed-a821aee72166.
15:51:20.337 [reactor-executor-1] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - Closing expired renewal operation. lockToken[d45404aa-d398-4a81-a1ed-a821aee72166]. status[RUNNING]. throwable[null].
15:51:20.337 [reactor-executor-1] DEBUG c.a.m.s.LockRenewalOperation - token[d45404aa-d398-4a81-a1ed-a821aee72166] Cancelled operation.
Finished process message.
15:51:20.338 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"There are no credits to add.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","credits":"0"}
15:51:20.338 [boundedElastic-2] DEBUG c.a.m.s.ServiceBusProcessorClient - Requesting 1 more message from upstream
15:51:20.338 [boundedElastic-2] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - prefetch: '0', requested: '1', linkCredits: '0', expectedTotalCredit: '1', queuedMessages:'0', creditsToAdd: '1', messageQueue.size(): '0'
Received message with id: 72
15:51:20.626 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","updatedLinkCredit":1,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:20.627 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"There are no credits to add.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","credits":"0"}
15:51:37.981 [parallel-10] INFO  c.a.m.s.LockRenewalOperation - token[5d9a8be4-09c9-4cb4-8faf-fd4b6dd7d759]. now[2022-08-24T15:51:37.981601+08:00]. Starting lock renewal.
15:51:37.982 [parallel-10] DEBUG c.a.c.a.i.AzureTokenManagerProvider - {"az.sdk.message":"Creating new token manager.","audience":"amqp://servicebusliuku.servicebus.windows.net/testqueue","resource":"testqueue"}
15:51:37.983 [parallel-10] INFO  c.a.m.s.i.ServiceBusReactorAmqpConnection - Creating management node. entityPath: [testqueue]. address: [testqueue/$management]. linkName: [testqueue-mgmt]
15:51:37.984 [parallel-10] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","sendState":null,"receiveState":"UNINITIALIZED"}
15:51:37.985 [parallel-10] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","sendState":"UNINITIALIZED","receiveState":"UNINITIALIZED"}
15:51:37.986 [parallel-10] INFO  c.a.c.a.i.ReactorConnection - {"az.sdk.message":"Emitting new response channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management","linkName":"testqueue-mgmt"}
15:51:37.987 [parallel-10] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Setting next AMQP channel.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:37.987 [parallel-10] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Next AMQP channel received, updating 0 current subscribers","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:37.989 [parallel-10] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","messageId":null}
15:51:37.990 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","unsettled":0,"credits":96}
15:51:38.276 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"cbs","messageId":"4"}
15:51:38.277 [reactor-executor-1] INFO  c.a.c.a.i.ActiveClientTokenManager - {"az.sdk.message":"Scheduling refresh token task.","scopes":"amqp://servicebusliuku.servicebus.windows.net/testqueue"}
15:51:38.279 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"$cbs","linkName":"cbs","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:38.569 [parallel-15] INFO  c.a.m.s.LockRenewalOperation - token[a11c5254-c3c7-4b21-8bc9-b5a5b353b230]. now[2022-08-24T15:51:38.569860700+08:00]. Starting lock renewal.
15:51:40.423 [parallel-11] INFO  c.a.m.s.LockRenewalOperation - token[a4ba14fd-f719-4e79-b011-f6fa00fddc77]. now[2022-08-24T15:51:40.423458800+08:00]. Starting lock renewal.
Start schedule message.
15:51:50.348 [boundedElastic-2] DEBUG c.a.m.s.i.ServiceBusReactorAmqpConnection - Get or create sender link : 'testqueue'
15:51:50.348 [boundedElastic-2] DEBUG c.a.m.s.i.ServiceBusReactorSession - Get or create sender link testqueuetestqueue for entity path: 'testqueue'
15:51:50.348 [boundedElastic-2] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Returning existing send link.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueuetestqueue"}
15:51:50.349 [boundedElastic-2] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:51:50.350 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","sendState":"ACTIVE","receiveState":"UNINITIALIZED"}
15:51:50.350 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":998}
15:51:50.646 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Updating endpoint states.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","sendState":"ACTIVE","receiveState":"ACTIVE"}
15:51:50.646 [reactor-executor-1] INFO  c.a.c.a.i.AmqpChannelProcessor - {"az.sdk.message":"Channel is now active.","connectionId":"MF_3a9815_1661327474431","entityPath":"testqueue/$management"}
15:51:50.647 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:51:50.647 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:51:50.648 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:51:50.649 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"2"}
15:51:50.653 [reactor-executor-1] ERROR c.a.m.s.i.ManagementChannel<testqueue> - Service bus response empty. Could not renew message with lock token: 'a11c5254-c3c7-4b21-8bc9-b5a5b353b230'., errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue]
com.azure.core.amqp.exception.AmqpException: Service bus response empty. Could not renew message with lock token: 'a11c5254-c3c7-4b21-8bc9-b5a5b353b230'., errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue]
	at com.azure.messaging.servicebus.implementation.ManagementChannel.lambda$renewMessageLock$10(ManagementChannel.java:266)
	at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onNext(FluxMapFuseable.java:113)
	at reactor.core.publisher.Operators$MonoSubscriber.complete(Operators.java:1816)
	at reactor.core.publisher.MonoFlatMap$FlatMapInner.onNext(MonoFlatMap.java:249)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onNext(FluxOnErrorResume.java:79)
	at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onNext(FluxSwitchIfEmpty.java:74)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:119)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.complete(MonoIgnoreThen.java:284)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.onNext(MonoIgnoreThen.java:187)
	at reactor.core.publisher.MonoCreate$DefaultMonoSink.success(MonoCreate.java:165)
	at com.azure.core.amqp.implementation.RequestResponseChannel.settleMessage(RequestResponseChannel.java:392)
	at com.azure.core.amqp.implementation.RequestResponseChannel.lambda$new$0(RequestResponseChannel.java:180)
	at reactor.core.publisher.LambdaSubscriber.onNext(LambdaSubscriber.java:160)
	at reactor.core.publisher.FluxMap$MapSubscriber.onNext(FluxMap.java:120)
	at reactor.core.publisher.FluxPeek$PeekSubscriber.onNext(FluxPeek.java:200)
	at reactor.core.publisher.EmitterProcessor.drain(EmitterProcessor.java:491)
	at reactor.core.publisher.EmitterProcessor.tryEmitNext(EmitterProcessor.java:299)
	at reactor.core.publisher.SinkManySerialized.tryEmitNext(SinkManySerialized.java:100)
	at reactor.core.publisher.InternalManySink.emitNext(InternalManySink.java:27)
	at com.azure.core.amqp.implementation.handler.ReceiveLinkHandler.onDelivery(ReceiveLinkHandler.java:164)
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:185)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
15:51:50.654 [reactor-executor-1] ERROR c.a.m.s.LockRenewalOperation - token[{}]. Error occurred while renewing lock token.
Service bus response empty. Could not renew message with lock token: 'a11c5254-c3c7-4b21-8bc9-b5a5b353b230'., errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue]
com.azure.core.amqp.exception.AmqpException: Service bus response empty. Could not renew message with lock token: 'a11c5254-c3c7-4b21-8bc9-b5a5b353b230'., errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue]
	at com.azure.messaging.servicebus.implementation.ManagementChannel.lambda$renewMessageLock$10(ManagementChannel.java:266)
	at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onNext(FluxMapFuseable.java:113)
	at reactor.core.publisher.Operators$MonoSubscriber.complete(Operators.java:1816)
	at reactor.core.publisher.MonoFlatMap$FlatMapInner.onNext(MonoFlatMap.java:249)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onNext(FluxOnErrorResume.java:79)
	at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onNext(FluxSwitchIfEmpty.java:74)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:119)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.complete(MonoIgnoreThen.java:284)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.onNext(MonoIgnoreThen.java:187)
	at reactor.core.publisher.MonoCreate$DefaultMonoSink.success(MonoCreate.java:165)
	at com.azure.core.amqp.implementation.RequestResponseChannel.settleMessage(RequestResponseChannel.java:392)
	at com.azure.core.amqp.implementation.RequestResponseChannel.lambda$new$0(RequestResponseChannel.java:180)
	at reactor.core.publisher.LambdaSubscriber.onNext(LambdaSubscriber.java:160)
	at reactor.core.publisher.FluxMap$MapSubscriber.onNext(FluxMap.java:120)
	at reactor.core.publisher.FluxPeek$PeekSubscriber.onNext(FluxPeek.java:200)
	at reactor.core.publisher.EmitterProcessor.drain(EmitterProcessor.java:491)
	at reactor.core.publisher.EmitterProcessor.tryEmitNext(EmitterProcessor.java:299)
	at reactor.core.publisher.SinkManySerialized.tryEmitNext(SinkManySerialized.java:100)
	at reactor.core.publisher.InternalManySink.emitNext(InternalManySink.java:27)
	at com.azure.core.amqp.implementation.handler.ReceiveLinkHandler.onDelivery(ReceiveLinkHandler.java:164)
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:185)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
15:51:50.654 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:50.655 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":995}
15:51:51.174 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"2"}
15:51:51.174 [reactor-executor-1] WARN  c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Received delivery without pending message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"2"}
15:51:51.175 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":2,"remoteCredit":1,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:51.175 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"1"}
15:51:51.178 [reactor-executor-1] WARN  c.a.m.s.i.ManagementChannel<testqueue> - status[GONE] description[The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:39a37907-b2ca-4cdd-8da2-811557d8ddf1, TrackingId:b4decae9-59f0-4ec3-8df0-216546fce61a_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50] condition[com.microsoft:message-lock-lost] Operation not successful.
15:51:51.180 [reactor-executor-1] ERROR c.a.m.s.LockRenewalOperation - token[{}]. Error occurred while renewing lock token.
The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:39a37907-b2ca-4cdd-8da2-811557d8ddf1, TrackingId:b4decae9-59f0-4ec3-8df0-216546fce61a_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 1]
com.azure.messaging.servicebus.ServiceBusException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:39a37907-b2ca-4cdd-8da2-811557d8ddf1, TrackingId:b4decae9-59f0-4ec3-8df0-216546fce61a_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 1]
	at com.azure.messaging.servicebus.implementation.ManagementChannel.mapError(ManagementChannel.java:241)
	at reactor.core.publisher.Mono.lambda$onErrorMap$31(Mono.java:3733)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onError(FluxOnErrorResume.java:94)
	at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.onError(Operators.java:2063)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onError(FluxHandle.java:203)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:125)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.complete(MonoIgnoreThen.java:284)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.onNext(MonoIgnoreThen.java:187)
	at reactor.core.publisher.MonoCreate$DefaultMonoSink.success(MonoCreate.java:165)
	at com.azure.core.amqp.implementation.RequestResponseChannel.settleMessage(RequestResponseChannel.java:392)
	at com.azure.core.amqp.implementation.RequestResponseChannel.lambda$new$0(RequestResponseChannel.java:180)
	at reactor.core.publisher.LambdaSubscriber.onNext(LambdaSubscriber.java:160)
	at reactor.core.publisher.FluxMap$MapSubscriber.onNext(FluxMap.java:120)
	at reactor.core.publisher.FluxPeek$PeekSubscriber.onNext(FluxPeek.java:200)
	at reactor.core.publisher.EmitterProcessor.drain(EmitterProcessor.java:491)
	at reactor.core.publisher.EmitterProcessor.tryEmitNext(EmitterProcessor.java:299)
	at reactor.core.publisher.SinkManySerialized.tryEmitNext(SinkManySerialized.java:100)
	at reactor.core.publisher.InternalManySink.emitNext(InternalManySink.java:27)
	at com.azure.core.amqp.implementation.handler.ReceiveLinkHandler.onDelivery(ReceiveLinkHandler.java:164)
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:185)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: com.azure.core.amqp.exception.AmqpException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:39a37907-b2ca-4cdd-8da2-811557d8ddf1, TrackingId:b4decae9-59f0-4ec3-8df0-216546fce61a_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 1]
	at com.azure.core.amqp.implementation.ExceptionUtil.toException(ExceptionUtil.java:85)
	at com.azure.messaging.servicebus.implementation.ManagementChannel.lambda$sendWithVerify$17(ManagementChannel.java:498)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:103)
	... 25 common frames omitted
15:51:51.180 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":1,"remoteCredit":1,"delivery.isPartial":false,"delivery.isSettled":false}
15:51:51.180 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"3"}
15:51:51.180 [reactor-executor-1] WARN  c.a.m.s.i.ManagementChannel<testqueue> - status[GONE] description[The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:d8ab854f-fc5c-4b55-83ad-0e8b0161eac9, TrackingId:02c4993e-a8f0-4823-b96f-b7348d218962_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50] condition[com.microsoft:message-lock-lost] Operation not successful.
15:51:51.181 [reactor-executor-1] ERROR c.a.m.s.LockRenewalOperation - token[{}]. Error occurred while renewing lock token.
The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:d8ab854f-fc5c-4b55-83ad-0e8b0161eac9, TrackingId:02c4993e-a8f0-4823-b96f-b7348d218962_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 0]
com.azure.messaging.servicebus.ServiceBusException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:d8ab854f-fc5c-4b55-83ad-0e8b0161eac9, TrackingId:02c4993e-a8f0-4823-b96f-b7348d218962_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 0]
	at com.azure.messaging.servicebus.implementation.ManagementChannel.mapError(ManagementChannel.java:241)
	at reactor.core.publisher.Mono.lambda$onErrorMap$31(Mono.java:3733)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onError(FluxOnErrorResume.java:94)
	at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.onError(Operators.java:2063)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onError(FluxHandle.java:203)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:125)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.complete(MonoIgnoreThen.java:284)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.onNext(MonoIgnoreThen.java:187)
	at reactor.core.publisher.MonoCreate$DefaultMonoSink.success(MonoCreate.java:165)
	at com.azure.core.amqp.implementation.RequestResponseChannel.settleMessage(RequestResponseChannel.java:392)
	at com.azure.core.amqp.implementation.RequestResponseChannel.lambda$new$0(RequestResponseChannel.java:180)
	at reactor.core.publisher.LambdaSubscriber.onNext(LambdaSubscriber.java:160)
	at reactor.core.publisher.FluxMap$MapSubscriber.onNext(FluxMap.java:120)
	at reactor.core.publisher.FluxPeek$PeekSubscriber.onNext(FluxPeek.java:200)
	at reactor.core.publisher.EmitterProcessor.drain(EmitterProcessor.java:491)
	at reactor.core.publisher.EmitterProcessor.tryEmitNext(EmitterProcessor.java:299)
	at reactor.core.publisher.SinkManySerialized.tryEmitNext(SinkManySerialized.java:100)
	at reactor.core.publisher.InternalManySink.emitNext(InternalManySink.java:27)
	at com.azure.core.amqp.implementation.handler.ReceiveLinkHandler.onDelivery(ReceiveLinkHandler.java:164)
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:185)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: com.azure.core.amqp.exception.AmqpException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:d8ab854f-fc5c-4b55-83ad-0e8b0161eac9, TrackingId:02c4993e-a8f0-4823-b96f-b7348d218962_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 0]
	at com.azure.core.amqp.implementation.ExceptionUtil.toException(ExceptionUtil.java:85)
	at com.azure.messaging.servicebus.implementation.ManagementChannel.lambda$sendWithVerify$17(ManagementChannel.java:498)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:103)
	... 25 common frames omitted
15:51:51.181 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
java.lang.IllegalStateException: Timeout on blocking read for 245600000000 NANOSECONDS
	at reactor.core.publisher.BlockingSingleSubscriber.blockingGet(BlockingSingleSubscriber.java:123)
	at reactor.core.publisher.Mono.block(Mono.java:1731)
	at com.azure.messaging.servicebus.ServiceBusSenderClient.scheduleMessage(ServiceBusSenderClient.java:274)
	at com.azure.servicebus.issues.schedule.ReceiveAndScheduleMessage.scheduleMessage(ReceiveAndScheduleMessage.java:28)
	at com.azure.servicebus.issues.schedule.ReceiveAndScheduleMessage.lambda$static$0(ReceiveAndScheduleMessage.java:41)
	at com.azure.messaging.servicebus.ServiceBusProcessorClient$1.onNext(ServiceBusProcessorClient.java:295)
	at com.azure.messaging.servicebus.ServiceBusProcessorClient$1.onNext(ServiceBusProcessorClient.java:269)
	at reactor.core.publisher.FluxPublishOn$PublishOnSubscriber.runAsync(FluxPublishOn.java:440)
	at reactor.core.publisher.FluxPublishOn$PublishOnSubscriber.run(FluxPublishOn.java:527)
	at reactor.core.scheduler.WorkerTask.call(WorkerTask.java:84)
	at reactor.core.scheduler.WorkerTask.call(WorkerTask.java:37)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
15:55:55.951 [boundedElastic-2] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - testqueue: Update started. Disposition: COMPLETED. Lock: 5d9a8be4-09c9-4cb4-8faf-fd4b6dd7d759. SessionId: null.
15:55:56.591 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:55:56.591 [reactor-executor-1] DEBUG c.a.m.s.i.ServiceBusReactorReceiver - entityPath[testqueue], linkName[testqueue_7dfe44_1661327474488], deliveryTag[5d9a8be4-09c9-4cb4-8faf-fd4b6dd7d759], state[Rejected{error=Error{condition=com.microsoft:message-lock-lost, description='The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:edf7bdf9-7445-4c1d-b036-45c349ed6bb4, TrackingId:db7bf3c8000000400393c2ab6305d875_G37_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:55:56', info=null}}] Received update disposition delivery.
15:55:56.592 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReactorReceiver - Received delivery '5d9a8be4-09c9-4cb4-8faf-fd4b6dd7d759' state 'Rejected{error=Error{condition=com.microsoft:message-lock-lost, description='The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:edf7bdf9-7445-4c1d-b036-45c349ed6bb4, TrackingId:db7bf3c8000000400393c2ab6305d875_G37_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:55:56', info=null}}' doesn't match expected state 'Accepted{}'
15:55:56.594 [reactor-executor-1] INFO  c.a.m.s.i.ServiceBusReactorReceiver - deliveryTag[5d9a8be4-09c9-4cb4-8faf-fd4b6dd7d759], state[{}]. Retry attempts exhausted.
com.azure.core.amqp.exception.AmqpException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:edf7bdf9-7445-4c1d-b036-45c349ed6bb4, TrackingId:db7bf3c8000000400393c2ab6305d875_G37_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:55:56, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue, REFERENCE_ID: testqueue_7dfe44_1661327474488, LINK_CREDIT: 0]
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
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:206)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.engine.impl.EventImpl.delegate(EventImpl.java:129)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:114)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
15:55:56.594 [reactor-executor-1] ERROR c.a.m.s.i.ServiceBusReactorReceiver - The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:edf7bdf9-7445-4c1d-b036-45c349ed6bb4, TrackingId:db7bf3c8000000400393c2ab6305d875_G37_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:55:56, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue, REFERENCE_ID: testqueue_7dfe44_1661327474488, LINK_CREDIT: 0]
com.azure.core.amqp.exception.AmqpException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:edf7bdf9-7445-4c1d-b036-45c349ed6bb4, TrackingId:db7bf3c8000000400393c2ab6305d875_G37_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:55:56, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue, REFERENCE_ID: testqueue_7dfe44_1661327474488, LINK_CREDIT: 0]
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
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:206)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.engine.impl.EventImpl.delegate(EventImpl.java:129)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:114)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
15:55:56.595 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"There are no credits to add.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","credits":"0"}
15:55:56.595 [boundedElastic-2] DEBUG c.a.m.s.ServiceBusProcessorClient - Requesting 1 more message from upstream
15:55:56.595 [boundedElastic-2] INFO  c.a.m.s.i.ServiceBusReceiveLinkProcessor - prefetch: '0', requested: '1', linkCredits: '0', expectedTotalCredit: '1', queuedMessages:'0', creditsToAdd: '1', messageQueue.size(): '0'
Received message with id: 73
Error occurred while receiving message: com.azure.messaging.servicebus.ServiceBusException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:edf7bdf9-7445-4c1d-b036-45c349ed6bb4, TrackingId:db7bf3c8000000400393c2ab6305d875_G37_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:55:56, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue, REFERENCE_ID: testqueue_7dfe44_1661327474488, LINK_CREDIT: 0]
15:55:56.884 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue","linkName":"testqueue_7dfe44_1661327474488","updatedLinkCredit":1,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:55:56.884 [reactor-executor-1] DEBUG c.a.c.a.i.ReactorReceiver - {"az.sdk.message":"There are no credits to add.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue_7dfe44_1661327474488","credits":"0"}
15:56:16.668 [parallel-11] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:56:16.668170300+08:00]. Starting lock renewal.
15:56:16.668 [parallel-11] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:56:16.669 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":994}
15:56:16.956 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"4"}
15:56:16.958 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:56:46.740Z]. next: [PT29.7820963S]. isSession[false]
15:56:16.959 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
Start schedule message.
15:56:26.608 [boundedElastic-2] DEBUG c.a.m.s.i.ServiceBusReactorAmqpConnection - Get or create sender link : 'testqueue'
15:56:26.609 [boundedElastic-2] DEBUG c.a.m.s.i.ServiceBusReactorSession - Get or create sender link testqueuetestqueue for entity path: 'testqueue'
15:56:26.609 [boundedElastic-2] DEBUG c.a.c.a.i.ReactorSession - {"az.sdk.message":"Returning existing send link.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueuetestqueue"}
15:56:26.611 [boundedElastic-2] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:56:26.612 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":993}
15:56:26.906 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"3"}
15:56:26.906 [reactor-executor-1] WARN  c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Received delivery without pending message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"3"}
15:56:26.906 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:56:36.750 [parallel-13] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:56:36.750202500+08:00]. Starting lock renewal.
15:56:36.751 [parallel-13] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:56:36.754 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":992}
15:56:37.039 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"5"}
15:56:37.040 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:57:06.820Z]. next: [PT29.7801569S]. isSession[false]
15:56:37.040 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:56:56.833 [parallel-2] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:56:56.833328400+08:00]. Starting lock renewal.
15:56:56.833 [parallel-2] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:56:56.834 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":991}
15:56:57.121 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"6"}
15:56:57.121 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:57:26.899Z]. next: [PT29.7777569S]. isSession[false]
15:56:57.122 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:57:14.490 [parallel-2] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - Closing expired renewal operation. lockToken[a4ba14fd-f719-4e79-b011-f6fa00fddc77]. status[FAILED]. throwable[{}].
com.azure.messaging.servicebus.ServiceBusException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:d8ab854f-fc5c-4b55-83ad-0e8b0161eac9, TrackingId:02c4993e-a8f0-4823-b96f-b7348d218962_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 0]
	at com.azure.messaging.servicebus.implementation.ManagementChannel.mapError(ManagementChannel.java:241)
	at reactor.core.publisher.Mono.lambda$onErrorMap$31(Mono.java:3733)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onError(FluxOnErrorResume.java:94)
	at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.onError(Operators.java:2063)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onError(FluxHandle.java:203)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:125)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.complete(MonoIgnoreThen.java:284)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.onNext(MonoIgnoreThen.java:187)
	at reactor.core.publisher.MonoCreate$DefaultMonoSink.success(MonoCreate.java:165)
	at com.azure.core.amqp.implementation.RequestResponseChannel.settleMessage(RequestResponseChannel.java:392)
	at com.azure.core.amqp.implementation.RequestResponseChannel.lambda$new$0(RequestResponseChannel.java:180)
	at reactor.core.publisher.LambdaSubscriber.onNext(LambdaSubscriber.java:160)
	at reactor.core.publisher.FluxMap$MapSubscriber.onNext(FluxMap.java:120)
	at reactor.core.publisher.FluxPeek$PeekSubscriber.onNext(FluxPeek.java:200)
	at reactor.core.publisher.EmitterProcessor.drain(EmitterProcessor.java:491)
	at reactor.core.publisher.EmitterProcessor.tryEmitNext(EmitterProcessor.java:299)
	at reactor.core.publisher.SinkManySerialized.tryEmitNext(SinkManySerialized.java:100)
	at reactor.core.publisher.InternalManySink.emitNext(InternalManySink.java:27)
	at com.azure.core.amqp.implementation.handler.ReceiveLinkHandler.onDelivery(ReceiveLinkHandler.java:164)
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:185)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: com.azure.core.amqp.exception.AmqpException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:d8ab854f-fc5c-4b55-83ad-0e8b0161eac9, TrackingId:02c4993e-a8f0-4823-b96f-b7348d218962_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 0]
	at com.azure.core.amqp.implementation.ExceptionUtil.toException(ExceptionUtil.java:85)
	at com.azure.messaging.servicebus.implementation.ManagementChannel.lambda$sendWithVerify$17(ManagementChannel.java:498)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:103)
	... 25 common frames omitted
15:57:14.491 [parallel-2] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - Closing expired renewal operation. lockToken[5d9a8be4-09c9-4cb4-8faf-fd4b6dd7d759]. status[FAILED]. throwable[{}].
com.azure.messaging.servicebus.ServiceBusException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:39a37907-b2ca-4cdd-8da2-811557d8ddf1, TrackingId:b4decae9-59f0-4ec3-8df0-216546fce61a_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 1]
	at com.azure.messaging.servicebus.implementation.ManagementChannel.mapError(ManagementChannel.java:241)
	at reactor.core.publisher.Mono.lambda$onErrorMap$31(Mono.java:3733)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onError(FluxOnErrorResume.java:94)
	at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.onError(Operators.java:2063)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onError(FluxHandle.java:203)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:125)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.complete(MonoIgnoreThen.java:284)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.onNext(MonoIgnoreThen.java:187)
	at reactor.core.publisher.MonoCreate$DefaultMonoSink.success(MonoCreate.java:165)
	at com.azure.core.amqp.implementation.RequestResponseChannel.settleMessage(RequestResponseChannel.java:392)
	at com.azure.core.amqp.implementation.RequestResponseChannel.lambda$new$0(RequestResponseChannel.java:180)
	at reactor.core.publisher.LambdaSubscriber.onNext(LambdaSubscriber.java:160)
	at reactor.core.publisher.FluxMap$MapSubscriber.onNext(FluxMap.java:120)
	at reactor.core.publisher.FluxPeek$PeekSubscriber.onNext(FluxPeek.java:200)
	at reactor.core.publisher.EmitterProcessor.drain(EmitterProcessor.java:491)
	at reactor.core.publisher.EmitterProcessor.tryEmitNext(EmitterProcessor.java:299)
	at reactor.core.publisher.SinkManySerialized.tryEmitNext(SinkManySerialized.java:100)
	at reactor.core.publisher.InternalManySink.emitNext(InternalManySink.java:27)
	at com.azure.core.amqp.implementation.handler.ReceiveLinkHandler.onDelivery(ReceiveLinkHandler.java:164)
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:185)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: com.azure.core.amqp.exception.AmqpException: The lock supplied is invalid. Either the lock expired, or the message has already been removed from the queue. For more information please see https://aka.ms/ServiceBusExceptions . Reference:39a37907-b2ca-4cdd-8da2-811557d8ddf1, TrackingId:b4decae9-59f0-4ec3-8df0-216546fce61a_B23, SystemTracker:servicebusliuku:Queue:testqueue, Timestamp:2022-08-24T07:51:50, errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue/$management, REFERENCE_ID: testqueue-mgmt:receiver, LINK_CREDIT: 1]
	at com.azure.core.amqp.implementation.ExceptionUtil.toException(ExceptionUtil.java:85)
	at com.azure.messaging.servicebus.implementation.ManagementChannel.lambda$sendWithVerify$17(ManagementChannel.java:498)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:103)
	... 25 common frames omitted
15:57:14.492 [parallel-2] DEBUG c.a.m.s.ServiceBusReceiverAsyncClient - Closing expired renewal operation. lockToken[a11c5254-c3c7-4b21-8bc9-b5a5b353b230]. status[FAILED]. throwable[{}].
com.azure.core.amqp.exception.AmqpException: Service bus response empty. Could not renew message with lock token: 'a11c5254-c3c7-4b21-8bc9-b5a5b353b230'., errorContext[NAMESPACE: servicebusliuku.servicebus.windows.net. ERROR CONTEXT: N/A, PATH: testqueue]
	at com.azure.messaging.servicebus.implementation.ManagementChannel.lambda$renewMessageLock$10(ManagementChannel.java:266)
	at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onNext(FluxMapFuseable.java:113)
	at reactor.core.publisher.Operators$MonoSubscriber.complete(Operators.java:1816)
	at reactor.core.publisher.MonoFlatMap$FlatMapInner.onNext(MonoFlatMap.java:249)
	at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onNext(FluxOnErrorResume.java:79)
	at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onNext(FluxSwitchIfEmpty.java:74)
	at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:119)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.complete(MonoIgnoreThen.java:284)
	at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.onNext(MonoIgnoreThen.java:187)
	at reactor.core.publisher.MonoCreate$DefaultMonoSink.success(MonoCreate.java:165)
	at com.azure.core.amqp.implementation.RequestResponseChannel.settleMessage(RequestResponseChannel.java:392)
	at com.azure.core.amqp.implementation.RequestResponseChannel.lambda$new$0(RequestResponseChannel.java:180)
	at reactor.core.publisher.LambdaSubscriber.onNext(LambdaSubscriber.java:160)
	at reactor.core.publisher.FluxMap$MapSubscriber.onNext(FluxMap.java:120)
	at reactor.core.publisher.FluxPeek$PeekSubscriber.onNext(FluxPeek.java:200)
	at reactor.core.publisher.EmitterProcessor.drain(EmitterProcessor.java:491)
	at reactor.core.publisher.EmitterProcessor.tryEmitNext(EmitterProcessor.java:299)
	at reactor.core.publisher.SinkManySerialized.tryEmitNext(SinkManySerialized.java:100)
	at reactor.core.publisher.InternalManySink.emitNext(InternalManySink.java:27)
	at com.azure.core.amqp.implementation.handler.ReceiveLinkHandler.onDelivery(ReceiveLinkHandler.java:164)
	at org.apache.qpid.proton.engine.BaseHandler.handle(BaseHandler.java:185)
	at org.apache.qpid.proton.engine.impl.EventImpl.dispatch(EventImpl.java:108)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.dispatch(ReactorImpl.java:324)
	at org.apache.qpid.proton.reactor.impl.ReactorImpl.process(ReactorImpl.java:291)
	at com.azure.core.amqp.implementation.ReactorExecutor.run(ReactorExecutor.java:91)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68)
	at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
15:57:16.899 [parallel-4] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:57:16.899426+08:00]. Starting lock renewal.
15:57:16.900 [parallel-4] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:57:16.901 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":990}
15:57:17.188 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"7"}
15:57:17.188 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:57:46.978Z]. next: [PT29.789486S]. isSession[false]
15:57:17.189 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:57:36.993 [parallel-6] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:57:36.993347900+08:00]. Starting lock renewal.
15:57:36.995 [parallel-6] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:57:36.998 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":989}
15:57:37.288 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"8"}
15:57:37.288 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:58:07.073Z]. next: [PT29.7844291S]. isSession[false]
15:57:37.289 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:57:57.087 [parallel-8] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:57:57.087937400+08:00]. Starting lock renewal.
15:57:57.089 [parallel-8] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:57:57.091 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":988}
15:57:57.379 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"9"}
15:57:57.380 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:58:27.167Z]. next: [PT29.7861357S]. isSession[false]
15:57:57.380 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:58:17.175 [parallel-10] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:58:17.175882300+08:00]. Starting lock renewal.
15:58:17.177 [parallel-10] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:58:17.179 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":987}
15:58:17.463 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"10"}
15:58:17.464 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:58:47.246Z]. next: [PT29.7816679S]. isSession[false]
15:58:17.465 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:58:37.248 [parallel-12] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:58:37.248177400+08:00]. Starting lock renewal.
15:58:37.249 [parallel-12] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:58:37.251 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":986}
15:58:37.732 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"11"}
15:58:37.732 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:59:07.466Z]. next: [PT29.7330884S]. isSession[false]
15:58:37.732 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:58:57.473 [parallel-14] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:58:57.473184100+08:00]. Starting lock renewal.
15:58:57.475 [parallel-14] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:58:57.478 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":985}
15:58:57.760 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"12"}
15:58:57.760 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:59:27.550Z]. next: [PT29.7897922S]. isSession[false]
15:58:57.760 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:59:17.561 [parallel-16] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:59:17.561680+08:00]. Starting lock renewal.
15:59:17.561 [parallel-16] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:59:17.563 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":984}
15:59:17.849 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"13"}
15:59:17.850 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T07:59:47.629Z]. next: [PT29.7789951S]. isSession[false]
15:59:17.850 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:59:37.628 [parallel-2] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:59:37.628844400+08:00]. Starting lock renewal.
15:59:37.630 [parallel-2] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:59:37.631 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":983}
15:59:37.913 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"14"}
15:59:37.914 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T08:00:07.692Z]. next: [PT29.7778245S]. isSession[false]
15:59:37.914 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}
15:59:57.701 [parallel-4] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. now[2022-08-24T15:59:57.701436700+08:00]. Starting lock renewal.
15:59:57.702 [parallel-4] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Scheduling on dispatcher.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":null}
15:59:57.705 [reactor-executor-1] DEBUG c.a.c.a.i.handler.SendLinkHandler - {"az.sdk.message":"onLinkFlow.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","unsettled":0,"credits":982}
15:59:57.994 [reactor-executor-1] DEBUG c.a.c.a.i.RequestResponseChannel - {"az.sdk.message":"Settling message.","connectionId":"MF_3a9815_1661327474431","linkName":"testqueue-mgmt","messageId":"15"}
15:59:57.994 [reactor-executor-1] INFO  c.a.m.s.LockRenewalOperation - token[99d18fdf-03a8-40c5-82f2-9dffab6e037b]. nextExpiration[2022-08-24T08:00:27.771Z]. next: [PT29.7762637S]. isSession[false]
15:59:57.995 [reactor-executor-1] DEBUG c.a.c.a.i.handler.ReceiveLinkHandler - {"az.sdk.message":"onDelivery.","connectionId":"MF_3a9815_1661327474431","errorCondition":null,"errorDescription":null,"entityPath":"testqueue/$management","linkName":"testqueue-mgmt","updatedLinkCredit":0,"remoteCredit":0,"delivery.isPartial":false,"delivery.isSettled":false}

```