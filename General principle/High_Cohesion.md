# High Cohesion 

## 1. Formal Definition
High Cohesion means:
> A module/class/function should contain elements that are **closely related** and contribute to a **single, well‑defined purpose**.

Cohesion describes *how strongly related* a module’s internal responsibilities are.

High Cohesion =  
- focused purpose  
- consistent responsibilities  
- clear boundaries  
- minimal unrelated logic  

Low Cohesion =  
- mixed, scattered responsibilities  
- “kitchen sink” modules  
- hard to maintain or extend  

---

## 2. Practical Meaning in Software Engineering
In practice, high cohesion means:

- A class does one conceptual job  
- A module has one domain meaning  
- A function performs one operation  
- Files have clear themes  
- APIs expose focused capabilities  

High cohesion leads to:
- better readability  
- easier testing  
- fewer bugs  
- easier onboarding  
- more stable architecture  

---

## 3. Why High Cohesion Matters

### ✔ Easier Maintenance  
Focused modules are easy to reason about.

### ✔ Better Reusability  
Small cohesive modules apply naturally to many contexts.

### ✔ Reduced Side Effects  
Less risk of breaking unrelated behavior.

### ✔ Enables Clean Architecture  
Cohesive domains → good module boundaries → scalable systems.

### ✔ Increased Testability  
Small cohesive units are trivial to test.

---

## 4. Good Example (High Cohesion)

```python
class EmailValidator:
    def is_valid(self, email: str) -> bool:
        return "@" in email and "." in email
```

This class handles **one concern**: validating email format.

---

## 5. Bad Example (Low Cohesion)

```python
class Utils:
    def validate_email(self, email): ...
    def hash_password(self, pwd): ...
    def convert_pdf(self, file): ...
    def fetch_api(self, url): ...
```

Problems:
- mixes unrelated domains  
- not reusable  
- hard to maintain  
- violates SRP  
- “Utils” = red flag  

---

## 6. Cohesion Levels (CS classic scale)

From worst → best:

1. **Coincidental Cohesion** (random functions thrown together)  
2. **Logical Cohesion** (same category but unrelated tasks)  
3. **Temporal Cohesion** (run at same time, e.g., init scripts)  
4. **Procedural Cohesion** (steps in a routine)  
5. **Communicational Cohesion** (operate on same data)  
6. **Functional Cohesion** (one single, well‑defined task)  

High Cohesion = level 5 and 6.

---

## 7. Real-World Examples

### Example 1: High Cohesion in a microservice
A “User Service” should handle:
- user profile  
- user authentication  
- user settings  

It should **not** also handle:
- payments  
- analytics  
- product catalog  

### Example 2: High Cohesion in a class
```python
class ShoppingCart:
    def add_item(self, item): ...
    def remove_item(self, item): ...
    def total(self): ...
```

Class is cohesive around **cart behavior**.

---

## 8. Relation to SRP vs Cohesion
They are related but distinct:

| Principle | Meaning |
|----------|---------|
| SRP | One **reason to change** |
| Cohesion | One **conceptual purpose** |

A highly cohesive class generally satisfies SRP.

---

## 9. Signs of High Cohesion
- Easy to name (“OrderService”, “PaymentGateway”)  
- Does one conceptual job  
- Internal functions all relate to one theme  
- Changes tend to all come from the same domain context  

---

## 10. Signs of Low Cohesion
- Class names like: `Utils`, `Helpers`, `Manager`, `Processor`, `Misc`  
- Many unrelated public methods  
- Difficult to summarize responsibility in one sentence  
- Code changes often affect many areas  

---

## 11. Common Misconceptions

### ❌ “High Cohesion = small class”
Not always. A cohesive class can be large if domain is large.

### ❌ “High Cohesion = one function”
No. Cohesion is about **strongly related** functions.

### ❌ “High Cohesion = strict SRP”
High cohesion supports SRP, but SRP focuses on reasons to change.

---

## 12. Interview Questions

1. What is high cohesion?  
2. How does high cohesion relate to SRP?  
3. Give an example of a low-cohesion class.  
4. Why are “Utils” classes bad?  
5. What are the benefits of high cohesion?  
6. How do you refactor a low-cohesion module?  
7. How does high cohesion support scalability?  

---

## 13. Winning Interview Answer Template

> “High cohesion means grouping together only the responsibilities that logically belong together.  
A cohesive module does one conceptual job and keeps related behavior close together.  
It improves readability, maintainability, testability, and creates clean architectural boundaries.  
Low cohesion happens when unrelated logic is bundled, such as utils or god classes.  
I maintain high cohesion by defining clear domain boundaries and refactoring mixed responsibilities into separate modules.”

---

## 14. One-Minute Summary
- High Cohesion = modules/classes focused on a single conceptual purpose  
- Improves readability, testability, maintainability  
- Opposite of “God classes” and “Utils” dumping grounds  
- Supports SRP, Clean Architecture, Low Coupling  
