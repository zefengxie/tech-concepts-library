# Long Polling

## What Long Polling Is

Long polling is a client–server communication technique where the client sends a request
and the server holds the connection open until new data is available or a timeout occurs.

It improves upon traditional polling by reducing unnecessary requests.

---

## Why Long Polling Exists

Standard polling wastes resources by repeatedly asking for updates.
Long polling allows:
- Lower latency than polling
- Fewer empty responses
- Near real-time updates without WebSockets

---

## How Long Polling Works

1. Client sends a request
2. Server waits until data is available
3. Server responds immediately when data arrives
4. Client processes the response
5. Client instantly sends a new request

This creates a continuous request cycle.

---

## Client-Side Example

```js
async function poll() {
  const res = await fetch("/api/updates");
  const data = await res.json();
  updateUI(data);
  poll(); // immediately re-open connection
}

poll();
```

---

## Server-Side Behavior

- Hold request open
- Respond only when data is ready or timeout expires
- Close connection after responding

---

## Timeout Handling

Servers typically set a maximum wait time.
If no data is available:
- Server responds with empty or heartbeat response
- Client re-initiates request

---

## Long Polling vs Polling

Polling:
- Fixed intervals
- High number of empty responses
- Higher latency

Long Polling:
- Response only when data is available
- Fewer requests
- Lower latency

---

## Long Polling vs SSE

| Long Polling | SSE |
|-------------|-----|
| Repeated HTTP requests | Single persistent connection |
| More overhead | Less overhead |
| Client-managed reconnect | Automatic reconnect |
| Wider legacy support | Browser-supported |

---

## Long Polling vs WebSockets

| Long Polling | WebSockets |
|-------------|-----------|
| One-way or pseudo two-way | Full duplex |
| HTTP-based | Persistent socket |
| More overhead | More efficient |

---

## Scalability Considerations

- Each client holds a connection
- Requires async/non-blocking servers
- Reverse proxies must support long-lived requests

---

## Common Mistakes

- Forgetting to immediately re-poll
- Blocking server threads
- Very long timeouts
- Using long polling for high-frequency updates

---

## When to Use Long Polling

- Legacy environments
- When SSE/WebSockets are unavailable
- Medium-frequency updates
- Transitional architectures

---

## Interview Questions

- How does long polling work?
- Difference between polling and long polling?
- Long polling vs SSE?
- What are scalability concerns with long polling?
- Why does long polling reduce latency?

---

## Key Takeaways

- Long polling keeps requests open
- Reduces wasted requests
- Near real-time updates
- Still less efficient than SSE or WebSockets
