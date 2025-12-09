# Token

## What a Token Is

A token is a piece of data issued by an authentication system that represents a user, client, or access grant.
Tokens are commonly used to authenticate requests in stateless systems.

Unlike sessions, tokens usually store or encode the necessary information themselves.

---

## Why Tokens Exist

Tokens enable:
- Stateless authentication
- Horizontal scalability
- Cross-domain and mobile-friendly auth
- Decoupling authentication from storage

They remove the need for server-side session state.

---

## Common Token Types

### Access Token
Used to access protected resources.

Characteristics:
- Short-lived
- Sent with every request
- Often stored in memory or cookies

---

### Refresh Token
Used to obtain a new access token.

Characteristics:
- Long-lived
- Stored securely
- Never sent to resource servers

---

### JWT (JSON Web Token)

Structure:
```
header.payload.signature
```

- Header: algorithm and type
- Payload: claims (user info, scope, exp)
- Signature: integrity verification

JWTs are self-contained and verifiable without server storage.

---

## How Token Authentication Works

1. User authenticates
2. Server issues token
3. Client stores token
4. Client sends token with requests
5. Server validates token
6. Access granted or denied

Typical usage:
```http
Authorization: Bearer <token>
```

---

## Token Storage Options

### In Memory
- Most secure against XSS persistence
- Lost on refresh

### HttpOnly Cookies
- Protected from XSS
- Vulnerable to CSRF if misconfigured

### LocalStorage
- Persistent
- Vulnerable to XSS
- Generally discouraged for auth tokens

---

## Token Validation

Validation may include:
- Signature verification
- Expiration check
- Issuer and audience checks
- Scope or role validation

---

## Tokens vs Sessions

| Tokens | Sessions |
|------|----------|
| Stateless | Stateful |
| Scalable | Requires shared storage |
| Self-contained | Server-managed |
| Harder to revoke | Easy to invalidate |

---

## Security Considerations

- Always use HTTPS
- Keep access tokens short-lived
- Rotate refresh tokens
- Protect against XSS and CSRF
- Avoid storing sensitive data in token payloads

---

## Common Mistakes

- Long-lived access tokens
- Storing tokens in localStorage without protection
- Not validating token claims
- Exposing tokens to third-party scripts

---

## Interview Questions

- What is a token?
- Token vs session?
- How does JWT work?
- Where should tokens be stored?
- How do you revoke tokens?

---

## Key Takeaways

- Tokens enable stateless authentication
- Commonly used with Authorization headers
- JWTs are self-contained tokens
- Secure storage and validation are critical
