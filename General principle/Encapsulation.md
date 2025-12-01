# Encapsulation

## 1. Formal Definition
Encapsulation is an OOP principle that means:
> **Bundling data and behavior together AND restricting direct access to internal state.**

Two key ideas:
1. **Data Hiding** — internal state is not directly exposed.  
2. **Controlled Access** — interaction happens through well‑defined methods.

Encapsulation protects objects from:
- invalid states,
- external interference,
- unintended misuse,
- tight coupling.

---

## 2. Practical Meaning in Software Engineering
Encapsulation solves a real-world problem:

> Code that freely manipulates each other's internal variables becomes impossible to change safely.

Encapsulation ensures:
- internal state can be changed only via controlled logic,
- invariants are maintained consistently,
- implementation can evolve without breaking other code.

Examples of invariants:
- “balance cannot be negative”
- “email must contain '@'”
- “order status can only move forward”

---

## 3. Why Encapsulation Matters

### ✔ Prevents Invalid or Corrupt State
Without encapsulation:
- fields can be mutated into illegal values,
- invariants become impossible to enforce.

### ✔ Decouples Implementation From Interface
You can change:
- how values are stored,
- how calculations are made,
- how rules are applied  
**without breaking other code.**

### ✔ Makes Code Safer and More Predictable
Controlled access reduces unexpected side effects from external code.

### ✔ Enables Clean APIs and Domain Modeling
Objects expose meaningful behaviours, not raw fields.

---

## 4. Good Example (Proper Encapsulation)

```python
class BankAccount:
    def __init__(self, balance: float):
        if balance < 0:
            raise ValueError("Balance cannot be negative")
        self._balance = balance  # “protected” value

    @property
    def balance(self):
        return self._balance

    def deposit(self, amount: float):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self._balance += amount

    def withdraw(self, amount: float):
        if amount <= 0:
            raise ValueError("Withdrawal must be positive")
        if amount > self._balance:
            raise ValueError("Insufficient funds")
        self._balance -= amount
```

### ✔ Why good:
- `_balance` is hidden  
- operations enforce business invariants  
- state always remains valid  
- internal changes won’t break callers  

---

## 5. Bad Example (Violation of Encapsulation)

```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance  # fully exposed

acc = BankAccount(100)
acc.balance = -999  # externally corrupted state
```

### ✘ Problems:
- state becomes invalid  
- no invariants enforced  
- no guarantee of correctness  
- all business code must re-check the same rules → DRY violation  

---

## 6. Encapsulation vs Abstraction  
They are related but not the same.

| Concept | Definition |
|--------|------------|
| **Abstraction** | Hiding complexity by exposing essential concepts |
| **Encapsulation** | Hiding internal state & implementation details |

**Abstraction = what an object does**  
**Encapsulation = how it hides its internals**

Example:
- A “Car” abstraction exposes `drive()` and `brake()`
- Encapsulation hides the engine internals, gear ratios, torque handling, etc.

---

## 7. Typical Encapsulation Tools in Modern Languages

### Python
- Naming conventions: `_protected`, `__private`
- Properties (`@property`)
- Modules and packages

### Java / C# / C++
- `public`, `private`, `protected`
- getters and setters
- access modifiers at class level

### JavaScript / TypeScript
- private `#fields`
- closures
- modules

---

## 8. Common Misconceptions

### ❌ “Encapsulation = using getters and setters everywhere”
Wrong.
- Getters and setters can also *break* encapsulation if misused.
- If they expose raw fields with no logic, they are **public fields in disguise**.

### ❌ “Encapsulation prevents flexibility”
No.
It **increases flexibility** because implementation can change safely.

### ❌ “Encapsulation is only for OOP”
Also applies to:
- modules,
- services,
- API boundaries,
- microservices,
- entire distributed architecture.

---

## 9. Signs of Good Encapsulation
- State cannot be invalid
- Methods enforce domain rules
- Object behavior is predictable
- API is small and intentional
- Callers cannot break invariants

---

## 10. Signs of Poor Encapsulation
- public fields everywhere
- direct mutation from outside
- too many getters/setters with zero logic
- invariants enforced inconsistently
- state that can be changed in unexpected ways

---

## 11. Deep Example: Encapsulation in a Domain Model

### Good
```python
class Order:
    def __init__(self):
        self._status = "created"

    def pay(self):
        if self._status != "created":
            raise RuntimeError("Cannot pay in current state")
        self._status = "paid"

    def ship(self):
        if self._status != "paid":
            raise RuntimeError("Cannot ship an unpaid order")
        self._status = "shipped"
```

### Bad
```python
class Order:
    def __init__(self):
        self.status = "created"  # public

order = Order()
order.status = "shipped"  # illegal state
```

---

## 12. Relationship to Other Principles

| Principle | Relationship |
|----------|--------------|
| SRP | Encapsulated objects tend to have clear responsibilities |
| DRY | Encapsulation centralizes logic → prevents duplication |
| LSP | Proper encapsulation ensures substitutable behavior |
| DIP | Encapsulation hides implementation from high-level modules |
| KISS | Simplifies usage through a clear interface |

---

## 13. Interview Questions

1. What is encapsulation?  
2. What’s the difference between abstraction and encapsulation?  
3. Why is encapsulation important in OOP?  
4. Show a real example of bad encapsulation you refactored.  
5. How does encapsulation improve testability?  
6. Can getters and setters break encapsulation? When?  
7. How do modules achieve encapsulation?  

---

## 14. Winning Interview Answer Template

> “Encapsulation bundles data and behavior while hiding internal implementation details.  
It enforces invariants and prevents invalid or inconsistent states.  
Encapsulation allows the implementation of a class to change without breaking its callers, making systems safer, more modular, and easier to maintain.  
I use encapsulation to enforce domain rules and expose only necessary behaviors through a clean API.”

---

## 15. One-Minute Summary
- Encapsulation = hide internal state & enforce invariants  
- Protects objects from invalid or corrupted state  
- Decouples interface from implementation  
- Makes code safer, more modular, easier to test  
- Not the same as abstraction  
