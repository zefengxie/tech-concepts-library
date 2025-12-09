# Cookies

## What Cookies Are

Cookies are small pieces of data stored by the browser and sent with HTTP requests to the server.
They are primarily used to maintain state in an otherwise stateless HTTP protocol.

Cookies are shared between the browser and server and are scoped by domain and path.

---

## Why Cookies Exist

HTTP is stateless.
Cookies allow servers to:
- Identify clients across requests
- Maintain authentication sessions
- Store user preferences
- Track behavior (analytics)

---

## Cookie Structure

```http
Set-Cookie: sessionId=abc123; Path=/; HttpOnly; Secure; SameSite=Lax
```

Components:
- Name/value pair
- Attributes controlling scope, lifetime, and security

---

## Cookie Attributes

### Domain
Controls which domains receive the cookie.

```http
Domain=example.com
```

---

### Path
Restricts cookies to specific URL paths.

```http
Path=/api
```

---

### Expires / Max-Age
Defines cookie lifetime.

```http
Expires=Wed, 21 Oct 2026 07:28:00 GMT
Max-Age=3600
```

Session cookies expire when the browser closes.

---

### Secure
Cookie is only sent over HTTPS connections.

---

### HttpOnly
Cookie is inaccessible to JavaScript.
Protects against XSS attacks.

---

### SameSite
Controls cross-site cookie behavior.

Values:
- Strict
- Lax
- None (requires Secure)

Used to mitigate CSRF attacks.

---

## How Cookies Are Sent

- Server sets cookie via `Set-Cookie`
- Browser stores cookie
- Browser sends cookie automatically in `Cookie` header

```http
Cookie: sessionId=abc123
```

---

## Cookies vs Local Storage

| Cookies | LocalStorage |
|-------|--------------|
| Sent with every request | Client-side only |
| Limited size (~4KB) | Larger capacity |
| Server-readable | Not sent to server |
| Used for auth | Used for UI state |

---

## Security Considerations

- Always use Secure in production
- Use HttpOnly for session identifiers
- Configure SameSite correctly
- Avoid storing sensitive data directly
- Prefer short lifetimes

---

## Common Mistakes

- Storing JWTs in non-HttpOnly cookies
- Overly permissive Domain attributes
- Missing SameSite on auth cookies
- Using cookies for large payloads

---

## When Cookies Are Not Ideal

- Pure stateless APIs
- Mobile-first token-based auth
- Large client-side datasets

---

## Interview Questions

- How do cookies work?
- What is the difference between HttpOnly and Secure?
- What problem does SameSite solve?
- Cookies vs tokens?
- How do cookies enable sessions?

---

## Key Takeaways

- Cookies persist client identity
- Automatically sent with requests
- Powerful but security-sensitive
- Correct configuration is critical
