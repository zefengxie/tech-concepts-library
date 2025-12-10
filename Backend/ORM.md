# ORM (Object–Relational Mapping)

## 1. Formal Definition
**ORM (Object–Relational Mapping)** is a technique that maps database tables (rows/columns) to application objects (classes/properties).
It allows developers to interact with a relational database using objects instead of raw SQL.

In simple terms:
> An ORM lets you work with database records as if they were normal objects in your code.

---

## 2. Why It Matters for Backend Development
ORMs are widely used because they:
- Reduce boilerplate SQL
- Improve developer productivity
- Provide database-agnostic abstractions
- Enforce consistent data access patterns
- Integrate with repositories and services

Common ORMs:
- Sequelize, TypeORM, Prisma (Node.js)
- Hibernate (Java)
- Entity Framework (C#)
- ActiveRecord (Rails)

---

## 3. Core ORM Concepts
Key ORM concepts include:
- **Entity / Model**: maps to a database table
- **Field / Column**: maps to a table column
- **Relationship**: one-to-one, one-to-many, many-to-many
- **Identity Map**: tracks loaded objects
- **Unit of Work**: batches changes into transactions

Understanding these concepts helps avoid performance pitfalls.

---

## 4. Good Example: ORM Used with Repository

```js
// user.entity.js
class User {
  id;
  email;
  password;
}
```

```js
// user.repository.js
async function findByEmail(email) {
  return orm.user.findOne({ where: { email } });
}
```

Why this is good:
- ORM details isolated in repository
- Domain code stays clean
- Database can change with minimal impact

---

## 5. Bad Example: ORM Leakage Everywhere

```js
// controller.js
const user = await orm.user.findOne({
  where: { email: req.body.email },
  include: ["posts", "comments"]
});
```

Problems:
- Controller tightly coupled to ORM
- Hard to test
- Business logic polluted by persistence concerns
- Refactoring becomes expensive

---

## 6. ORM vs Raw SQL

| Aspect | ORM | Raw SQL |
|-----|-----|---------|
| Productivity | High | Medium |
| Control | Medium | High |
| Performance tuning | Harder | Easier |
| Portability | High | Low |
| Learning curve | Lower | Higher |

Most real systems use **both**, selectively.

---

## 7. Performance Pitfalls
Common ORM performance issues:
- N+1 query problems
- Lazy loading surprises
- Over-fetching data
- Complex joins hidden by abstractions

Mitigations:
- Explicit eager loading
- Query logging and profiling
- Using raw SQL for hot paths

---

## 8. ORM Anti-Patterns

- Treating ORM as a magic layer
- Letting ORM models become domain models
- Massive entities with too many relations
- Blindly trusting lazy loading
- Skipping database knowledge entirely

ORMs do not eliminate the need to understand SQL.

---

## 9. Common Backend Mistakes

- Using ORM directly in controllers
- Not understanding generated SQL
- Ignoring transaction boundaries
- Overusing ORM for analytics queries
- Tight coupling to a single ORM

---

## 10. Interview Questions You Should Expect

1. What is an ORM?
2. ORM vs raw SQL — trade-offs?
3. What is the N+1 query problem?
4. What is lazy vs eager loading?
5. When should you avoid an ORM?
6. How do ORMs handle transactions?
7. How does ORM fit with repository pattern?
8. How do you debug ORM performance?
9. Are ORMs database-agnostic?
10. What are common ORM anti-patterns?

---

## 11. Answer Template for Interviews

> “An ORM maps database tables to objects, allowing developers to work with persistence using application-level abstractions.
> ORMs improve productivity and maintainability, but they can hide performance issues.
> In clean architectures, ORMs are used inside repositories, not exposed to business logic.”

---

## 12. One-Minute Summary

- ORM maps tables to objects
- Reduces SQL boilerplate
- Improves productivity
- Can hide performance issues
- Best used behind repositories
