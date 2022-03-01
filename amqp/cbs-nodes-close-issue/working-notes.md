# Only emit AMQP channels downstream when they are active #24582

## Description

https://github.com/Azure/azure-sdk-for-java/issues/24582

Currently in AMQPChannelProcessor, when the CBS node is closed, it'll make a request upstream for a new instance. This instance is immediately emitted even if it's not active.

- In the case that the underlying connection is closed, the link will never be active.
- Consequently, there is a lot of back and forth when reconnection happens.

This should alleviate the mass of logs accumulated about retries and fix retry policy-related issues.

Massive close logs:

```
2022-02-22 15:20:02.909  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.SessionHandler         : onSessionRemoteClose connectionId[eventhub-test], entityName[MF_3765e5_1645514398813], condition[Error{condition=null, description='null', info=null}]
2022-02-22 15:20:02.909  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.SessionHandler         : onSessionRemoteClose connectionId[cbs-session], entityName[MF_3765e5_1645514398813], condition[Error{condition=null, description='null', info=null}]
2022-02-22 15:21:02.626  INFO 52784 --- [     parallel-2] c.a.c.a.i.RequestResponseChannel         : connectionId[MF_3765e5_1645514398813] linkName[cbs] Timed out waiting for RequestResponseChannel to complete closing. Manually closing.
2022-02-22 15:21:02.626  INFO 52784 --- [     parallel-1] c.a.c.a.i.RequestResponseChannel         : connectionId[MF_3765e5_1645514398813] linkName[cbs] Timed out waiting for RequestResponseChannel to complete closing. Manually closing.
2022-02-22 15:21:02.626  INFO 52784 --- [     parallel-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Channel is closed. Requesting upstream. 
2022-02-22 15:21:02.626  INFO 52784 --- [     parallel-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Connection not requested, yet. Requesting one.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.ReactorConnection              : connectionId[MF_3765e5_1645514398813] Closing executor.
2022-02-22 15:21:02.626  INFO 52784 --- [     parallel-1] c.a.c.a.i.ReactorConnection              : connectionId[MF_3765e5_1645514398813] entityPath[$cbs] linkName[cbs] Emitting new response channel.
2022-02-22 15:21:02.626  INFO 52784 --- [     parallel-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Setting next AMQP channel.
2022-02-22 15:21:02.626  INFO 52784 --- [     parallel-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Next AMQP channel received, updating 1 current subscribers
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.ConnectionHandler      : onConnectionLocalClose connectionId[MF_3765e5_1645514398813] hostname[eventhubs-liuku.servicebus.windows.net] errorCondition[null] errorDescription[null]
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.SendLinkHandler        : connectionId[MF_3765e5_1645514398813] linkName[cbs] entityPath[$cbs] Sender link was never active. Closing endpoint states.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.ReceiveLinkHandler     : connectionId[MF_3765e5_1645514398813] linkName[cbs] entityPath[$cbs] Receiver link was never active. Closing endpoint states.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Channel is closed. Requesting upstream. 
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Connection not requested, yet. Requesting one.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.ReactorConnection              : connectionId[MF_3765e5_1645514398813] entityPath[$cbs] linkName[cbs] Emitting new response channel.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Setting next AMQP channel.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Next AMQP channel received, updating 1 current subscribers
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.SendLinkHandler        : connectionId[MF_3765e5_1645514398813] linkName[cbs] entityPath[$cbs] Sender link was never active. Closing endpoint states.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.ReceiveLinkHandler     : connectionId[MF_3765e5_1645514398813] linkName[cbs] entityPath[$cbs] Receiver link was never active. Closing endpoint states.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Channel is closed. Requesting upstream. 
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Connection not requested, yet. Requesting one.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.ReactorConnection              : connectionId[MF_3765e5_1645514398813] entityPath[$cbs] linkName[cbs] Emitting new response channel.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Setting next AMQP channel.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Next AMQP channel received, updating 1 current subscribers
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.SendLinkHandler        : connectionId[MF_3765e5_1645514398813] linkName[cbs] entityPath[$cbs] Sender link was never active. Closing endpoint states.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.handler.ReceiveLinkHandler     : connectionId[MF_3765e5_1645514398813] linkName[cbs] entityPath[$cbs] Receiver link was never active. Closing endpoint states.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Channel is closed. Requesting upstream. 
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.RequestResponseChannel:$cbs    : namespace[MF_3765e5_1645514398813] entityPath[$cbs]: Connection not requested, yet. Requesting one.
2022-02-22 15:21:02.626  INFO 52784 --- [ctor-executor-1] c.a.c.a.i.ReactorConnection              : connectionId[MF_3765e5_1645514398813] entityPath[$cbs] linkName[cbs] Emitting new response channel.
```

## Root cause

call graphs

![img](./cbs-note-close-issue.png
)

Use `requestUpStream()` will involve infinite loop.

