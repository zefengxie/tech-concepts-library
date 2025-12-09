# HTTP Status Codes

## What HTTP Status Codes Are

HTTP status codes are standardized numeric responses sent by a server to indicate the outcome of a client’s request.
They communicate success, failure, redirection, and error semantics without requiring a response body.

Status codes are grouped by their first digit.

---

## 1xx – Informational

Indicates that the request has been received and processing is continuing.

Common examples:
- 100 Continue
- 101 Switching Protocols

These are rarely handled directly in application code.

---

## 2xx – Success

Indicates that the request was successfully received, understood, and accepted.

### 200 OK
Request succeeded.

### 201 Created
A new resource was successfully created.

### 202 Accepted
Request accepted for asynchronous processing.

### 204 No Content
Request succeeded but no response body is returned.

---

## 3xx – Redirection

Indicates that further action is needed to complete the request.

### 301 Moved Permanently
Resource has a new permanent URL.

### 302 Found
Temporary redirect.

### 304 Not Modified
Used for caching; client can use cached version.

---

## 4xx – Client Errors

Indicates that the client made an invalid request.

### 400 Bad Request
Malformed request or invalid data.

### 401 Unauthorized
Authentication required or failed.

### 403 Forbidden
Authenticated but not authorized.

### 404 Not Found
Resource does not exist.

### 409 Conflict
Request conflicts with current server state.

### 429 Too Many Requests
Rate limit exceeded.

---

## 5xx – Server Errors

Indicates a failure on the server side.

### 500 Internal Server Error
Generic server failure.

### 502 Bad Gateway
Invalid response from upstream server.

### 503 Service Unavailable
Server temporarily unavailable.

### 504 Gateway Timeout
Upstream server timed out.

---

## Status Codes and REST APIs

Best practices:
- Use status codes consistently
- Do not encode errors only in response bodies
- Use 4xx for client issues, 5xx for server issues
- Combine status codes with meaningful error payloads

---

## Common Mistakes

- Always returning 200 for errors
- Using 500 for validation issues
- Ignoring 409 conflicts
- Misusing 401 vs 403

---

## Interview Questions

- Difference between 401 and 403?
- When should you use 201 vs 200?
- What does 204 mean?
- When should a server return 5xx?
- How does 304 improve performance?

---

## Key Takeaways

- Status codes describe request outcomes
- First digit defines category
- Proper usage improves API clarity
- Essential for debugging and monitoring
