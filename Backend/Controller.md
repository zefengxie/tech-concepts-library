# Controller (Backend)

## 1. Formal Definition
A **Controller** is a component in a backend application responsible for **handling incoming requests**, coordinating application logic, and returning responses.
It acts as the bridge between HTTP transport (routing) and the core business logic.

In simple terms:
> A controller translates an HTTP request into application actions and returns an HTTP response.

---

## 2. Why It Matters for Backend Development
Controllers matter because they:
- Define the boundary between the web layer and business logic
- Keep request/response handling consistent
- Improve testability and maintainability
- Enable clear separation of concerns

Poorly designed controllers often become “god objects” that are hard to maintain and test.

---

## 3. Controller Responsibilities
A well-designed controller typically:
- Parses request inputs (params, body, headers)
- Performs basic validation
- Calls service-layer methods
- Handles success and error responses
- Maps domain results to HTTP responses

A controller should **not** contain complex business rules.

---

## 4. Good Example: Thin Controller

```js
// users.controller.js
async function getUser(req, res, next) {
  try {
    const user = await userService.getById(req.params.id);
    if (!user) return res.status(404).send("Not found");
    res.status(200).json(user);
  } catch (err) {
    next(err);
  }
}
```

Why this is good:
- Minimal logic
- Delegates work to service layer
- Clear HTTP concerns only
- Easy to test with mocks

---

## 5. Bad Example: Fat Controller

```js
async function getUser(req, res) {
  const rows = await db.query("SELECT * FROM users");
  const user = rows.find(u => u.id === req.params.id);
  if (!user) return res.send("Not found");
  if (user.isBlocked) {
    // complex business logic
  }
  // more logic...
  res.json(user);
}
```

Problems:
- Mixes DB access, business logic, and HTTP handling
- Hard to test
- Violates separation of concerns
- Difficult to refactor

---

## 6. Controller vs Route vs Service
Clear distinctions help architecture:

- **Route**: maps URL + method to controller
- **Controller**: handles HTTP concerns
- **Service**: contains business logic

Controllers should be thin; services should be rich.

---

## 7. Error Handling in Controllers
Best practices:
- Use centralized error-handling middleware
- Throw or pass errors, don’t swallow them
- Map domain errors to HTTP status codes

Avoid try/catch everywhere if your framework supports global handlers.

---

## 8. Controllers in Different Frameworks
Conceptual role is consistent across frameworks:

- Express: plain functions
- NestJS: class-based controllers
- Spring: annotated controllers
- Rails: controller classes

Patterns are transferable across ecosystems.

---

## 9. Common Backend Mistakes

- Putting business logic in controllers
- Accessing databases directly from controllers
- Returning inconsistent response formats
- Handling authentication logic in controllers
- Overusing controllers for orchestration

---

## 10. Interview Questions You Should Expect

1. What is a controller?
2. How is a controller different from a service?
3. Why should controllers be thin?
4. Where should business logic live?
5. How do controllers handle errors?
6. What should a controller not do?
7. Controller vs middleware?
8. How do you test controllers?
9. How do controllers scale in large apps?
10. How do controllers fit into MVC?

---

## 11. Answer Template for Interviews

> “A controller handles incoming requests and coordinates application logic by calling services.
> It should focus on HTTP concerns such as request parsing and response formatting, while delegating business rules to the service layer.
> Thin controllers lead to cleaner, more testable backend code.”

---

## 12. One-Minute Summary

- Controllers handle HTTP requests and responses
- Sit between routes and services
- Should remain thin
- Delegate business logic to services
- Critical for clean backend architecture
