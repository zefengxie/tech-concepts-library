# Polling

## What Polling Is

Polling is a client-driven technique where the client repeatedly sends requests to the server at fixed intervals
to check whether new data is available.

It is one of the simplest approaches for simulating real-time updates over HTTP.

---

## Why Polling Exists

HTTP is request–response based.
Polling allows clients to:
- Detect updates without server push
- Work with legacy infrastructure
- Avoid long-lived connections

Polling trades efficiency for simplicity.

---

## How Polling Works

1. Client sends a request to the server
2. Server responds immediately with current data
3. Client waits for a fixed interval
4. Client repeats the request

---

## Client-Side Example

```js
setInterval(async () => {
  const res = await fetch("/api/status");
  const data = await res.json();
  updateUI(data);
}, 5000);
```

---

## Polling Frequency

Choosing the interval is critical:
- Too frequent → unnecessary load
- Too slow → stale data

Polling frequency determines:
- Latency
- Server load
- Network usage

---

## Advantages

- Simple to implement
- Works everywhere
- Easy to reason about
- Compatible with caches and proxies

---

## Disadvantages

- Wasted requests when no data changes
- Increased latency
- Higher server and network cost
- Poor scalability at scale

---

## Polling vs Short Polling vs Long Polling

Standard Polling:
- Fixed interval requests
- Server responds immediately

Short Polling:
- Essentially the same as standard polling

Long Polling:
- Server delays response until data is available or timeout

---

## Polling vs SSE vs WebSockets

| Technique | Direction | Efficiency |
|---------|-----------|------------|
| Polling | Client → Server | Low |
| SSE | Server → Client | Medium |
| WebSockets | Bi-directional | High |

---

## When to Use Polling

- Simple status checks
- Low-frequency updates
- Legacy systems
- Environments without SSE/WebSocket support

---

## Common Mistakes

- Polling too frequently
- Ignoring backoff strategies
- Not canceling polling when UI is inactive
- Using polling for high-frequency updates

---

## Improvements

- Adaptive polling intervals
- Exponential backoff
- Conditional requests (ETag, If-Modified-Since)
- Switching to push-based mechanisms

---

## Interview Questions

- What is polling?
- What are the drawbacks of polling?
- How does polling differ from long polling?
- When is polling acceptable?
- How would you optimize a polling system?

---

## Key Takeaways

- Polling is client-initiated
- Simple but inefficient
- Scales poorly
- Often replaced by SSE or WebSockets
