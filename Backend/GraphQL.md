# GraphQL (Backend)

## 1. Formal Definition
**GraphQL** is a query language and runtime for APIs that allows clients to request **exactly the data they need**, no more and no less.
Unlike REST, GraphQL exposes a **single endpoint** with a strongly typed schema.

In simple terms:
> GraphQL lets the client decide what data shape it wants, and the server fulfills it according to a schema.

---

## 2. Why It Matters for Backend Development
GraphQL is important in backend systems because it:
- Eliminates over-fetching and under-fetching
- Enables flexible client-driven queries
- Works well with complex relational data
- Reduces the number of API calls needed by clients

Common use cases:
- Mobile applications
- Frontend-heavy systems
- BFF (Backend-for-Frontend) layers
- Systems with rapidly changing UI requirements

---

## 3. Core GraphQL Concepts
Key GraphQL building blocks:
- **Schema**: defines types and relationships
- **Query**: fetches data (read)
- **Mutation**: modifies data (write)
- **Resolver**: functions that return data for fields
- **Type system**: strong typing and validation

Everything in GraphQL is defined and validated at the schema level.

---

## 4. Good Example: GraphQL Query

### Schema (simplified)
```graphql
type User {
  id: ID!
  name: String!
  email: String!
}

type Query {
  user(id: ID!): User
}
```

### Client query
```graphql
query {
  user(id: 123) {
    name
    email
  }
}
```

Why this is good:
- Client fetches only needed fields
- No extra data sent
- Clear contract via schema

---

## 5. Bad Example: Misusing GraphQL

```graphql
query {
  user(id: 123) {
    id
    name
    email
    posts {
      comments {
        author {
          profile {
            address {
              street
            }
          }
        }
      }
    }
  }
}
```

Problems:
- Extremely deep query
- Performance risks
- Potential denial-of-service
- Hard to reason about execution cost

Without safeguards, GraphQL can be abused.

---

## 6. GraphQL vs REST

| Aspect | GraphQL | REST |
|------|--------|------|
| Endpoints | Single | Multiple |
| Data shape | Client-defined | Server-defined |
| Typing | Strong | Weak |
| Caching | Hard | Easy |
| Flexibility | High | Moderate |

GraphQL trades simplicity in caching for query flexibility.

---

## 7. Performance Considerations
GraphQL introduces backend challenges:
- N+1 query problems
- Expensive nested resolvers
- Need for query complexity limits
- Field-level authorization

Techniques to mitigate:
- DataLoader batching
- Query depth/complexity limits
- Persisted queries
- Caching at resolver level

---

## 8. Security Concerns
Common GraphQL security issues:
- Unbounded queries
- Introspection leaks
- Authorization at field level
- Excessive data exposure

Best practices:
- Disable introspection in production
- Apply rate limiting
- Enforce depth and complexity limits
- Validate user permissions per field

---

## 9. Common Backend Mistakes

- Treating GraphQL like REST
- Putting business logic in resolvers directly
- Not batching database calls
- Ignoring authorization on nested fields
- Exposing internal schemas publicly

GraphQL requires **discipline** in backend design.

---

## 10. Interview Questions You Should Expect

1. What is GraphQL?
2. How does GraphQL differ from REST?
3. What are resolvers?
4. What is the N+1 problem?
5. How do you secure a GraphQL API?
6. Why is caching harder in GraphQL?
7. When should you NOT use GraphQL?
8. What are mutations?
9. How does schema evolution work?
10. What are persisted queries?

---

## 11. Answer Template for Interviews

> “GraphQL is an API query language that allows clients to request exactly the data they need from a single endpoint.
> It is strongly typed and works well for complex data relationships.
> However, it introduces backend complexity around performance, security, and caching, so it must be carefully designed.”

---

## 12. One-Minute Summary

- GraphQL is client-driven data fetching
- Strongly typed schema
- Single endpoint
- Solves over-fetching
- Requires careful performance and security controls
