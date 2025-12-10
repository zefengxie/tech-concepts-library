# Repository Pattern (Backend)

## 1. Formal Definition
The **Repository Pattern** is an architectural pattern that abstracts data access logic behind a collection-like interface.
It separates **business logic** from **persistence concerns** (databases, external storage).

In simple terms:
> A repository hides how data is stored and retrieved, and exposes domain-friendly methods instead.

---

## 2. Why It Matters for Backend Development
The repository pattern matters because it:
- Decouples business logic from database technology
- Improves testability (easy to mock repositories)
- Centralizes data access logic
- Enables database or ORM changes with minimal impact
- Enforces clean architecture boundaries

Without repositories, database logic often leaks into services or controllers.

---

## 3. What a Repository Represents
A repository typically represents:
- A **collection of domain objects**
- Accessed via meaningful methods

Example:
- `UserRepository.findById(id)`
- `OrderRepository.save(order)`

A repository should feel like working with an **in-memory collection**, not raw SQL.

---

## 4. Good Example: Repository Abstraction

```js
// user.repository.js
class UserRepository {
  async findById(id) {
    return db.users.findOne({ id });
  }

  async existsByEmail(email) {
    return db.users.exists({ email });
  }

  async save(user) {
    return db.users.insert(user);
  }
}
```

```js
// user.service.js
const user = await userRepository.findById(userId);
```

Why this is good:
- Database details are hidden
- Service code is clean and expressive
- Easy to replace db implementation
- Repository can be mocked in tests

---

## 5. Bad Example: No Repository Pattern

```js
// user.service.js
const rows = await db.query("SELECT * FROM users WHERE id = ?", id);
```

Problems:
- SQL logic mixed with business logic
- Hard to test without real database
- Tightly coupled to schema and database
- Difficult to refactor or migrate

---

## 6. Repository vs Service vs ORM
Clear separation:

- **Repository**: data access abstraction
- **Service**: business rules and workflows
- **ORM**: implementation detail used by repository

Repositories may use an ORM internally, but services should not depend on ORM APIs directly.

---

## 7. Repository Responsibilities
A repository should:
- Fetch and persist domain entities
- Encapsulate query logic
- Handle data mapping
- Expose intention-revealing methods

A repository should **not**:
- Contain business logic
- Handle HTTP concerns
- Perform complex workflows

---

## 8. Common Anti-Patterns

- Repository as a thin ORM wrapper with hundreds of methods
- Generic “BaseRepository” used everywhere
- Repositories returning raw database rows
- Services leaking ORM queries
- Repositories containing business rules

These patterns defeat the purpose of repositories.

---

## 9. Common Backend Mistakes

- Skipping repositories entirely
- Creating one repository for the entire database
- Tightly coupling repositories to frameworks
- Over-abstracting trivial queries
- Mixing read/write models without intent

---

## 10. Interview Questions You Should Expect

1. What is the repository pattern?
2. Why use repositories instead of direct DB access?
3. Repository vs service — what’s the difference?
4. How does repository improve testability?
5. Should repositories return domain models or DTOs?
6. How do repositories work with ORMs?
7. When is repository pattern overkill?
8. Can repositories call other repositories?
9. How do you handle transactions with repositories?
10. What are repository anti-patterns?

---

## 11. Answer Template for Interviews

> “The repository pattern abstracts persistence logic behind a domain-focused interface.
> It decouples business logic from database implementation details, improves testability, and supports clean architecture.
> Services depend on repositories, while repositories handle how data is stored and retrieved.”

---

## 12. One-Minute Summary

- Repository abstracts data access
- Separates business logic from persistence
- Improves testability and maintainability
- Works alongside service layer
- Key pattern in clean backend architecture
