# Middleware (Backend)

## 1. Formal Definition
**Middleware** is a software component that sits **between the incoming request and the final request handler**.
It can inspect, modify, block, or augment requests and responses before they reach business logic.

In simple terms:
> Middleware runs *before or after* your endpoint logic to handle cross-cutting concerns.

---

## 2. Why It Matters for Backend Development
Middleware is fundamental in backend systems because it:
- Keeps business logic clean
- Centralizes cross-cutting concerns
- Improves maintainability and consistency
- Enables composable request pipelines

Common backend concerns handled by middleware:
- Authentication & authorization
- Logging
- Input validation
- Rate limiting
- CORS
- Error handling
- Request parsing

Without middleware, these concerns get duplicated across endpoints.

---

## 3. Conceptual Model
Middleware typically follows this flow:

```
Request → Middleware 1 → Middleware 2 → Controller → Response
```

Each middleware can:
- Read or modify the request
- Read or modify the response
- Stop the request entirely
- Pass control to the next middleware

This pattern is also known as a **pipeline** or **chain of responsibility**.

---

## 4. Good Example: Authentication Middleware

```js
function authMiddleware(req, res, next) {
  const token = req.headers.authorization;
  if (!token) {
    return res.status(401).send("Unauthorized");
  }
  req.user = verifyToken(token);
  next();
}

app.use(authMiddleware);
```

Why this is good:
- Authentication logic is centralized
- Controllers stay clean
- Behavior is consistent across endpoints
- Easy to add or remove

---

## 5. Bad Example: Mixing Middleware Logic into Controllers

```js
app.get("/profile", (req, res) => {
  if (!req.headers.authorization) {
    return res.status(401).send("Unauthorized");
  }
  const user = verifyToken(req.headers.authorization);
  res.json(user.profile);
});
```

Problems:
- Authentication duplicated across routes
- Harder to maintain
- Harder to test
- Violates separation of concerns

---

## 6. Middleware Execution Order
Middleware order **matters**:

```js
app.use(logMiddleware);
app.use(authMiddleware);
app.use(rateLimitMiddleware);
```

Execution happens in the order defined.
Incorrect ordering can cause:
- Security bypasses
- Missing logs
- Incorrect error handling

---

## 7. Good vs Bad Middleware Design

### ✅ Good patterns
- One responsibility per middleware
- Small, composable functions
- Clear naming
- Stateless middleware where possible

### ❌ Bad patterns
- Monolithic middleware doing everything
- Heavy CPU work inside middleware
- Modifying request objects unpredictably
- Global middleware without scope control

---

## 8. Middleware vs Filters vs Interceptors
Different frameworks use different terms:

- **Middleware** (Express, Koa): request pipeline
- **Filters** (some frameworks): validation or transformation
- **Interceptors** (NestJS): wrap execution before & after handlers

Conceptually, they all implement **pre/post processing**.

---

## 9. Common Backend Mistakes

- Writing async middleware without error handling
- Forgetting to call `next()`
- Blocking the event loop
- Catching errors but not propagating them
- Applying middleware globally when it should be route-specific

Example mistake:

```js
app.use((req, res, next) => {
  throw new Error("Boom");
});
// No error-handling middleware → crash
```

---

## 10. Interview Questions You Should Expect

1. What is middleware?
2. Why is middleware useful?
3. How does middleware differ from controllers?
4. What happens if you forget to call `next()`?
5. How does middleware order affect behavior?
6. What should not be done in middleware?
7. How do you handle errors in middleware?
8. Middleware vs interceptor?
9. Can middleware modify responses?
10. When should middleware be scoped?

---

## 11. Answer Template for Interviews

> “Middleware is code that runs between the incoming request and the endpoint handler.
> It is used to handle cross-cutting concerns like authentication, logging, and validation.
> Proper middleware design keeps controllers clean, improves reuse, and enforces consistent behavior across the backend.”

---

## 12. One-Minute Summary

- Middleware sits between request and controller
- Handles cross-cutting concerns
- Executed in order
- Enables clean and modular backend design
- Misuse leads to bugs and security issues
