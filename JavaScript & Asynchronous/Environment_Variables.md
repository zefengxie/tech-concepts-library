# Environment Variables (Backend)

## 1. Formal Definition
**Environment Variables** are key–value pairs provided by the operating system to a running process.
In backend applications, they are commonly used to configure behavior **without changing code**.

In simple terms:
> Environment variables allow you to change how your backend behaves across environments (dev, test, prod) without redeploying different code.

---

## 2. Why It Matters for Backend Development
Environment variables are critical because they:
- Separate configuration from code
- Keep secrets out of source control
- Enable the same codebase to run in different environments
- Support deployment automation and CI/CD

Typical backend values stored as environment variables:
- Database connection strings
- API keys and secrets
- JWT signing keys
- Feature flags
- Service URLs
- Runtime settings (port, log level)

Hardcoding these values is a **serious security and operational risk**.

---

## 3. How Environment Variables Work in Node.js
In Node.js, environment variables are accessed through:

```js
process.env
```

Example:

```js
const port = process.env.PORT || 3000;
```

Key characteristics:
- All values are strings
- They are read at runtime
- They are injected by the OS, container, or process manager
- Changing them usually requires restarting the process

---

## 4. Good Example: Using Environment Variables Correctly

```js
const express = require("express");
const app = express();

const PORT = Number(process.env.PORT) || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

Why this is good:
- No hardcoded port
- Works locally and in production
- Compatible with Docker, cloud platforms, and PaaS

---

## 5. Bad Example: Hardcoding Configuration

```js
const DB_PASSWORD = "super-secret-password";
const API_KEY = "abcd-1234-xyz";
```

What goes wrong:
- Secrets leak if code is shared or committed
- Requires code changes per environment
- Violates security best practices
- Makes rotation and incident response difficult

This is one of the most common backend security mistakes.

---

## 6. Security Considerations
Environment variables often contain **secrets**.

Best practices:
- Never commit `.env` files to version control
- Use secret managers in production
- Rotate secrets regularly
- Restrict access to process environment

Common risks:
- Accidental logging of `process.env`
- Exposing env vars through error messages
- Misconfigured CI/CD pipelines leaking secrets

---

## 7. Environment Variables Across Environments

Typical setup:

- **Development**  
  `.env` file or shell exports

- **Testing**  
  CI-provided variables

- **Production**  
  Cloud / container / secret manager injection

Example `.env` file (local only):

```env
PORT=3000
DB_HOST=localhost
DB_USER=dev_user
DB_PASSWORD=dev_password
```

---

## 8. Environment Variables vs Config Files

Environment variables are:
- Simple
- Language-agnostic
- Easy to inject in containers and clouds

Config files are:
- Better for large structured configs
- Easier to validate
- Sometimes necessary for complex setups

In practice, modern backends often **combine both**.

---

## 9. Common Backend Mistakes

- Forgetting that all env vars are strings
- Not validating required environment variables at startup
- Using environment variables for large configuration blobs
- Logging secrets accidentally
- Assuming missing env vars will fail safely

Example mistake:

```js
if (process.env.DEBUG) {
  enableDebug();
}
// DEBUG="false" still evaluates as truthy
```

---

## 10. Interview Questions You Should Expect

1. What are environment variables?
2. Why should secrets not be stored in code?
3. How does Node.js access environment variables?
4. Why are all environment variables strings?
5. How do env vars work in Docker or Kubernetes?
6. How do you manage secrets in production?
7. What is the difference between env vars and config files?
8. How do you validate required environment variables?
9. What are risks of using environment variables incorrectly?
10. What happens if an environment variable changes at runtime?

---

## 11. Answer Template for Interviews

> “Environment variables are a way to configure backend applications without changing code.
> They are commonly used for secrets, service URLs, and environment-specific settings.
> In Node.js, they are accessed via process.env and are always strings.
> Proper use of environment variables improves security, portability, and deployment automation.”

---

## 12. One-Minute Summary

- Environment variables configure behavior without code changes
- Essential for security and deployment
- Used for secrets, ports, and runtime settings
- Always strings in Node.js
- Must be validated and protected carefully
