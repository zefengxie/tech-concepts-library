# Dependency Injection (DI) 

## 1. Formal Definition
**Dependency Injection (DI)** is a design pattern where:
> A class **does not create its own dependencies** — instead, they are **provided (injected)** from the outside.

This means:
- Objects do not instantiate their collaborators directly  
- Dependencies are passed in via:
  - constructor injection (recommended)
  - method injection
  - property injection  

DI is the most common way to implement **DIP (Dependency Inversion Principle)**.

---

## 2. Practical Meaning in Software Engineering

Without DI:
- Classes create their own dependencies  
- Tight coupling emerges  
- Hard to test  
- Hard to replace components  
- Hard to mock  

With DI:
- Dependencies are flexible  
- Components become interchangeable  
- Unit tests can inject mocks/fakes easily  
- Architecture becomes modular and clean  

DI is everywhere:
- Spring Boot  
- .NET Core  
- NestJS  
- Angular  
- FastAPI dependency injection  
- Cloud-native microservices  

---

## 3. Why Dependency Injection Matters

### ✔ Reduces Coupling  
The class depends on *an abstraction*, not the concrete implementation.

### ✔ Increases Testability  
You can inject mocks instead of real services.

### ✔ Enables Easy Swapping of Implementations  
Switch:
- Stripe → PayPal  
- MySQL → PostgreSQL  
- SMS provider → Email provider  

without changing business code.

### ✔ Improves Maintainability  
Dependencies are configurable in one place.

### ✔ Encourages Clean Architecture  
Business logic is separated from infrastructure.

### ✔ Supports Inversion of Control (IoC)  
DI is the mechanism for IoC containers.

---

## 4. Good Example (Proper DI)

```python
class EmailSender:
    def send(self, to, body): ...

class SMTPSender(EmailSender):
    def send(self, to, body):
        print(f"SMTP send to {to}")

class NotificationService:
    def __init__(self, sender: EmailSender):
        self.sender = sender

    def notify(self, user, msg):
        self.sender.send(user.email, msg)

smtp = SMTPSender()
service = NotificationService(smtp)
```

✓ Dependency passed in  
✓ Implementation replaceable  
✓ Easy to mock  

---

## 5. Bad Example (Hard-coded Dependency)

```python
class NotificationService:
    def __init__(self):
        self.sender = SMTPSender()  # WRONG: hardcoded dependency

    def notify(self, user, msg):
        self.sender.send(user.email, msg)
```

✘ Cannot replace SMTPSender  
✘ Not testable  
✘ High coupling  
✘ Violates DIP  

---

## 6. Types of Dependency Injection

### • Constructor Injection (Best Practice)
```python
def __init__(self, repo: UserRepository):
```

### • Method Injection
```python
def send(self, notifier):
    notifier.notify(...)
```

### • Property Injection
```python
service.sender = PushNotifier()
```

### • Framework/Container Injection
Used in DI frameworks:
- Angular
- NestJS
- Spring
- .NET Core

---

## 7. Dependency Injection vs Inversion of Control (IoC)

| Concept | Meaning |
|---------|---------|
| DI | A technique to provide external dependencies |
| IoC | A broader principle: program flow controlled externally |

**DI is one way to implement IoC** (the most common way).

---

## 8. Best Practices

### ✔ Depend on Abstract Interfaces  
Not concrete implementations.

### ✔ Use Constructor Injection  
Simplest, cleanest, most explicit.

### ✔ Avoid Injecting Too Many Dependencies  
If a class has > 3 dependencies → it likely violates SRP.

### ✔ Design Narrow Interfaces  
ISP (Interface Segregation) improves DI use.

### ✔ Use DI Containers Only When Needed  
Containers help manage object graphs, but avoid overcomplexity.

---

## 9. Real-World Examples

### Example 1: Changing database provider
Business code stays the same; only injected repository changes.

### Example 2: Unit testing with mocks
```python
mock = MockEmailSender()
service = NotificationService(mock)
```

### Example 3: Microservices
Services receive injected clients for:
- Redis  
- Kafka  
- SQS  
- HTTP clients  

Instead of creating them internally.

---

## 10. Common Misconceptions

### ❌ “DI means using a DI container”
No — DI is a pattern, not a framework.

### ❌ “DI is only for big systems”
Even small apps benefit from testability and flexibility.

### ❌ “Everything should be injected”
Don’t inject constants or trivial dependencies.

### ❌ “DI makes code complicated”
Manual DI is extremely simple — just pass an object as a parameter.

---

## 11. Signs You Need DI

- Hardcoded dependencies everywhere  
- Hard to write unit tests  
- You must mock global singletons  
- Replacing implementations requires touching many files  
- Adding features requires modifying deeply nested logic  
- Constructors create objects (`new` calls scattered everywhere)

---

## 12. Interview Questions

1. What is dependency injection?  
2. How does DI help with testing?  
3. What is the difference between DI and IoC?  
4. Why is constructor injection preferred?  
5. How does DI relate to DIP?  
6. Give an example of replacing a dependency with DI.  
7. What problems arise without DI?  
8. What is a DI container?  
9. When should DI not be used?  

---

## 13. Winning Interview Answer Template

> “Dependency Injection means providing a class its dependencies from the outside rather than creating them internally.  
It reduces coupling, improves testability, and supports the Dependency Inversion Principle.  
I use DI to keep business logic independent of infrastructure, allowing easy swapping of implementations and cleaner, modular architecture.”

---

## 14. One-Minute Summary
- DI = dependencies provided externally  
- Promotes testability, modularity, flexibility  
- Supports DIP, IoC, SRP  
- Avoid hardcoded dependencies  
- Constructor injection is best practice  
