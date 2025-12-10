# RPC (Remote Procedure Call)

## 1. Formal Definition
**RPC (Remote Procedure Call)** is an API paradigm where a client invokes a function on a remote server **as if it were a local function call**.
The network layer, serialization, and transport are abstracted away from the caller.

In simple terms:
> RPC lets you call a function on another service over the network.

---

## 2. Why It Matters for Backend Development
RPC is widely used in backend systems because it:
- Feels natural to developers (function calls)
- Enables strong typing and contracts
- Often provides higher performance than REST
- Works well for internal service-to-service communication

Common use cases:
- Microservices communication
- Internal backend APIs
- High-performance systems
- Strongly typed APIs (gRPC, Thrift)

---

## 3. Core RPC Concepts
Key elements of RPC systems:
- **Service definition** (interface / contract)
- **Methods** (functions exposed remotely)
- **Serialization** (JSON, Protobuf, Thrift)
- **Transport** (HTTP/2, TCP)
- **Code generation** (client/server stubs)

RPC shifts API design from *resources* to *operations*.

---

## 4. Good Example: RPC-style API

### Service definition (conceptual)
```proto
service UserService {
  rpc GetUser(GetUserRequest) returns (User);
}
```

### Client usage
```js
const user = await userService.getUser({ id: 123 });
```

**Why this is good:**
- Strongly typed contract
- Clear intent (GetUser)
- No ambiguity about behavior
- Easy to refactor with tooling

---

## 5. Bad Example: Overusing RPC Publicly

```js
POST /UserService/GetUser
```

Problems:
- Tight coupling to method names
- Harder to evolve publicly
- Less cacheable
- Poor fit for browser-based APIs

RPC is usually better for **internal** APIs than public ones.

---

## 6. RPC vs REST (Conceptual Difference)

| Aspect | RPC | REST |
|-----|-----|-----|
| Focus | Operations | Resources |
| Style | Function calls | HTTP semantics |
| Typing | Strong | Weak |
| Caching | Hard | Easy |
| Evolution | Tool-assisted | Manual |

Neither is “better”; the choice depends on context.

---

## 7. Good vs Bad Usage Patterns

### ✅ Good patterns
- Internal microservice communication
- Performance-critical systems
- Strict contracts with versioning
- Using Protobuf / IDL-based definitions

### ❌ Bad patterns
- Public APIs without version discipline
- Browser-facing APIs
- Over-chatty RPC calls
- Treating RPC like REST over POST

---

## 8. Error Handling in RPC
RPC errors are usually:
- Structured
- Typed
- Part of the protocol

Example:
- gRPC status codes (`INVALID_ARGUMENT`, `NOT_FOUND`)
- Application-level error metadata

This is different from HTTP status-based REST errors.

---

## 9. Common Backend Mistakes with RPC

- Designing chatty interfaces (many small calls)
- Ignoring backward compatibility
- Leaking internal method names externally
- Not handling network failures
- Assuming RPC calls are “fast local calls”

RPC calls are still **network calls** and can fail.

---

## 10. Interview Questions You Should Expect

1. What is RPC?
2. How does RPC differ from REST?
3. When would you choose RPC over REST?
4. What are the advantages of gRPC?
5. How does serialization work in RPC?
6. What are RPC stubs?
7. What are common RPC pitfalls?
8. Is RPC suitable for public APIs?
9. How do you version RPC services?
10. Why are RPC calls not the same as local calls?

---

## 11. Answer Template for Interviews

> “RPC is an API style where clients invoke remote methods as if they were local functions.
> It works well for internal service-to-service communication because of strong typing and performance.
> However, RPC increases coupling and is usually less suitable for public APIs than REST.”

---

## 12. One-Minute Summary

- RPC exposes functions, not resources
- Strong typing and contracts
- Great for internal services
- Higher performance, more coupling
- Network failures still apply
