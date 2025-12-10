# CORS (Cross-Origin Resource Sharing)

## 1. Formal Definition
**CORS (Cross-Origin Resource Sharing)** is a browser security mechanism that controls how web pages from one origin can request resources from another origin.
It is enforced by browsers, not servers.

In simple terms:
> CORS defines when a browser is allowed to call your backend from another domain.

---

## 2. Why It Matters for Backend Development
CORS matters because it:
- Directly affects frontend-backend communication
- Prevents unauthorized cross-origin access
- Is a frequent source of frontend-backend bugs
- Must be configured correctly for APIs

Many “API not working” issues are actually CORS misconfigurations.

---

## 3. What Is an Origin
An **origin** is defined by three parts:
- Scheme (http / https)
- Host (domain)
- Port

Examples:
- `https://example.com` ✅
- `https://example.com:3000` ❌ (different port → different origin)

CORS applies only when origins differ.

---

## 4. How CORS Works (Conceptual Flow)

1. Browser sends a request to another origin
2. Server responds with CORS headers
3. Browser decides whether to allow or block the response
4. If blocked, the request still reaches the server, but the response is hidden

CORS is a **browser enforcement mechanism**.

---

## 5. Good Example: Proper CORS Configuration

```js
app.use(cors({
  origin: "https://myfrontend.com",
  methods: ["GET", "POST", "PUT", "DELETE"],
  credentials: true
}));
```

Why this is good:
- Explicit allowed origin
- Minimal permissions
- Supports authenticated requests
- Safer than wildcard

---

## 6. Bad Example: Over-Permissive CORS

```js
app.use(cors({
  origin: "*",
  credentials: true
}));
```

Problems:
- Invalid configuration
- Security risk
- Browsers will reject this
- Signals poor security understanding

Never combine `origin: "*"` with credentials.

---

## 7. Preflight Requests
Browsers send a **preflight request** when:
- Method is not GET/POST
- Custom headers are used
- Credentials are included

Preflight uses:
```http
OPTIONS /api/resource
```

Server must respond correctly for the real request to proceed.

---

## 8. Common CORS Headers
Key headers include:
- `Access-Control-Allow-Origin`
- `Access-Control-Allow-Methods`
- `Access-Control-Allow-Headers`
- `Access-Control-Allow-Credentials`
- `Access-Control-Max-Age`

Missing or incorrect headers cause browser blocks.

---

## 9. Common Backend Mistakes

- Thinking CORS is a server security feature
- Blocking CORS instead of fixing config
- Allowing everything in production
- Debugging CORS on server logs only
- Forgetting preflight support

CORS bugs are frontend-visible but backend-owned.

---

## 10. Interview Questions You Should Expect

1. What is CORS?
2. Is CORS enforced by browser or server?
3. What is an origin?
4. What is a preflight request?
5. Why does my API work in Postman but not browser?
6. Can CORS prevent server attacks?
7. What does `credentials: true` mean?
8. When is preflight triggered?
9. Can CORS be disabled?
10. CORS vs CSRF?

---

## 11. Answer Template for Interviews

> “CORS is a browser-enforced security mechanism that controls cross-origin requests.
> The server provides CORS headers, but the browser decides whether to allow access.
> Correct configuration is essential for frontend-backend integration.”

---

## 12. One-Minute Summary

- CORS controls browser cross-origin requests
- Enforced by browsers, not servers
- Based on origin comparison
- Preflight requests are common
- Misconfiguration is a common backend issue
