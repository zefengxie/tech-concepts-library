# Convention Over Configuration (CoC) 

## 1. Formal Definition
**Convention Over Configuration (CoC)** is a software design principle that states:

> A system should follow sensible **default conventions** so developers do not need to write repetitive configuration.  
> Configuration should only be required when deviating from the convention.

CoC reduces:
- boilerplate  
- setup effort  
- configuration files  
- complexity  

It emphasizes:
- “Do it the default way”  
- “Configure only when necessary”  

---

## 2. Practical Meaning in Software Engineering

Without CoC:
- developers must specify *every detail*  
- routes, folders, mapping, naming, wiring  
- massive config files (YAML/JSON/XML)

With CoC:
- framework infers behavior automatically  
- developers spend less time configuring  
- faster development cycles  

Examples:
- Rails, Django, Spring Boot  
- Next.js file‑based routing  
- Angular/NestJS modules  
- ORMs conventioned models → tables  

---

## 3. Why CoC Matters

### ✔ Eliminates Boilerplate  
Reduces config by relying on defaults.

### ✔ Faster Development  
You focus on business logic, not framework setup.

### ✔ Easier Onboarding  
Everyone understands conventional structure.

### ✔ Predictability  
Codebases share uniform layout and naming.

### ✔ Fewer Bugs  
Less configuration = fewer chances for misconfiguration.

---

## 4. Real‑World Examples

### 4.1 Rails Routing Convention
Controller name:
```
users_controller.rb
```
Automatically maps to:
```
/users
```

### 4.2 Next.js File‑Based Routing
File:
```
pages/profile.js
```
Automatically becomes route:
```
/profile
```

### 4.3 Django Folder Conventions
- `models.py` → ORM models  
- `views.py` → view handlers  
No config required.

### 4.4 Spring Boot Auto Configuration
Add one dependency:
```
spring-boot-starter-web
```
Spring automatically:
- configures Tomcat  
- sets up JSON parser  
- wires controllers  
- applies DI  

---

## 5. Good Example (CoC Applied)

```python
# Django
# models.py
class Product(models.Model):
    name = models.CharField(max_length=200)
```

Django:
- auto‑maps DB table  
- auto‑creates admin UI  
- auto‑maps model name → table name  

No configuration required.

---

## 6. Bad Example (Over‑Configuration)

```xml
<!-- old Spring XML -->
<bean id="productService"
      class="com.app.services.ProductServiceImpl" />
```

This is what CoC removes.

---

## 7. Relationship to Other Principles

| Principle | Relationship |
|-----------|--------------|
| DRY | CoC removes repeated config |
| KISS | Defaults keep architecture simple |
| Clean Code | Predictable structure → easy to read |
| IoC | Convention often drives container auto‑wiring |
| High Cohesion | Conventions group related components |

---

## 8. When CoC is NOT Suitable

### ❌ Highly custom systems  
Defaults may not fit.

### ❌ When debugging framework “magic”  
Too much automation can obscure behavior.

### ❌ When overriding defaults frequently  
If you override 70%+ of conventions → CoC becomes burden.

### ❌ Regulated/enterprise environments  
Explicit configuration is sometimes required.

---

## 9. Interview Questions

1. What is Convention Over Configuration?  
2. Why does CoC improve developer productivity?  
3. Give examples of CoC in frameworks you’ve used.  
4. When is CoC a bad idea?  
5. Difference between CoC and IoC?  
6. How does file‑based routing use CoC?  
7. How does CoC differ from DRY?  

---

## 10. Winning Interview Answer Template

> “Convention Over Configuration means using sensible defaults so developers don’t need to write repetitive configuration.  
It reduces boilerplate, accelerates development, and makes codebases predictable.  
Frameworks like Rails, Django, Spring Boot, and Next.js use CoC for routing, wiring, and structure.  
CoC works best when applications follow common patterns; otherwise explicit configuration is preferable.”

---

## 11. One‑Minute Summary

- CoC = framework defaults reduce config  
- Faster development, cleaner codebase  
- Used by Rails, Django, Spring Boot, Next.js  
- Bad when conventions don’t fit the system  
