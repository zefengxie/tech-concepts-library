# SOLID Principles 

## Overview
SOLID represents five foundational principles of maintainable object‑oriented design:

1. **S – Single Responsibility Principle (SRP)**  
2. **O – Open/Closed Principle (OCP)**  
3. **L – Liskov Substitution Principle (LSP)**  
4. **I – Interface Segregation Principle (ISP)**  
5. **D – Dependency Inversion Principle (DIP)**  

Each principle attacks a different form of complexity.

---

# 1. Single Responsibility Principle (SRP)

## Formal Definition
A class/module should have **one reason to change** — one primary responsibility.

## Why It Matters
- Reduces coupling between unrelated logic  
- Makes code easier to test  
- Prevents massive “God classes”  
- Enables iterative change without breaking unrelated behavior  

## Good Example
```python
class InvoiceCalculator:
    def calculate_total(self, items):
        return sum(item.price for item in items)

class InvoicePrinter:
    def print(self, invoice):
        print(f"Invoice #{invoice.id}: {invoice.total}")
```

## Bad Example
```python
class Invoice:
    def calculate_total(self): ...
    def print(self): ...
    def save_to_db(self): ...
```
Mixed responsibilities → highly brittle.

## Misconceptions
- SRP ≠ “one method only”  
- SRP refers to **reasons to change**, not size  

## Interview Questions
- What is SRP?  
- Give an example of a God class you refactored.  
- How does SRP relate to cohesion?

---

# 2. Open/Closed Principle (OCP)

## Formal Definition
Software entities should be **open for extension** but **closed for modification**.

## Why It Matters
- New features don’t break existing features  
- Reduces regression risk  
- Enables plugin‑like modularity  

## Good Example
```python
class DiscountStrategy:
    def apply(self, amount): return amount

class ChristmasDiscount(DiscountStrategy):
    def apply(self, amount): return amount * 0.9

class PriceService:
    def __init__(self, strategy: DiscountStrategy):
        self.strategy = strategy

    def final_price(self, amount):
        return self.strategy.apply(amount)
```

## Bad Example
```python
def get_price(amount, type):
    if type == "christmas": return amount * 0.9
    if type == "vip": return amount * 0.8
```
Adding a new type → modify the function again.

## Misconceptions
- OCP does *not* mean “never change existing code”  
- It means “don’t modify the internals for new behavior”

## Interview Questions
- Explain OCP with an example.  
- How do strategy/abstract classes help OCP?

---

# 3. Liskov Substitution Principle (LSP)

## Formal Definition
Subtypes must be substitutable for their base types without breaking correctness.

Meaning:  
> If A is a subtype of B, code using B should work with A without modification.

## Why It Matters
- Prevents inheritance misuse  
- Ensures polymorphism behaves correctly  
- Avoids runtime surprises  

## Good Example
```python
class Bird:
    def fly(self): ...

class Sparrow(Bird):
    pass
```

## Bad Example (Classic)
```python
class Bird:
    def fly(self): ...

class Ostrich(Bird):
    def fly(self):
        raise Exception("I can't fly!")
```
Ostrich violates LSP.

## Signs of LSP Violations
- Methods raise “Not supported”  
- Subclass adds unexpected side effects  
- Subclass tightens preconditions  

## Interview Questions
- What is LSP?  
- Give a real example of violating LSP.  
- Why does LSP matter in inheritance hierarchies?

---

# 4. Interface Segregation Principle (ISP)

## Formal Definition
Clients should not be forced to depend on interfaces they do not use.  
Prefer **many small, focused interfaces** over one “fat” interface.

## Why It Matters
- Reduces unnecessary coupling  
- Enhances flexibility  
- Improves testability  

## Good Example
```python
class Printer:
    def print(self): ...

class Scanner:
    def scan(self): ...
```

## Bad Example
```python
class Machine:
    def print(self): ...
    def scan(self): ...
    def fax(self): ...
```
Classes needing only “print” must still implement scan/fax → ISP violation.

## Interview Questions
- What problem does ISP solve?  
- Why are “fat interfaces” harmful?

---

# 5. Dependency Inversion Principle (DIP)

## Formal Definition
1. High-level modules should not depend on low-level modules.  
2. Both should depend on **abstractions**.

## Why It Matters
- Reduces coupling  
- Enables mocking/testability  
- Allows swapping implementations (e.g., different DBs)  

## Good Example
```python
class EmailSender:
    def send(self, to, body): ...

class SMTPSender(EmailSender):
    def send(self, to, body): ...

class NotificationService:
    def __init__(self, sender: EmailSender):
        self.sender = sender
```

## Bad Example
```python
class NotificationService:
    def __init__(self):
        self.sender = SMTPSender()  # hardcoded dependency
```

## Interview Questions
- What is DIP?  
- How does DI (dependency injection) help?  
- How does DIP relate to modular architecture?

---

# Full One‑Minute Summary

- **SRP**: One reason to change → avoid God classes  
- **OCP**: Extend behavior without modifying existing code  
- **LSP**: Subclasses must behave like their parent  
- **ISP**: Prefer small focused interfaces  
- **DIP**: Depend on abstractions, not concretions  

Together, SOLID produces modular, testable, maintainable systems.

