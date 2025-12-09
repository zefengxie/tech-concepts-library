# HTTP Methods

## What HTTP Methods Are

HTTP methods define the type of action a client wants to perform on a resource.
They are a core part of RESTful API design and describe intent, semantics, and safety guarantees.

---

## GET

Purpose:
- Retrieve data from the server

Characteristics:
- Safe
- Idempotent
- No request body (by convention)

```http
GET /users/123 HTTP/1.1
```

Used for:
- Fetching resources
- Querying data

---

## POST

Purpose:
- Create a new resource or trigger processing

Characteristics:
- Not idempotent
- Can have side effects

```http
POST /users HTTP/1.1
```

Used for:
- Creating records
- Submitting forms
- Authentication flows

---

## PUT

Purpose:
- Replace a resource entirely

Characteristics:
- Idempotent

```http
PUT /users/123 HTTP/1.1
```

Used when:
- Client provides full resource representation

---

## PATCH

Purpose:
- Partially update a resource

Characteristics:
- Not strictly idempotent (but often treated as such)

```http
PATCH /users/123 HTTP/1.1
```

Used when:
- Updating only specific fields

---

## DELETE

Purpose:
- Remove a resource

Characteristics:
- Idempotent

```http
DELETE /users/123 HTTP/1.1
```

---

## HEAD

Purpose:
- Same as GET, but without response body

Used for:
- Checking existence
- Metadata inspection
- Cache validation

---

## OPTIONS

Purpose:
- Discover supported HTTP methods and CORS behavior

```http
OPTIONS /users HTTP/1.1
```

Often used in:
- CORS preflight requests

---

## Idempotency and Safety

| Method | Safe | Idempotent |
|------|------|------------|
| GET | Yes | Yes |
| POST | No | No |
| PUT | No | Yes |
| PATCH | No | No |
| DELETE | No | Yes |
| HEAD | Yes | Yes |
| OPTIONS | Yes | Yes |

---

## Common Mistakes

- Using POST for everything
- Updating resources with GET
- Confusing PUT and PATCH
- Ignoring idempotency in API design

---

## REST Design Guidelines

- Use nouns for URLs, verbs for methods
- GET should not change state
- Use POST for non-idempotent actions
- Prefer PATCH for partial updates

---

## Interview Questions

- Difference between PUT and PATCH?
- What does idempotent mean?
- Why is GET considered safe?
- Can POST be idempotent?
- When is OPTIONS used?

---

## Key Takeaways

- HTTP methods express intent
- Idempotency affects retries
- Correct usage improves API clarity and reliability
- REST relies heavily on proper method semantics
