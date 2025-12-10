# Endpoint (Backend)

## 1. Formal Definition
An **Endpoint** is a specific network address (URL) combined with an HTTP method that represents a callable entry point into a backend system.
It is where a client request is received and processed.

In simple terms:
> An endpoint is the exact place where a request hits your backend.

---

## 2. Why It Matters for Backend Development
Endpoints matter because they:
- Define the public surface area of your backend
- Control access to business functionality
- Shape API usability and maintainability
- Impact security, performance, and scalability

Well-designed endpoints are predictable and easy to consume; poorly designed ones create confusion and bugs.

---

## 3. Endpoint Components
An endpoint is defined by:
- **Path**: `/users/{id}`
- **HTTP Method**: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
- **Headers**: auth, content type, correlation IDs
- **Query params**: filtering, pagination
- **Request body**: payload for writes
- **Response**: status code + body

Changing any of these creates a different endpoint.

---

## 4. Good Example: Clear, RESTful Endpoint

```js
GET /users/123
```

```js
app.get("/users/:id", async (req, res) => {
  const user = await getUserById(req.params.id);
  res.status(200).json(user);
});
```

Why this is good:
- Resource-oriented
- Uses HTTP semantics correctly
- Predictable behavior
- Easy to document and cache

---

## 5. Bad Example: Confusing Endpoint Design

```js
POST /getUserInfo
```

```js
app.post("/getUserInfo", (req, res) => {
  const user = getUser(req.body.id);
  res.send(user);
});
```

Problems:
- Action-based naming
- Misuses POST for reading
- Harder to cache
- Less intuitive for clients

---

## 6. Idempotency and Safety
Endpoints must respect HTTP semantics:

- **Safe**: `GET` (no state changes)
- **Idempotent**: `PUT`, `DELETE`
- **Non-idempotent**: `POST`

Understanding this helps:
- Retry safely
- Design resilient clients
- Work with proxies and caches

---

## 7. Endpoint vs Route vs Controller
These concepts are related but different:

- **Endpoint**: external API contract
- **Route**: framework mapping (Express, Fastify)
- **Controller**: code handling the request

Mixing these concepts often leads to tangled backend code.

---

## 8. Security Considerations
Endpoints are attack surfaces.

Common protections:
- Authentication & authorization
- Input validation
- Rate limiting
- CORS configuration
- Logging and monitoring

Every exposed endpoint should be reviewed for security risk.

---

## 9. Common Backend Mistakes

- Creating too many endpoints
- Overloading one endpoint with many responsibilities
- Inconsistent naming
- Breaking backward compatibility
- Leaking internal errors via responses

---

## 10. Interview Questions You Should Expect

1. What is an endpoint?
2. How does an endpoint differ from a route?
3. What makes an endpoint idempotent?
4. Why should GET requests be safe?
5. How do you design versioned endpoints?
6. What makes an endpoint RESTful?
7. How do you secure endpoints?
8. What are common endpoint anti-patterns?
9. How do endpoints affect caching?
10. How do you document endpoints?

---

## 11. Answer Template for Interviews

> “An endpoint is the combination of a URL and HTTP method that defines a callable entry point in a backend API.
> It represents the external contract clients depend on.
> Good endpoint design follows HTTP semantics, is predictable, secure, and stable over time.”

---

## 12. One-Minute Summary

- Endpoints are where requests enter the backend
- Defined by path + method
- Form the public API contract
- Must follow HTTP semantics
- Strong impact on usability and security
