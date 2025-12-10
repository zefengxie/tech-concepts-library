# JWT (JSON Web Token)

## 1. Formal Definition
**JWT (JSON Web Token)** is a compact, URL-safe token format used to securely transmit claims between parties.
It is commonly used for **stateless authentication** in backend systems.

In simple terms:
> A JWT is a signed token that proves who you are and what you are allowed to do.

---

## 2. Why It Matters for Backend Development
JWT is widely used because it:
- Enables stateless authentication
- Scales well across distributed systems
- Reduces server-side session storage
- Works naturally with HTTP APIs

JWTs are commonly used in:
- REST APIs
- Microservices
- Mobile applications
- Single Page Applications (SPAs)

---

## 3. JWT Structure
A JWT consists of **three parts** separated by dots:

```
header.payload.signature
```

- **Header**: token type & signing algorithm
- **Payload**: claims (user data, permissions, metadata)
- **Signature**: ensures integrity and authenticity

JWTs are **signed**, not encrypted by default.

---

## 4. Good Example: JWT Usage Flow

1. User logs in with credentials
2. Server validates credentials
3. Server issues a JWT
4. Client stores token (memory or secure storage)
5. Client sends JWT in `Authorization` header
6. Server verifies token on each request

```http
Authorization: Bearer <token>
```

Why this is good:
- No server-side session state
- Simple horizontal scaling
- Fast verification

---

## 5. Good Example: JWT Verification

```js
function authenticate(req, res, next) {
  const token = req.headers.authorization?.split(" ")[1];
  if (!token) return res.status(401).send("Unauthenticated");

  const payload = jwt.verify(token, process.env.JWT_SECRET);
  req.user = payload;
  next();
}
```

Why this is good:
- Token verification centralized
- Clear failure handling
- No database lookup required

---

## 6. Bad Example: Storing Sensitive Data in JWT

```js
payload = {
  userId: 1,
  password: "plaintext-password"
};
```

Problems:
- JWT payload is base64-encoded, not encrypted
- Anyone with token can read payload
- Severe security risk

JWT payloads should contain **minimal, non-sensitive data**.

---

## 7. JWT vs Session Authentication

| Aspect | JWT | Session |
|----|----|----|
| Server storage | None | Required |
| Scalability | High | Medium |
| Revocation | Hard | Easy |
| Payload | Client-stored | Server-stored |

JWT improves scalability but complicates revocation.

---

## 8. JWT Security Best Practices
- Use strong secrets or asymmetric keys
- Set short expiration times
- Use refresh tokens
- Validate issuer and audience
- Never trust token payload blindly
- Use HTTPS only

JWT security failures are often catastrophic.

---

## 9. Common Backend Mistakes

- Long-lived tokens without rotation
- No token expiration
- Using weak secrets
- Putting authorization logic inside JWTs
- Assuming JWTs cannot be tampered with

JWTs are safe **only when verified correctly**.

---

## 10. Interview Questions You Should Expect

1. What is JWT?
2. How does JWT differ from sessions?
3. What is inside a JWT?
4. Are JWTs encrypted?
5. How do you revoke JWTs?
6. What are refresh tokens?
7. Where should JWTs be stored on the client?
8. What security risks exist with JWT?
9. Stateless vs stateful authentication?
10. When should you avoid JWT?

---

## 11. Answer Template for Interviews

> “JWT is a signed token format used for stateless authentication.
> It allows servers to verify identity without storing session state.
> JWTs scale well but require careful handling of expiration, rotation, and security.”

---

## 12. One-Minute Summary

- JWT is a stateless authentication token
- Signed, not encrypted by default
- Scales well for distributed systems
- Harder to revoke than sessions
- Must be implemented carefully
