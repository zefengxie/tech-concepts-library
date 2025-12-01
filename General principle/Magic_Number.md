# Magic Number 

## 1. Formal Definition
A **magic number** is:
> A hard-coded literal value that appears in code without explanation, meaning, or context.

Examples:
- `if (x > 7):`  
- `timeout = 3000`  
- `"USER_42"`  
- `for i in range(128):`

Magic numbers hide **meaning**, reduce readability, and make maintenance dangerous.

---

## 2. Practical Meaning in Software Engineering
Magic numbers are problematic because:
- The meaning is unclear  
- Changing them requires searching the entire codebase  
- Different parts of the system may rely on the same number for different reasons  
- They often cause bugs when business rules change  
- They violate DRY, KISS, and Clean Code principles  

Example:

```python
if score >= 90:
    return "A"
```

Why **90**? Passing threshold? Hard-coded business rule?

---

## 3. Why Magic Numbers Are Harmful

### ✔ Poor Readability
Future developers have no idea **why** the number exists.

### ✔ Hard to Update
If the rule changes (e.g., threshold becomes 85), you must hunt down all occurrences.

### ✔ Encourages Inconsistent Business Logic
Multiple modules might use different versions of the same number.

### ✔ Easy to Break When Requirements Change
If “7 days" becomes “14 days,” missing one instance = subtle bugs.

### ✔ Hard to Test
Tests depend on unclear logic.

---

## 4. Good Example (Correct Replacement)

```python
MAX_LOGIN_ATTEMPTS = 5

def can_attempt_login(count):
    return count < MAX_LOGIN_ATTEMPTS
```

Why it’s good:
- Name gives meaning  
- Centralized  
- Easy to update  
- Improves readability  

---

## 5. Bad Example (Magic Number)

```python
def can_attempt_login(count):
    return count < 5
```

What is “5”?  
Allowed attempts? API limit? Business rule? Security requirement?

No way to know.

---

## 6. Magic Numbers Are Not Always Numbers
They can be:
- Strings
- Booleans
- Magic symbols
- Magic array indices
- Magic dates
- Magic timing values  

Example:

```python
if user.role == "ADM42":
```

or:

```python
if event.type == "X01":
```

Better:

```python
ADMIN_ROLE = "ADM42"
EVENT_PAYMENT_RECEIVED = "X01"
```

---

## 7. Exceptions (When Magic Numbers Are Acceptable)

Some values are universally understood:

- `0`, `1`, `-1`
- Boolean flags (True/False)
- Array index starting at 0 (if inherent)
- Simple increments

Example:

```python
for i in range(10):
```

This is fine when the loop bound is intrinsically tied to the logic.

---

## 8. Magic Numbers vs Constants vs Enums

| Concept | Purpose |
|--------|---------|
| Magic Number | A literal with unclear meaning |
| Constant | Named value with explicit meaning |
| Enum | A set of related named values |

Use:
- constants for single values  
- enums for value families  
- configuration for adjustable rules  

---

## 9. Real-World Examples

### Example: Expiry durations
Bad:
```python
expiry = time_now + 86400
```

Good:
```python
SECONDS_PER_DAY = 86400
expiry = time_now + SECONDS_PER_DAY
```

### Example: API status
Bad:
```python
if response.status == 429:
```

Good:
```python
HTTP_TOO_MANY_REQUESTS = 429
```

---

## 10. Relationships to Other Principles

| Principle | Relationship |
|----------|--------------|
| DRY | Magic numbers often repeat across code |
| KISS | Named constants simplify understanding |
| Clean Code | Encourages intention-revealing names |
| High Cohesion | Constants remain in the correct domain |
| Low Coupling | Avoids leaking implementation details |

---

## 11. Interview Questions

1. What is a magic number?  
2. Why are magic numbers bad?  
3. Give an example of replacing a magic number.  
4. When is a magic number acceptable?  
5. How do magic numbers violate DRY?  
6. How do enums help replace magic numbers?  
7. Can strings be magic numbers? Give examples.  

---

## 12. Winning Interview Answer Template

> “A magic number is a hard-coded literal value with unclear meaning.  
It reduces readability and makes maintenance risky.  
I avoid magic numbers by creating named constants or enums that reveal intent.  
Constants centralize business rules and make future changes safer and easier.”

---

## 13. One-Minute Summary
- Magic numbers hide intention  
- Replace them with named constants or enums  
- Improves readability, maintainability, and consistency  
- Exceptions: universally understood values like 0 and 1  
