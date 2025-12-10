# Authorization (Backend)

## 1. Formal Definition
**Authorization** is the process of determining **what an authenticated user or system is allowed to do**.
It answers the question: *Do you have permission to perform this action?*

Authorization always happens **after authentication**.

---

## 2. Why It Matters for Backend Development
Authorization is critical because it:
- Protects sensitive resources
- Enforces business rules and access policies
- Prevents privilege escalation
- Enables multi-role and multi-tenant systems

Without authorization, authenticated users could access or modify data they should not.

---

## 3. Authorization vs Authentication
Clear distinction:

- **Authentication**: Who you are
- **Authorization**: What you can do

A system can authenticate a user correctly but still fail if authorization is weak.

---

## 4. Common Authorization Models

### Role-Based Access Control (RBAC)
- Permissions assigned to roles
- Users assigned roles
- Simple and widely used

Example:
- Admin, Editor, Viewer

### Permission-Based Access Control
- Fine-grained permissions
- More flexible than RBAC
- More complex to manage

### Attribute-Based Access Control (ABAC)
- Decisions based on attributes
- Context-aware (user, resource, environment)
- Highly flexible but complex

---

## 5. Good Example: Role-Based Authorization

```js
function authorize(role) {
  return (req, res, next) => {
    if (!req.user.roles.includes(role)) {
      return res.status(403).send("Forbidden");
    }
    next();
  };
}

app.post("/admin", authorize("admin"), handler);
```

Why this is good:
- Clear responsibility
- Reusable logic
- Easy to audit
- Centralized control

---

## 6. Bad Example: Authorization Logic in Controllers

```js
if (req.user.role !== "admin") {
  return res.send("Not allowed");
}
```

Problems:
- Scattered logic
- Hard to maintain
- Easy to forget checks
- Inconsistent behavior

Authorization should be centralized.

---

## 7. Resource-Based Authorization
Authorization often depends on **ownership**:

- User can access their own data
- Admin can access all data

Example checks:
- `post.ownerId === req.user.id`
- `order.customerId === req.user.id`

This logic often lives in services, not controllers.

---

## 8. Common Authorization Mistakes

- Trusting client-side role checks
- Using only frontend enforcement
- Missing authorization on nested resources
- Overusing admin roles
- Hardcoding permissions

Authorization bugs are high-impact security flaws.

---

## 9. Authorization and Middleware
Authorization is often implemented as:
- Middleware
- Guards (NestJS)
- Policy layers

Benefits:
- Consistency
- Reusability
- Easier testing
- Clear boundaries

---

## 10. Interview Questions You Should Expect

1. What is authorization?
2. Authorization vs authentication?
3. RBAC vs ABAC?
4. How do you implement authorization in APIs?
5. What is resource-based authorization?
6. Where should authorization logic live?
7. How do you avoid privilege escalation?
8. Role vs permission?
9. How do you test authorization?
10. Common authorization vulnerabilities?

---

## 11. Answer Template for Interviews

> “Authorization determines whether an authenticated user is allowed to perform an action.
> It enforces access control rules based on roles, permissions, or ownership.
> Strong authorization must be implemented on the backend and applied consistently.”

---

## 12. One-Minute Summary

- Authorization controls access
- Happens after authentication
- Enforces business rules
- Must be centralized and consistent
- Critical for backend security
