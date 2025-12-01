# DRY (Don't Repeat Yourself) 

## 1. Formal Definition
**DRY**, introduced in *The Pragmatic Programmer*, states:
> “Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.”

This emphasizes **eliminating duplication of knowledge**, not merely code.  
Knowledge includes business rules, validation logic, mappings, configuration, state transitions, and data schemas.

---

## 2. Practical Meaning in Software Engineering
DRY aims to prevent:
- Diverging business logic when similar rules evolve separately  
- Difficult maintenance due to duplicated logic  
- High testing overhead  
- Increased bugs from inconsistent updates  

“Duplication” includes:
- Repeated condition checks  
- Repeated constants  
- Repeated validation logic (front-end vs back-end)  
- Repeated mapping tables  
- Repeated schemas  
- Repeated business rules  

---

## 3. Why DRY Matters
### • Lower Maintenance Cost  
Change a rule once → instantly applied everywhere.

### • Prevent Logic Drift  
Multiple copies eventually diverge, causing inconsistent system behavior.

### • Lower Testing Cost  
Single implementation = single test suite.

### • Encourages Better Architecture  
Extracting shared logic leads to modular design, high cohesion, and cleaner abstractions.

---

## 4. Good Example (Proper DRY)

```python
DISCOUNT_THRESHOLD = 100
DISCOUNT_RATE = 0.10

def apply_discount(amount: float) -> float:
    if amount >= DISCOUNT_THRESHOLD:
        return amount * (1 - DISCOUNT_RATE)
    return amount

def price_for_new_customer(amount):
    return apply_discount(amount)

def price_for_vip(amount):
    return apply_discount(amount)
```

### ✔ Why this is good:
- A single source of discount truth  
- Consistent behavior  
- Easy future updates  

---

## 5. Bad Example (Violation of DRY)

```python
def price_for_new(amount):
    if amount >= 100:
        return amount * 0.9
    return amount

def price_for_vip(amount):
    if amount > 100:  # subtle difference!
        return amount * 0.90
    return amount
```

### ✘ Problems:
- Business rule duplicated  
- Small differences (`>=` vs `>`) cause bugs  
- High maintenance cost  

---

## 6. Common Misconceptions
### ❌ DRY = No copy-paste  
Wrong. Two pieces of logic may “look different” but still duplicate knowledge.

### ❌ DRY = Put everything in util.py  
This leads to low cohesion and God classes.

### ❌ DRY = Abstract everything early  
Premature abstraction often creates complexity.

---

## 7. When NOT to Apply DRY
DRY can be harmful when:
1. Two similar logics will evolve differently  
2. Abstraction makes code harder to read  
3. Tests require separated paths  
4. Teams change at different speeds (e.g., microservices)

---

## 8. Real-world Best Practices
- Share front-end and back-end validation via schemas  
- Extract domain logic into domain services  
- Use centralized mappings for enums and states  
- Avoid duplicating API contracts → use OpenAPI/Proto  
- Use duplication-detection tools in CI  

---

## 9. Relationship to Other Principles
| Principle | Relationship |
|----------|--------------|
| KISS | DRY must avoid over-abstraction |
| YAGNI | Don’t apply DRY too early |
| SRP | Proper DRY increases cohesion |
| Low Coupling | DRY reduces unnecessary dependencies |
| High Cohesion | DRY tends to group related logic |

---

## 10. Interview Questions
1. What is DRY and why does it matter?  
2. Give an example of DRY violation and how to fix it.  
3. How do you keep validation DRY across front-end and back-end?  
4. When is DRY harmful?  
5. How do you avoid over-abstraction?  
6. Why is logical drift dangerous?  
7. What is duplicated knowledge vs duplicated code?  
8. How do you apply DRY in microservices?  
9. How can DRY conflict with YAGNI?  
10. Give a real example from your experience.

---

## 11. Winning Interview Answer Template
> “DRY ensures each piece of business knowledge has one authoritative implementation.  
It prevents logic drift, reduces bugs, lowers testing cost, and improves maintainability.  
I centralize domain rules in shared services or schemas, but I avoid premature abstraction.  
If two similar logics will evolve differently, I intentionally keep them separate.”

---

## 12. One-Minute Summary
- DRY targets knowledge duplication  
- Repeated logic → future inconsistencies  
- Dangerous repeats: business rules, validation, state machines  
- Over-abstraction can violate KISS  
- DRY + YAGNI balance is key  

