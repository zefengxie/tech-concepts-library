# HTTP Headers

## What HTTP Headers Are

HTTP headers are key–value pairs sent with HTTP requests and responses.
They provide metadata about the request, the response, the client, the server, and how the connection should be handled.

Headers do not contain the main payload but describe how the payload should be interpreted, cached, secured, or processed.

---

## Request Headers

Request headers are sent by the client to describe the request context.

### Common Request Headers

#### Host
Specifies the target host and port.

```http
Host: api.example.com
```

Required in HTTP/1.1.

---

#### User-Agent
Identifies the client making the request.

```http
User-Agent: Mozilla/5.0
```

Used for analytics and conditional behavior.

---

#### Accept
Indicates which response formats the client can handle.

```http
Accept: application/json
```

---

#### Authorization
Carries credentials for authentication.

```http
Authorization: Bearer <token>
```

---

#### Content-Type
Describes the format of the request body.

```http
Content-Type: application/json
```

---

## Response Headers

Response headers are sent by the server to describe the response.

### Common Response Headers

#### Content-Type
Indicates the media type of the response.

```http
Content-Type: application/json
```

---

#### Content-Length
Specifies the size of the response body in bytes.

---

#### Cache-Control
Controls caching behavior.

```http
Cache-Control: no-store
```

---

#### Set-Cookie
Sets cookies on the client.

```http
Set-Cookie: sessionId=abc; HttpOnly; Secure
```

---

## Caching Headers

Used to control browser and proxy caching.

- Cache-Control
- Expires
- ETag
- Last-Modified
- If-None-Match
- If-Modified-Since

Example:

```http
Cache-Control: max-age=3600
ETag: "abc123"
```

---

## CORS Headers

Used to control cross-origin requests.

- Access-Control-Allow-Origin
- Access-Control-Allow-Methods
- Access-Control-Allow-Headers
- Access-Control-Allow-Credentials

Example:

```http
Access-Control-Allow-Origin: https://example.com
```

---

## Security-Related Headers

- Strict-Transport-Security
- Content-Security-Policy
- X-Frame-Options
- X-Content-Type-Options
- Referrer-Policy

These headers mitigate common web vulnerabilities.

---

## Custom Headers

Applications can define custom headers.

Convention:
- Prefix with `X-` (legacy) or use clear names

```http
X-Request-Id: 12345
```

---

## Common Mistakes

- Incorrect Content-Type
- Missing CORS headers
- Overly permissive Access-Control-Allow-Origin
- Disabling cache unintentionally
- Storing sensitive data in headers

---

## Interview Questions

- What is the difference between Content-Type and Accept?
- How do caching headers work together?
- What headers are involved in CORS?
- How does Authorization differ from Cookie-based auth?
- What headers improve security?

---

## Key Takeaways

- Headers carry metadata, not payloads
- Request and response headers serve different roles
- Correct headers are critical for security and performance
- Misconfigured headers cause many production issues
