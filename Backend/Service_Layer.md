# Service Layer (Backend)

## 1. Formal Definition
The **Service Layer** is an architectural layer that contains **business logic and domain rules** of an application.
It sits between controllers (or API handlers) and data access layers (repositories, ORMs).

In simple terms:
> The service layer is where the “real work” of the application happens.

---

## 2. Why It Matters for Backend Development
The service layer matters because it:
- Centralizes business rules
- Keeps controllers thin and focused
- Improves testability
- Enables reuse of business logic across interfaces
- Supports clean and scalable architecture

Without a service layer, logic often leaks into controllers or database code, making systems hard to evolve.

---

## 3. Core Responsibilities of the Service Layer
A well-designed service layer typically:
- Implements business rules and workflows
- Coordinates multiple repositories or external services
- Enforces invariants and validations
- Handles transactions and consistency
- Returns domain-level results (not HTTP responses)

The service layer should be **framework-agnostic**.

---

## 4. Good Example: Clear Service Layer Usage

```js
// user.service.js
async function registerUser(data) {
  if (await userRepository.existsByEmail(data.email)) {
    throw new Error("Email already in use");
  }

  const hashedPassword = await hashPassword(data.password);
  return userRepository.create({
    ...data,
    password: hashedPassword
  });
}
```

```js
// user.controller.js
async function register(req, res, next) {
  try {
    const user = await userService.registerUser(req.body);
    res.status(201).json(user);
  } catch (err) {
    next(err);
  }
}
```

Why this is good:
- Business rules live in the service
- Controller handles HTTP concerns only
- Service can be reused elsewhere
- Easy to test independently

---

## 5. Bad Example: Business Logic Outside Service Layer

```js
async function register(req, res) {
  const existing = await db.query("SELECT * FROM users WHERE email = ?", req.body.email);
  if (existing.length > 0) {
    return res.status(400).send("Email exists");
  }
  // more logic...
}
```

Problems:
- Business logic tied to HTTP
- Hard to test
- Duplicated logic across endpoints
- Poor separation of concerns

---

## 6. Service Layer vs Controller vs Repository

Clear boundaries:

- **Controller**: HTTP, request/response mapping
- **Service Layer**: business logic and workflows
- **Repository**: data access and persistence

The service layer orchestrates; it does not persist directly.

---

## 7. Transactions and Consistency
Services are responsible for:
- Defining transactional boundaries
- Coordinating multiple writes
- Ensuring consistency across operations

This prevents transaction logic from leaking into controllers or repositories.

---

## 8. Service Layer Anti-Patterns

- God services that do everything
- Mixing HTTP concepts into services
- Returning database models directly
- Tight coupling to ORM APIs
- Services calling controllers

Services should represent **use cases**, not technical details.

---

## 9. Common Backend Mistakes

- Skipping the service layer entirely
- Putting validation logic everywhere
- Making services stateful
- Duplicating business logic across services
- Treating services as simple pass-throughs

---

## 10. Interview Questions You Should Expect

1. What is the service layer?
2. Why should controllers be thin?
3. Where should business logic live?
4. Service layer vs repository?
5. Can services call other services?
6. How do you test service layers?
7. How do services handle transactions?
8. When is a service layer unnecessary?
9. How do you design service boundaries?
10. What are service layer anti-patterns?

---

## 11. Answer Template for Interviews

> “The service layer contains the core business logic of an application.
> It sits between controllers and repositories, keeping HTTP concerns separate from domain rules.
> This improves testability, reuse, and long-term maintainability of backend systems.”

---

## 12. One-Minute Summary

- Service layer holds business logic
- Controllers should delegate to services
- Repositories handle data access
- Key to clean architecture
- Essential for scalable backends
