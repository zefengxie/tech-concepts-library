# Low Coupling 

## 1. Formal Definition
Low Coupling means:
> **Modules should depend on each other as little as possible.**  
Dependencies should be minimal, explicit, and stable.

Coupling describes **how strongly one component relies on another**.

- High coupling → rigid, fragile, hard to test
- Low coupling → flexible, modular, maintainable

---

## 2. Practical Meaning in Software Engineering
Low coupling means:

- A class should not know unnecessary details about others  
- Changing one module should not break many others  
- Dependencies should be abstractions, not concrete implementations  
- Communication should happen through stable interfaces  
- Modules should be replaceable independently  

---

## 3. Why Low Coupling Matters

### ✔ Maintainability  
Systems with low coupling allow easy updates.

### ✔ Testability  
You can mock or stub dependencies easily.

### ✔ Reusability  
Modules can be used elsewhere without dragging along a large dependency graph.

### ✔ Scalability  
Teams can work independently on separate modules.

### ✔ Resilience  
Failures in one module don't cascade easily.

---

## 4. Good Example (Low Coupling via Abstraction)

```python
class PaymentGateway:
    def charge(self, amount): ...

class StripeGateway(PaymentGateway):
    def charge(self, amount): ...

class CheckoutService:
    def __init__(self, gateway: PaymentGateway):
        self.gateway = gateway

    def checkout(self, amount):
        return self.gateway.charge(amount)
```

✔ CheckoutService does not depend on Stripe specifics  
✔ Easy to replace Stripe with PayPal, MockGateway, or others  

---

## 5. Bad Example (High Coupling)

```python
class CheckoutService:
    def checkout(self, amount):
        stripe = StripeGateway("api-key", "region", "extra-flags")
        return stripe.stripe_charge(amount)
```

Problems:
- Hardcoded dependency  
- Breaks DIP  
- Cannot test without calling Stripe  
- Cannot switch payment providers  
- Changes in Stripe API break business logic  

---

## 6. Types of Coupling (From Worst → Best)

1. **Content Coupling** — one module changes internal data of another  
2. **Common Coupling** — shared global variables  
3. **External Coupling** — shared external formats  
4. **Control Coupling** — one module dictates the logic of another  
5. **Stamp Coupling** — passing whole objects when only part is needed  
6. **Data Coupling** — passing simple, necessary data  
7. **No Coupling** — modules operate independently  

Low Coupling = aim for **data coupling** whenever possible.

---

## 7. How to Achieve Low Coupling

### • Use interfaces/abstract classes  
Depend on behaviors, not implementations.

### • Use dependency injection  
Pass dependencies from outside.

### • Apply SOLID principles  
Especially DIP, SRP, ISP.

### • Hide implementation details via encapsulation  
Expose only necessary APIs.

### • Avoid global variables  
They create invisible coupling.

### • Use modules to group related code  
And hide private helpers.

---

## 8. Signs of Low Coupling
- Module is easy to test with mocks  
- Replacing a dependency requires minimal code change  
- Modules are independent in behavior  
- Code changes are localized  
- Small and clear public API  

---

## 9. Signs of High Coupling
- Changing one class breaks many others  
- Circular dependencies  
- Long parameter lists passing entire objects  
- Shared global state  
- Logic spread across unrelated modules  
- Hard-to-test code requiring real databases/APIs  

---

## 10. Common Misconceptions

### ❌ “Low coupling means zero dependencies.”  
No. Systems need dependencies — they should just be **lightweight and stable**.

### ❌ “Low coupling means everything must be microservices.”  
Microservices can increase coupling if designed poorly.

### ❌ “Low coupling requires interfaces everywhere.”  
Use interfaces where variability exists.  
Don’t abstract prematurely.

---

## 11. Low Coupling vs High Cohesion
They work together:

| Concept | Meaning |
|---------|---------|
| High Cohesion | Group related logic inside a module |
| Low Coupling | Minimize dependency between modules |

A good module is:
- cohesive inside,
- loosely coupled outside.

---

## 12. Interview Questions

1. What is low coupling?  
2. Give an example of high vs low coupling.  
3. Why is low coupling important in microservices?  
4. What problems does high coupling create?  
5. How does DIP reduce coupling?  
6. How do you design low-coupled systems?  
7. What is the difference between cohesion and coupling?  

---

## 13. Winning Interview Answer Template

> “Low coupling means minimizing dependencies between modules so that changes in one part do not ripple through the system.  
It improves testability, maintainability, reusability, and modularity.  
I achieve low coupling through dependency injection, clear interfaces, encapsulation, and avoiding shared global state.  
Low coupling works best with high cohesion, giving systems stability and flexibility.”

---

## 14. One-Minute Summary
- Low Coupling = minimize inter-module dependency  
- Improves testing, maintainability, modularity  
- Avoid global state, hardcoded dependencies, and concrete coupling  
- Works tightly with high cohesion and SOLID (especially DIP)  
