# Pure Function 

## 1. Formal Definition
A **pure function** is a function that satisfies two strict conditions:

1. **Deterministic**  
   Given the same input, it *always* returns the same output.

2. **No Side Effects**  
   It does **not modify** anything outside its scope:
   - no global variables  
   - no I/O  
   - no database operations  
   - no network calls  
   - no mutation of external objects  
   - no randomness  
   - no time-based values  

Pure functions are the mathematical ideal of computation.

---

## 2. Practical Meaning in Software Engineering
A pure function:

- treats inputs like data and produces outputs like data  
- is *isolated* and *self-contained*  
- does not rely on shared state  
- cannot be affected by the outside world  
- cannot affect the outside world  

This makes pure functions:

- predictable  
- reliable  
- safer  
- easier to test  

---

## 3. Why Pure Functions Matter

### ✔ Predictability  
They behave the same every single time.

### ✔ Testability  
Testing pure functions requires:
```python
assert func(a, b) == expected
```
No mocking, no setup, no teardown.

### ✔ Debuggability  
Bugs can only be caused by:
- incorrect logic  
- incorrect inputs  

No mysterious global changes.

### ✔ Concurrency Safety  
No shared state → no race conditions.

### ✔ Refactor Safety  
Pure functions don't depend on external context → easy to move, reuse, refactor.

### ✔ Functional Programming Foundations  
FP languages build entire systems from pure functions:
- map  
- reduce  
- filter  

---

## 4. Good Example (True Pure Function)

```python
def add(a: int, b: int) -> int:
    return a + b
```

✓ Same input → same output  
✓ No side effects  
✓ No mutation  

---

## 5. Bad Examples (NOT Pure Functions)

### ❌ Example 1: Reads global state
```python
discount = 0.1

def price_after_discount(amount):
    return amount * (1 - discount)
```

Breaks purity because output depends on external variable.

---

### ❌ Example 2: Mutates input
```python
def add_item(cart, item):
    cart.append(item)  # modifies external state!
```
Side effect = impure.

---

### ❌ Example 3: Randomness
```python
import random
def roll():
    return random.randint(1,6)
```

Non-deterministic → impure.

---

### ❌ Example 4: I/O operations
```python
def load_user():
    return open("user.json").read()
```

File system access → side effect.

---

## 6. How to Purify Impure Functions

### Example: Move side effects outward

Bad:
```python
def compute_total():
    prices = fetch_prices_from_api()
    return sum(prices)
```

Good:
```python
def compute_total(prices):
    return sum(prices)
```

Let *callers* handle side effects (API call).

---

## 7. When Pure Functions Are Especially Valuable

- Data processing pipelines  
- Machine learning transformations  
- Game logic  
- Compilers / interpreters  
- Backend validation  
- Business rules  
- Mathematical computation  
- Multithreading environments  

---

## 8. When Pure Functions Should NOT Be Used Alone

You cannot build a real program without:
- I/O  
- state changes  
- network calls  
- persistence  

Thus:  
**Pure functions handle logic; impure functions handle integration.**

FP style:
- “core” = pure  
- “shell” = impure wrappers

---

## 9. Pure Functions vs Idempotency vs Immutability

| Concept | Meaning |
|--------|---------|
| Pure Function | No side effects + deterministic output |
| Idempotent | Repeated calls have same *effect* |
| Immutable | State cannot change once created |

Pure functions → often rely on immutability  
Idempotent ≠ pure (can still have side effects)

---

## 10. Relationship to Other Principles

| Principle | Link |
|----------|------|
| Immutability | Pure functions prefer immutable inputs |
| DRY | Pure functions eliminate duplicated logic |
| KISS | Pure functions keep logic simple |
| High Cohesion | Pure functions focus on one task |
| Low Coupling | Pure functions do not depend on global state |
| Testability | Pure functions are trivial to test |

---

## 11. Interview Questions

1. What is a pure function?  
2. Why are pure functions important for testing?  
3. Are pure functions always stateless?  
4. Can pure functions exist in OOP?  
5. How do pure functions help concurrency?  
6. Give examples of pure vs impure functions.  
7. Is idempotent equal to pure?  
8. Why does functional programming prioritize purity?  
9. How would you refactor impure code into pure functions?  

---

## 12. Winning Interview Answer Template

> “A pure function always returns the same output for the same input and produces no side effects.  
This makes it predictable, easy to test, safe for concurrency, and highly reusable.  
I isolate business rules into pure functions and move side effects to the edges of the system to improve maintainability and reliability.”

---

## 13. One-Minute Summary
- Pure Function = deterministic + no side effects  
- Ideal for logic, transformation, data processing  
- Easy to test, debug, reuse  
- Impure code should be isolated at system boundaries  
- Core logic pure → outer layers impure  
