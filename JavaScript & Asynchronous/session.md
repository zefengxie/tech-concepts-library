# Session

## What a Session Is

A session is a server-side mechanism for maintaining state across multiple HTTP requests
from the same client. It allows the server to associate requests with a specific user
without requiring the client to resend all state information each time.

---

## Why Sessions Exist

HTTP is stateless.
Sessions provide:
- Authentication persistence
- User-specific state
- Temporary data storage across requests
- Server-controlled security

---

## How Sessions Work

1. Client sends initial request (e.g., login)
2. Server creates a session object in memory or storage
3. Server generates a session identifier
4. Session ID is sent to the client (usually via a cookie)
5. Client includes session ID in subsequent requests
6. Server uses the ID to retrieve session data

---

## Session Identifier

The session ID:
- Is a unique, random value
- Identifies session data on the server
- Must be unpredictable

Typical transport:
```http
Set-Cookie: sessionId=abc123; HttpOnly; Secure
```

---

## Session Storage Strategies

### In-Memory
- Fast
- Lost on server restart
- Not scalable without sticky sessions

---

### Database
- Persistent
- Slower access
- Easier to share across servers

---

### Distributed Cache (Redis, Memcached)
- Fast
- Scalable
- Common in production systems

---

## Session vs Cookies

Cookies:
- Stored on the client
- Can carry session IDs

Sessions:
- Stored on the server
- Identified by session IDs

They work together but are not the same.

---

## Session Lifecycle

- Created on authentication
- Updated on each request
- Expired after inactivity
- Explicitly destroyed on logout

---

## Security Considerations

- Use HTTPS
- Regenerate session ID after login
- Use HttpOnly and Secure flags
- Set appropriate expiration
- Protect against session fixation and hijacking

---

## Stateless vs Stateful Systems

Sessions introduce statefulness:
- Harder to scale
- Requires shared storage or sticky sessions

Stateless alternatives:
- Token-based authentication (JWT)

---

## Common Mistakes

- Storing sensitive data directly in cookies
- Long session lifetimes
- Not invalidating sessions on logout
- Using predictable session IDs

---

## Interview Questions

- How do sessions work?
- How are sessions different from cookies?
- Where is session data stored?
- How do you scale session-based systems?
- What security issues affect sessions?

---

## Key Takeaways

- Sessions maintain server-side state
- Identified via session IDs
- Often implemented using cookies
- Security and scalability are primary concerns
