# Logging (Backend)

## 1. Formal Definition
**Logging** is the practice of recording structured information about an application's runtime behavior.
In backend systems, logs capture events such as requests, errors, warnings, and internal state transitions.

In simple terms:
> Logging answers the question: “What happened in the system, and when?”

---

## 2. Why It Matters for Backend Development
Logging is critical because it:
- Enables debugging in production where live debugging is impossible
- Provides audit trails for security and compliance
- Supports incident response and root-cause analysis
- Feeds monitoring, alerting, and analytics systems

Without proper logging, backend failures become **invisible** and **untraceable**.

---

## 3. Types of Logs
Backends typically produce multiple log categories:

- **Application logs**: business events, domain actions
- **Access logs**: HTTP requests/responses, latency, status codes
- **Error logs**: exceptions, stack traces, failures
- **Security logs**: auth attempts, permission changes
- **Audit logs**: data mutations and user actions

Each type serves a different operational purpose.

---

## 4. Log Levels and Their Meaning
Common log levels (in increasing severity):

- **DEBUG** – Detailed diagnostic information
- **INFO** – Normal application flow
- **WARN** – Unexpected but recoverable situations
- **ERROR** – Failed operations
- **FATAL / CRITICAL** – System cannot continue

Correct log levels allow teams to filter noise and focus during incidents.

---

## 5. Good Example: Structured Logging

```js
const logger = {
  info: (msg, meta) => console.log(JSON.stringify({ level: "info", msg, meta })),
  error: (msg, meta) => console.error(JSON.stringify({ level: "error", msg, meta })),
};

logger.info("User logged in", { userId: 123 });
logger.error("Database connection failed", { host: "db-1" });
```

Why this is good:
- Structured (JSON), not free text
- Machine-readable
- Easy to index and search
- Works well with log aggregation tools

---

## 6. Bad Example: Poor Logging Practices

```js
console.log("error");
console.log("something broke");
```

Problems:
- No context
- No severity
- Impossible to correlate events
- Not searchable or actionable

This kind of logging becomes useless in real systems.

---

## 7. Logging Best Practices

### ✅ Good practices
- Use structured logs (JSON)
- Include context (requestId, userId, service name)
- Log at appropriate levels
- Centralize logs
- Protect sensitive data

### ❌ Bad practices
- Logging secrets or PII
- Excessive logging inside loops
- Using logs as a data store
- Relying only on console logs in production

---

## 8. Logging in Distributed Systems
In modern backends:
- One request may touch multiple services
- Logs must be correlated using:
  - Request IDs
  - Trace IDs
  - Correlation IDs

Without correlation, logs across services are nearly impossible to follow.

---

## 9. Common Backend Logging Mistakes
- Logging too little (no visibility)
- Logging too much (noise, cost, performance hit)
- Inconsistent log formats
- Not rotating logs
- Blocking the event loop with synchronous logging
- Forgetting error stack traces

---

## 10. Interview Questions You Should Expect

1. What is logging and why is it important?
2. What are common log levels and how do you use them?
3. Difference between structured and unstructured logs?
4. How do you log errors in production safely?
5. How do logs differ from metrics and traces?
6. How do you correlate logs across services?
7. Why should logs not contain secrets?
8. Can logging affect performance?
9. How do you handle logs in containerized environments?
10. How would you design logging for a high-traffic API?

---

## 11. Answer Template for Interviews

> “Logging is the process of recording runtime events to understand and operate backend systems.
> In production, logs are the primary tool for debugging and incident analysis.
> Good backend logging is structured, contextual, and secure, and integrates with centralized monitoring systems.”

---

## 12. One-Minute Summary

- Logging provides visibility into backend behavior
- Essential for debugging, security, and operations
- Use structured logs and proper log levels
- Avoid logging secrets or excessive noise
- Logs are critical in distributed systems
