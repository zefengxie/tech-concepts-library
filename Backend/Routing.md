# Routing (Backend)

## 1. Formal Definition
**Routing** is the mechanism that maps incoming requests (HTTP method + path) to the correct handler in a backend application.
A router decides *which code runs* for a given request.

In simple terms:
> Routing is how your backend decides what logic handles a specific URL and HTTP method.

---

## 2. Why It Matters for Backend Development
Routing is fundamental because it:
- Defines how clients access functionality
- Organizes API structure
- Affects maintainability and scalability
- Impacts security and performance

Well-designed routing keeps APIs predictable; poor routing leads to tangled controllers and bugs.

---

## 3. Routing Components
A routing decision is based on:
- **HTTP method** (GET, POST, PUT, PATCH, DELETE)
- **Path pattern** (`/users/:id`)
- **Route parameters**
- **Query parameters** (often parsed before routing)
- **Middleware chaining**

Routers typically match from top to bottom.

---

## 4. Good Example: Clean, Structured Routing

```js
const router = require("express").Router();

router.get("/users", listUsers);
router.get("/users/:id", getUser);
router.post("/users", createUser);
router.put("/users/:id", updateUser);
router.delete("/users/:id", deleteUser);

app.use("/api", router);
```

Why this is good:
- Resource-oriented
- Consistent patterns
- Easy to read and extend
- Clear separation of concerns

---

## 5. Bad Example: Chaotic Routing

```js
app.get("/getUser", handler);
app.post("/users/get", handler);
app.post("/updateUserInfo", handler);
```

Problems:
- Inconsistent naming
- Action-based paths
- Hard to document and maintain
- Difficult for clients to predict

---

## 6. Routing vs Endpoint vs Controller
These concepts are related but distinct:

- **Routing**: request → handler mapping
- **Endpoint**: public API contract (method + path)
- **Controller**: code that handles the request

Good architecture keeps these layers separate.

---

## 7. Route Ordering and Matching
Route order matters:

```js
router.get("/users/:id", handler);
router.get("/users/settings", handler);
```

If ordered incorrectly:
- `/users/settings` may be treated as `{ id: "settings" }`

Specific routes should come **before** generic ones.

---

## 8. Security Considerations
Routing influences security:
- Route exposure = attack surface
- Missing auth middleware = vulnerabilities
- Poor scoping = privilege escalation

Best practices:
- Group protected routes
- Apply middleware at router-level
- Avoid exposing internal routes

---

## 9. Common Backend Routing Mistakes
- Putting business logic in routers
- Overusing route parameters
- Deeply nested paths
- Duplicated route definitions
- Hardcoding routes across services

---

## 10. Interview Questions You Should Expect

1. What is routing?
2. How does routing differ from endpoints?
3. How does route matching work?
4. Why does route order matter?
5. How do you organize routes in large apps?
6. Router vs controller?
7. How do you secure routes?
8. RESTful routing best practices?
9. How do you version routes?
10. What causes routing conflicts?

---

## 11. Answer Template for Interviews

> “Routing is the mechanism that maps incoming requests to the appropriate handler based on the HTTP method and path.
> It defines the structure of an API and controls how functionality is exposed.
> Clean routing improves maintainability, predictability, and security in backend systems.”

---

## 12. One-Minute Summary

- Routing maps requests to handlers
- Core to API structure
- Order matters
- Must align with REST design
- Strong impact on maintainability and security
