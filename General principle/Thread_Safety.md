# Thread Safety 

## 1. Formal Definition
A piece of code is **thread-safe** if:
> It functions correctly during **concurrent execution** by multiple threads, regardless of scheduling or interleaving, without causing race conditions, data corruption, or undefined behavior.

Thread-safe code:
- maintains invariants  
- does not corrupt shared state  
- behaves deterministically (within spec) under concurrency  

---

## 2. Practical Meaning in Software Engineering

Thread safety is required when:
- multiple threads access shared data  
- multiple requests handle the same resource  
- asynchronous operations run in parallel  

Thread-safe code can be:
- reentrant
- stateless
- protected by synchronization
- using immutable state
- using thread-local or isolated state  

---

## 3. Strategies to Achieve Thread Safety

### 3.1 Immutability
Immutable objects are inherently thread-safe.

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Money:
    amount: int
    currency: str
```

No mutation → safe to share across threads.

---

### 3.2 Synchronization (Locks/Mutexes)

```python
from threading import Lock

lock = Lock()
counter = 0

def increment():
    global counter
    with lock:
        counter += 1
```

Protects critical sections.

---

### 3.3 Confinement / Thread-Local State

Each thread has its own data; no sharing.

```python
import threading

local = threading.local()

def worker():
    local.cache = {}
```

---

### 3.4 Using Thread-Safe Libraries

Examples:
- concurrent queues
- thread-safe collections
- database connections using connection pools

---

## 4. Thread Safety Levels

- **Not thread-safe**: breaks under concurrent use  
- **Conditionally thread-safe**: safe if used correctly (e.g., external locking)  
- **Thread-safe**: safe for concurrent access  
- **Lock-free / Wait-free**: advanced; no blocking  

---

## 5. Examples

### 5.1 Non-Thread-Safe Counter

```python
class Counter:
    def __init__(self):
        self.value = 0

    def inc(self):
        self.value += 1
```

Used by multiple threads → race condition.

---

### 5.2 Thread-Safe Counter

```python
from threading import Lock

class SafeCounter:
    def __init__(self):
        self.value = 0
        self._lock = Lock()

    def inc(self):
        with self._lock:
            self.value += 1
```

---

## 6. Why Thread Safety Matters

- Prevents data corruption  
- Avoids race conditions and deadlocks  
- Improves reliability at scale  
- Essential for high-performance servers  

---

## 7. Common Misconceptions

### ❌ “Python has a GIL, so everything is thread-safe”
Wrong. GIL does not protect you from race conditions on your own shared data structures.

### ❌ “Marking something synchronized equals thread safety”
Incorrect — wrong synchronization strategy can still cause races or deadlocks.

### ❌ “Using threads always improves performance”
Synchronization costs and contention can degrade performance.

---

## 8. Interview Questions

1. What is thread safety?  
2. How do you make a class thread-safe?  
3. Difference between immutability and synchronization?  
4. What is thread confinement?  
5. How does GIL affect thread safety?  
6. Give an example of thread-unsafe code and fix it.  

---

## 9. Winning Interview Answer Template

> “Thread safety means code behaves correctly when accessed concurrently by multiple threads, without race conditions or data corruption.  
I achieve thread safety using immutability, locks, thread-local state, and thread-safe data structures, and I design APIs to minimize shared mutable state.”

---

## 10. One-Minute Summary

- Thread-safe = safe under concurrent use  
- Implement via immutability, locks, confinement, thread-safe collections  
- Not guaranteed by GIL or synchronized alone  
- Essential for scalable, safe concurrent systems  
