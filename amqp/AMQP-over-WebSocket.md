# AMQP over WebSocket


[Advanced Message Queuing Protocol (AMQP) WebSocket Binding (WSB) Version 1.0](http://docs.oasis-open.org/amqp-bindmap/amqp-wsb/v1.0/amqp-wsb-v1.0.html)

## Introducation

WebSocket protocol can be used as a transport for AMQP 1.0 protocol traffic.

Use scenarios

- **Firewall traversal.** Since Websocket connection establishment is implemented as standard HTTP traffic using the default ports (80 and 443), it is often able to pass through network security devices without requiring special configuration or opening of additional ports. In this scenario, the AMQP communication can be between arbitrary AMQP peers, e.g., between an application using an AMQP client library and a message broker.

- **Browser-based messaging.** Since many Web browsers have built-in support for the WebSocket protocol, then this binding can be used to enable AMQP messaging through to the browser thereby enable a broad range of browser-based messaging scenarios.



## Layer Structure

AMQP WebSocket endpoint -> HTTP server(port 80)

       AMQP
        |
     WebSocket 
        |
      TCP/IP


AMQP WebSocket endpoint -> HTTPS server(port 443）

       AMQP
        |
     WebSocket 
        |
    SSL(old)/TLS
        |
      TCP/IP 


## Opening a Connection 

**1. Opening a WebSocket Connection**

The WebSocket Protocol connection MUST be opened as described in Section 4 of the WebSocket specification [[RFC6455]](https://datatracker.ietf.org/doc/html/rfc6455#section-4).


WebSocket use HTTP/HTTPS protocol for handshake, and upgrade to the **AMQP WebSocket binding subprotocol**:

- Request
  ```
  GET /examplepath HTTP/1.1
  Host: server.example.com
  Upgrade: websocket
  Connection: Upgrade
  Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
  Sec-WebSocket-Protocol: amqp
  Sec-WebSocket-Version: 13
  ```
- Response 
  ```
  HTTP/1.1 101 Switching Protocols
  Upgrade: websocket
  Connection: Upgrade
  Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
  Sec-WebSocket-Protocol: amqp
  ```

**2. Establishing a SASL Security Layer**
**3. AMQP Protocol Version Handshake**
**4. Exchanging AMQP Frames**
**5. Opening an AMQP Connection**

one AMQP frame would be separated to many WebSocket frames and sent out.


## Qpid-proton-j-extension project

https://github.com/Azure/qpid-proton-j-extensions


- ws - Implement Websocket protocol over AMQP (proton-j)
  - WebSocketImpl  
    - Define state and websocket workflow
  - WebSocketHandlerImpl 
    - Handle state for web socket
  - WebSocketUpgrage 
    - Used for websocket upgrade hand-shake with http server
  - WebSocketSniffer 
    - Tansport layer, extends pronton-j HandShakeSniffingTransportWrapper 

- proxy
  - ProxyImpl	
  - ProxyHandleImpl
  - ProxyResponseImpl
  - BasicProxyChanllengeProcessorImpl
  - DigestProxyChallengeProcessorImpl


azure-amqp-core project
- WebSocketConnectionHandler
- WebSocketProxyConnectionHandler
  - Add proxy up to websocket transport layer