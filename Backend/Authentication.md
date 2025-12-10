# Authentication (Backend)

## 1. Formal Definition
**Authentication** is the process of verifying the identity of a user or system.
It answers the question: *Who are you?*

In backend systems, authentication ensures that a request is made by a known and verified entity before access is granted.

---

## 2. Why It Matters for Backend Development
Authentication is critical because it:
- Protects sensitive data and operations
- Prevents unauthorized access
- Enables user-specific behavior
- Forms the foundation of access control systems

Without proper authentication, authorization and security controls are meaningless.

---

## 3. Authentication vs Authorization
These concepts are related but distinct:

- **Authentication**: Who you are
- **Authorization**: What you are allowed to do

Authentication always comes **before** authorization.

---

## 4. Common Authentication Methods

### Username & Password
- Most common
- Requires secure password storage (hashing + salting)

### Token-Based Authentication
- JWT, opaque tokens
- Stateless and scalable

### Session-Based Authentication
- Server-side sessions
- Requires session storage

### OAuth / SSO
- Delegated authentication
- Used with third-party identity providers

---

## 5. Good Example: Token-Based Authentication

```js
function authenticate(req, res, next) {
  const token = req.headers.authorization?.split(" ")[1];
  if (!token) return res.status(401).send("Unauthenticated");

  req.user = verifyToken(token);
  next();
}
```

Why this is good:
- Stateless
- Scales horizontally
- Clear separation of concerns
- Easy to integrate with middleware

---

## 6. Bad Example: Plaintext Password Handling

```js
if (req.body.password === user.password) {
  // authenticated
}
```

Problems:
- Insecure password storage
- Vulnerable to breaches
- Violates basic security principles

Passwords must **never** be stored or compared in plaintext.

---

## 7. Password Security Best Practices
- Use strong hashing algorithms (bcrypt, argon2)
- Apply salting
- Enforce password complexity
- Support password rotation
- Implement account lockout policies

Passwords are secrets and must be treated as such.

---

## 8. Stateless vs Stateful Authentication

| Aspect | Stateless (JWT) | Stateful (Session) |
|-----|-----|-----|
| Server storage | None | Required |
| Scalability | High | Medium |
| Revocation | Hard | Easy |
| Complexity | Medium | Low |

Choice depends on scale and security requirements.

---

## 9. Common Backend Mistakes

- Confusing authentication with authorization
- Storing passwords incorrectly
- Hardcoding secrets
- Skipping token expiration
- Not validating authentication inputs

Authentication bugs often lead to severe security breaches.

---

## 10. Interview Questions You Should Expect

1. What is authentication?
2. Authentication vs authorization?
3. How do passwords get stored securely?
4. JWT vs session authentication?
5. How does token expiration work?
6. What is OAuth?
7. How do you revoke access?
8. What are common authentication attacks?
9. How do you secure login endpoints?
10. Where should authentication logic live?

---

## 11. Answer Template for Interviews

> “Authentication is the process of verifying a user's identity before granting access.
> It is implemented using methods such as passwords, tokens, or third-party providers.
> Strong authentication practices are critical for backend security and must be implemented before authorization.”

---

## 12. One-Minute Summary

- Authentication verifies identity
- Foundation of backend security
- Comes before authorization
- Must be implemented securely
- Essential for protecting systems and data
