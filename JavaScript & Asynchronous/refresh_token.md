# Refresh Token

## What a Refresh Token Is

A refresh token is a credential used to obtain a new access token without requiring the user to re-authenticate.
It is part of token-based authentication systems, commonly used with OAuth 2.0.

Refresh tokens extend session longevity while keeping access tokens short-lived.

---

## Why Refresh Tokens Exist

Access tokens should be short-lived to reduce security risk.
However, frequent re-authentication hurts user experience.

Refresh tokens allow:
- Short-lived access tokens
- Long-lived user sessions
- Reduced exposure if access tokens leak
- Seamless token renewal

---

## How Refresh Tokens Work

1. User authenticates
2. Authorization server issues:
   - Access token (short-lived)
   - Refresh token (long-lived)
3. Client uses access token for API calls
4. Access token expires
5. Client sends refresh token to authorization server
6. Server issues a new access token (and sometimes a new refresh token)

---

## Token Exchange Example

```http
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token
&refresh_token=REFRESH_TOKEN_VALUE
```

---

## Refresh Token Rotation

Modern systems use rotation:
- Each refresh request returns a new refresh token
- Old refresh token is invalidated
- Detects token reuse attacks

Benefits:
- Limits damage from token leakage
- Improves security auditing

---

## Storage Best Practices

Recommended:
- HttpOnly, Secure cookies
- Encrypted storage on the client
- Server-side storage for rotation tracking

Avoid:
- localStorage
- Exposing refresh tokens to JavaScript
- Sending refresh tokens to resource servers

---

## Refresh Token vs Access Token

| Refresh Token | Access Token |
|--------------|--------------|
| Long-lived | Short-lived |
| Used for renewal | Used for API access |
| Sent to auth server only | Sent to resource servers |
| Stored securely | Stored in memory or cookie |

---

## Security Considerations

- Always use HTTPS
- Restrict refresh token scope
- Bind tokens to client and device
- Implement rotation
- Revoke refresh tokens on logout
- Monitor reuse attempts

---

## Common Mistakes

- Long-lived access tokens instead of refresh tokens
- Storing refresh tokens in localStorage
- Not rotating refresh tokens
- Accepting refresh tokens at API endpoints
- Not revoking tokens on logout

---

## Interview Questions

- Why are refresh tokens needed?
- Difference between access and refresh tokens?
- What is refresh token rotation?
- Where should refresh tokens be stored?
- How do you revoke refresh tokens?

---

## Key Takeaways

- Refresh tokens renew access tokens
- Enable secure long-lived sessions
- Must be stored and handled carefully
- Rotation significantly improves security
