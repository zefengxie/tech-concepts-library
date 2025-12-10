# WebSocket (Backend)

## 1. Formal Definition
**WebSocket** is a communication protocol that provides **full-duplex, persistent connections** over a single TCP connection.
It enables real-time, bidirectional communication between client and server.

In simple terms:
> WebSocket keeps a connection open so both sides can send messages at any time.

---

## 2. Why It Matters for Backend Development
WebSockets matter because they:
- Enable real-time features
- Reduce latency compared to polling
- Support bidirectional communication
- Lower overhead for frequent updates

Common use cases:
- Chat applications
- Live notifications
- Online gaming
- Real-time dashboards
- Collaborative editing

---

## 3. How WebSocket Works
Basic flow:
1. Client sends an HTTP request with an **Upgrade** header
2. Server accepts and upgrades the connection
3. HTTP connection becomes a persistent WebSocket connection
4. Messages flow both ways until closed

After the handshake, communication is no longer HTTP.

---

## 4. Good Example: WebSocket Server

```js
const WebSocket = require("ws");
const wss = new WebSocket.Server({ port: 8080 });

wss.on("connection", (ws) => {
  ws.on("message", (msg) => {
    ws.send(`Echo: ${msg}`);
  });
});
```

Why this is good:
- Persistent connection
- Event-driven communication
- Low latency
- Efficient for frequent messages

---

## 5. Bad Example: Using WebSocket for Simple CRUD

```js
ws.send(JSON.stringify({ action: "getUser", id: 1 }));
```

Problems:
- Reinvents REST
- Harder to cache
- More complex error handling
- Poor tooling support

WebSockets should not replace REST for standard CRUD.

---

## 6. WebSocket vs HTTP Polling

| Aspect | WebSocket | Polling |
|---|---|---|
| Connection | Persistent | Repeated |
| Latency | Low | High |
| Server load | Low | Higher |
| Complexity | Medium | Low |

WebSockets are better for frequent updates.

---

## 7. Scaling WebSockets
Challenges:
- Connections are stateful
- Load balancers must support sticky sessions or shared state
- Broadcast requires coordination

Common solutions:
- Shared message brokers (Redis Pub/Sub)
- Dedicated WebSocket gateways
- Connection sharding

---

## 8. Security Considerations
Best practices:
- Authenticate during handshake
- Authorize per message
- Use WSS (TLS)
- Validate message payloads
- Limit message rates

WebSockets bypass many HTTP protections.

---

## 9. Common Backend Mistakes
- No authentication
- Using WebSocket for infrequent requests
- Blocking event loop on messages
- No heartbeat or ping handling
- Ignoring backpressure

Broken WebSocket implementations cause resource leaks.

---

## 10. Interview Questions You Should Expect
1. What is WebSocket?
2. WebSocket vs HTTP?
3. How does WebSocket handshake work?
4. When should you use WebSockets?
5. How do you scale WebSockets?
6. How do you authenticate WebSocket connections?
7. What is backpressure?
8. WebSocket vs Server-Sent Events?
9. What are WebSocket limitations?
10. Common WebSocket pitfalls?

---

## 11. Answer Template for Interviews

> “WebSocket is a protocol that enables persistent, bidirectional communication between client and server.
> It is ideal for real-time applications such as chat or live updates.
> WebSockets require careful handling of scaling, authentication, and connection management.”

---

## 12. One-Minute Summary

- WebSocket enables real-time communication
- Persistent, bidirectional connection
- Not suitable for standard CRUD
- Harder to scale than HTTP
- Essential for real-time backends
