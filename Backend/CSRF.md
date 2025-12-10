# CSRF (Cross-Site Request Forgery)

## 1. Formal Definition
**CSRF (Cross-Site Request Forgery)** is an attack where a malicious website causes a user’s browser to perform an unwanted action on a trusted site where the user is already authenticated.

In simple terms:
> CSRF tricks a logged-in user’s browser into sending a request they did not intend to make.

---

## 2. Why It Matters for Backend Development
CSRF is critical to understand because it:
- Exploits browser behavior, not server bugs
- Affects authenticated users
- Can lead to data modification or account takeover
- Commonly targets session-based authentication systems

Any backend that relies on cookies for authentication is potentially vulnerable.

---

## 3. How a CSRF Attack Works
Typical CSRF flow:
1. User logs in to a trusted site (session cookie stored)
2. User visits a malicious site
3. Malicious site triggers a request to the trusted site
4. Browser automatically includes session cookies
5. Server treats the request as legitimate

The server cannot distinguish intent without protection.

---

## 4. Good Example: CSRF Protection with Token

```js
app.get("/form", (req, res) => {
  res.render("form", { csrfToken: req.csrfToken() });
});

app.post("/update", csrfProtection, (req, res) => {
  // request validated
});
```

Why this is good:
- Token tied to user session
- Token validated on every state-changing request
- Prevents forged requests

---

## 5. Bad Example: No CSRF Protection

```js
app.post("/transfer-money", (req, res) => {
  transfer(req.body.amount);
});
```

Problems:
- Any website can trigger this request
- Session cookies sent automatically
- User intent is not verified

---

## 6. CSRF vs XSS
Key differences:

| Aspect | CSRF | XSS |
|-----|-----|-----|
| Attack source | External site | Injected script |
| Exploits | Browser trust | Script execution |
| Mitigation | Tokens, SameSite | Input sanitization |

Both are serious but distinct vulnerabilities.

---

## 7. CSRF Protection Techniques
Common defenses:
- CSRF tokens
- SameSite cookies
- Double-submit cookies
- Checking Origin / Referer headers

CSRF tokens remain the most robust defense.

---

## 8. When CSRF Is Not a Concern
CSRF risk is reduced when:
- APIs use Authorization headers instead of cookies
- Tokens are stored outside cookies
- Requests require custom headers

However, assumptions must be validated carefully.

---

## 9. Common Backend Mistakes

- Disabling CSRF protection blindly
- Assuming JWT alone solves CSRF
- Not protecting PUT / DELETE requests
- Using GET requests for state changes
- Ignoring SameSite cookie settings

CSRF vulnerabilities are often subtle.

---

## 10. Interview Questions You Should Expect

1. What is CSRF?
2. How does a CSRF attack work?
3. Why are cookies vulnerable?
4. How do CSRF tokens work?
5. Is JWT immune to CSRF?
6. What is SameSite?
7. CSRF vs XSS?
8. When can CSRF protection be skipped?
9. Which HTTP methods need CSRF protection?
10. How do you test CSRF defenses?

---

## 11. Answer Template for Interviews

> “CSRF is an attack that forces a user’s browser to perform unintended actions on a site where the user is already authenticated.
> It exploits the automatic sending of cookies.
> CSRF protection relies on verifying user intent, commonly using CSRF tokens or SameSite cookies.”

---

## 12. One-Minute Summary

- CSRF exploits browser trust
- Targets authenticated users
- Common in session-based auth
- Tokens are the primary defense
- Must be handled explicitly by backend
