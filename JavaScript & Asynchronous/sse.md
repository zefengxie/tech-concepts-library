# Server-Sent Events (SSE)

## What SSE Is

Server-Sent Events (SSE) is a web technology that allows a server to push real-time updates to a client over a single, long-lived HTTP connection.
It is a one-way communication channel: server → client.

SSE is built on standard HTTP and is natively supported by browsers.

---

## The Problem SSE Solves

Traditional HTTP is request–response based.
For real-time updates, repeatedly polling the server is inefficient.

SSE enables:
- Real-time notifications
- Live updates
- Reduced network overhead
- Simpler alternative to WebSockets for one-way streams

---

## How SSE Works

1. Client opens an HTTP connection using EventSource
2. Server keeps the connection open
3. Server sends events as text streams
4. Client receives updates automatically
5. Connection is automatically re-established if dropped

---

## Client-Side Example

```js
const source = new EventSource("/events");

source.onmessage = event => {
  console.log(event.data);
};

source.onerror = () => {
  console.error("Connection lost");
};
```

---

## Server-Side Example

```http
HTTP/1.1 200 OK
Content-Type: text/event-stream
Cache-Control: no-cache
Connection: keep-alive
```

Event format:
```
data: Hello world

data: Another message
```

Each event is separated by a blank line.

---

## Event Fields

- data: payload
- id: event identifier
- event: event type
- retry: reconnection delay

Example:
```
id: 42
event: update
data: {"status":"ok"}
```

---

## Connection Characteristics

- Uses a single HTTP connection
- Text-based protocol
- Automatically reconnects
- Supports event IDs for resuming streams

---

## SSE vs Polling

Polling:
- Client repeatedly requests data
- High overhead
- Latency between polls

SSE:
- Server pushes data immediately
- Lower overhead
- Real-time delivery

---

## SSE vs WebSockets

| SSE | WebSockets |
|----|-----------|
| One-way | Bi-directional |
| HTTP-based | Custom protocol |
| Simple | More complex |
| Auto-reconnect | Manual reconnect |
| Text only | Binary support |

---

## When to Use SSE

- Notifications
- Live dashboards
- Progress updates
- News feeds
- Logging streams

---

## Limitations

- One-way communication
- Max concurrent connections per browser
- Not ideal for high-frequency bidirectional data
- Requires proper proxy support

---

## Security Considerations

- Use HTTPS
- Authenticate connections
- Validate user permissions
- Prevent data leakage
- Handle CORS correctly

---

## Common Mistakes

- Forgetting correct Content-Type
- Using SSE for bidirectional use cases
- Sending large payloads
- Not handling disconnects gracefully

---

## Interview Questions

- What is SSE?
- SSE vs WebSockets?
- How does SSE handle reconnections?
- How is SSE different from polling?
- What are SSE limitations?

---

## Key Takeaways

- SSE enables server-to-client real-time streaming
- Built on standard HTTP
- Simple and reliable for one-way updates
- Good alternative to WebSockets in many cases
