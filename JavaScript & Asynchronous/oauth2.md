# OAuth 2.0

## What OAuth 2.0 Is

OAuth 2.0 is an authorization framework that allows a third-party application
to obtain limited access to a protected resource on behalf of a user,
without exposing the user’s credentials.

OAuth 2.0 is about **authorization**, not authentication.

---

## The Problem OAuth Solves

Without OAuth:
- Users would share usernames and passwords with third-party apps
- Credentials could be stored, leaked, or abused

OAuth allows:
- Delegated access
- Scoped permissions
- Token-based authorization
- Revocable access

---

## Core Roles in OAuth 2.0

### Resource Owner
The user who owns the data.

### Client
The application requesting access.

### Authorization Server
Issues tokens after authenticating the user and obtaining consent.

### Resource Server
Hosts the protected resources and validates access tokens.

---

## OAuth Tokens

### Access Token
- Short-lived
- Sent to the resource server
- Represents granted permissions (scope)

### Refresh Token
- Long-lived
- Used to obtain new access tokens
- Never sent to resource servers

---

## Authorization Grant Types

### Authorization Code Grant
Most common and secure flow.

Steps:
1. Client redirects user to authorization server
2. User authenticates and grants consent
3. Authorization server returns authorization code
4. Client exchanges code for access token
5. Client accesses resource server

Used by:
- Web applications
- Mobile apps

---

### Authorization Code + PKCE
Adds proof key to prevent authorization code interception.

Standard for:
- Public clients
- Mobile and SPA applications

---

### Client Credentials Grant
Used for machine-to-machine communication.

No user involved.

---

### Implicit Grant (Deprecated)
Previously used for SPAs.
Now discouraged due to security risks.

---

## Scopes

Scopes define what actions the client can perform.

Examples:
- read:user
- write:orders
- admin

The access token is limited by its scopes.

---

## OAuth vs Authentication

OAuth:
- Grants access to APIs
- Answers: “What can this app do?”

Authentication:
- Verifies identity
- Answers: “Who are you?”

OAuth can be layered with OpenID Connect for authentication.

---

## OAuth in Practice

Typical Authorization header:

```http
Authorization: Bearer <access_token>
```

---

## Security Considerations

- Always use HTTPS
- Use PKCE for public clients
- Keep access tokens short-lived
- Protect refresh tokens
- Validate issuer, audience, and scopes

---

## Common Mistakes

- Treating OAuth as authentication
- Not validating tokens
- Overly broad scopes
- Storing refresh tokens insecurely
- Using deprecated flows

---

## Interview Questions

- What problem does OAuth solve?
- OAuth vs JWT vs Sessions?
- What is PKCE and why is it important?
- Difference between access and refresh tokens?
- How does OAuth differ from OpenID Connect?

---

## Key Takeaways

- OAuth 2.0 is an authorization framework
- Tokens replace credential sharing
- Scopes limit access
- PKCE improves security
- OAuth scales well across systems
