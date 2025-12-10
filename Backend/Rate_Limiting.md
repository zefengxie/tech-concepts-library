# Rate Limiting (Backend)

## 1. Formal Definition
**Rate Limiting** is a technique used to control how many requests a client can make to a backend service within a given time window.
It protects systems from abuse, overload, and denial-of-service scenarios.

In simple terms:
> Rate limiting restricts how often clients can call your API.

---

## 2. Why It Matters for Backend Development
Rate limiting is critical because it:
- Protects availability and stability
- Prevents abuse and brute-force attacks
- Controls operational costs
- Ensures fair usage among clients

Without rate limiting, a single client can degrade service for everyone.

---

## 3. Common Rate Limiting Strategies
Typical strategies include:
- **Fixed Window**: counts requests in a fixed time bucket
- **Sliding Window**: smoother limits using rolling windows
- **Token Bucket**: allows bursts within capacity
- **Leaky Bucket**: enforces steady request flow

The choice affects fairness and burst behavior.

---

## 4. Good Example: Token Bucket Limiting

```js
// Pseudocode
if (bucket.tokens > 0) {
  bucket.tokens--;
  allowRequest();
} else {
  rejectRequest(429);
}
```

Why this is good:
- Allows short bursts
- Predictable limits
- Works well for APIs with variable traffic
- Widely adopted in production systems

---

## 5. Bad Example: Per-Process In-Memory Limiting

```js
const count = {};
count[ip] = (count[ip] || 0) + 1;
```

Problems:
- Breaks in multi-instance deployments
- Resets on restart
- Easy to bypass
- Not horizontally scalable

Distributed systems require shared state.

---

## 6. Rate Limiting vs Throttling
Related but different:

| Aspect | Rate Limiting | Throttling |
|---|---|---|
| Purpose | Block excess requests | Slow down excess requests |
| Response | Reject (429) | Delay responses |
| Complexity | Lower | Higher |

APIs often use rate limiting; internal systems may throttle.

---

## 7. Where to Apply Rate Limiting
Common enforcement points:
- API gateway
- Load balancer
- Reverse proxy
- Application middleware

Applying limits closer to the edge reduces backend load.

---

## 8. Identifying Clients
Rate limits are typically keyed by:
- IP address
- API key
- User ID
- Client ID

Best practice:
- Prefer authenticated identifiers over IPs
- Combine signals when possible

---

## 9. Common Backend Mistakes

- No rate limiting on auth endpoints
- Using IP-only limits for authenticated APIs
- Not returning proper status codes
- Inconsistent limits across services
- No visibility into blocked traffic

Authentication endpoints are high-risk and must be protected.

---

## 10. Interview Questions You Should Expect

1. What is rate limiting?
2. Why is rate limiting important?
3. Token bucket vs leaky bucket?
4. How do you rate limit in distributed systems?
5. Where should rate limiting be applied?
6. How do you identify clients?
7. What HTTP status code is used?
8. Rate limiting vs throttling?
9. How do you tune limits?
10. What endpoints need the strictest limits?

---

## 11. Answer Template for Interviews

> “Rate limiting controls how many requests a client can make in a given time window.
> It protects backend services from abuse and overload.
> In production systems, rate limiting is usually enforced at the edge using distributed stores.”

---

## 12. One-Minute Summary

- Rate limiting controls request frequency
- Protects availability and security
- Multiple algorithms exist
- Must be distributed to scale
- Essential for production backends
