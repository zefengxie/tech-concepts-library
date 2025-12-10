# Error Handling (Backend)

## 1. Formal Definition
**Error Handling** is the practice of detecting, managing, and responding to errors in a backend system in a controlled and predictable way.
It ensures failures do not crash the system or leak sensitive information.

In simple terms:
> Error handling decides how your backend behaves when something goes wrong.

---

## 2. Why It Matters for Backend Development
Error handling is critical because it:
- Prevents system crashes
- Improves reliability and user experience
- Enables debugging and monitoring
- Protects sensitive internal details
- Supports graceful degradation

Poor error handling turns small bugs into system-wide failures.

---

## 3. Types of Backend Errors
Common categories:
- **Client errors (4xx)**: invalid input, unauthorized access
- **Server errors (5xx)**: bugs, infrastructure failures
- **Operational errors**: database down, timeout
- **Programmer errors**: null pointer, logic mistakes

Good systems handle these categories differently.

---

## 4. Good Example: Centralized Error Handling

```js
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).json({ message: "Internal Server Error" });
});
```

Why this is good:
- Centralized handling
- Consistent responses
- Internal details hidden
- Easy to integrate logging and monitoring

---

## 5. Bad Example: Leaking Internal Errors

```js
catch (err) {
  res.send(err.stack);
}
```

Problems:
- Leaks internal implementation details
- Security risk
- Confuses clients
- Makes API unstable

Clients should never see stack traces.

---

## 6. Error Handling vs Validation
Important distinction:
- **Validation errors**: expected, user-caused
- **System errors**: unexpected, backend-caused

Validation errors should return clear 4xx responses.
System errors should be logged and return generic messages.

---

## 7. Error Propagation Strategy
Best practices:
- Throw errors early
- Catch errors at boundaries (controllers/middleware)
- Use custom error types
- Map domain errors to HTTP status codes

Avoid catching errors too low without context.

---

## 8. Retry and Resilience
Some errors are transient:
- Network timeouts
- Temporary service outages

Strategies:
- Retries with backoff
- Circuit breakers
- Fallback responses

Not all errors should be retried blindly.

---

## 9. Common Backend Mistakes
- Catching errors and ignoring them
- Using generic errors everywhere
- Returning inconsistent error formats
- No correlation IDs
- No error monitoring

Silent failures are the most dangerous.

---

## 10. Interview Questions You Should Expect
1. What is error handling?
2. Client error vs server error?
3. How do you handle unexpected errors?
4. Why centralize error handling?
5. What should error responses contain?
6. Should you expose stack traces?
7. How do retries work?
8. What is graceful degradation?
9. Error handling vs validation?
10. Common error handling anti-patterns?

---

## 11. Answer Template for Interviews

> “Error handling is the process of managing failures so systems remain stable and secure.
> It involves categorizing errors, handling them centrally, and returning consistent responses.
> Proper error handling improves reliability, observability, and user trust.”

---

## 12. One-Minute Summary

- Error handling manages failures safely
- Centralized handling is best practice
- Do not leak internal details
- Different errors need different responses
- Essential for reliable backend systems
