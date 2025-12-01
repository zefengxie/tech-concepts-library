# Deadlock

## 1. Formal Definition
A **deadlock** is a state in which:
> A set of threads or processes are **permanently blocked**, each waiting for resources held by others, so none of them can proceed.

Classic cycle:
- Thread A holds resource X, waits for Y  
- Thread B holds resource Y, waits for X  
→ both block forever.

---

## 2. Coffman’s Four Conditions (Deadlock Requirements)

Deadlock can occur only if **all four** hold:

1. **Mutual Exclusion**  
   Resources are non-shareable (one thread at a time).

2. **Hold and Wait**  
   A thread holds one resource and waits for others.

3. **No Preemption**  
   Resources can’t be forcibly taken; they’re released voluntarily.

4. **Circular Wait**  
   A cycle of threads exists: T1 → T2 → … → Tn → T1.

Preventing deadlock often means breaking at least **one** of these.

---

## 3. Practical Example (Two Locks, Two Threads)

```python
import threading, time

lock_a = threading.Lock()
lock_b = threading.Lock()

def t1():
    with lock_a:
        time.sleep(0.1)
        with lock_b:
            print("T1 acquired both")

def t2():
    with lock_b:
        time.sleep(0.1)
        with lock_a:
            print("T2 acquired both")

threading.Thread(target=t1).start()
threading.Thread(target=t2).start()
```

Possible outcome:
- T1 locks A, waits for B  
- T2 locks B, waits for A  
→ neither can proceed = deadlock.

---

## 4. Why Deadlocks Are Dangerous

- Block threads forever  
- Consume resources (threads, locks, CPU waiting)  
- Cause system hangs  
- Difficult to reproduce  
- Can occur only under load → production-only bug  

---

## 5. Strategies to Prevent/Handle Deadlocks

### 5.1 Lock Ordering (Most Practical)

Define a **global order** of resources and always acquire locks in that order.

Example:
- Always acquire lock A before lock B.

This prevents circular wait.

---

### 5.2 Try-Lock with Timeout

```python
if lock_a.acquire(timeout=1):
    try:
        if lock_b.acquire(timeout=1):
            try:
                ...
            finally:
                lock_b.release()
    finally:
        lock_a.release()
```

If not acquired → back off / retry / fail gracefully.

---

### 5.3 Lock Hierarchies / Granularity

- Use fewer locks  
- Use coarse-grained locks (but may reduce parallelism)  
- Or use fine-grained locks but very strict ordering.

---

### 5.4 Avoid Hold-and-Wait

Acquire **all needed resources at once**, or:
- release all if one can’t be acquired.

---

### 5.5 Immutable or Lock-Free Structures

Use:
- atomic operations
- lock-free algorithms
- message-passing / actor model

---

## 6. Deadlock in Databases

Deadlocks can appear when:
- multiple transactions lock rows in different orders.

DBMS usually:
- detects deadlock via wait-for graph
- kills one transaction (“deadlock victim”)
- returns error to retry

Good practice:
- Access tables/rows in consistent order  
- Keep transactions short  

---

## 7. Detection vs Prevention vs Avoidance

- **Prevention**: design system so deadlocks can’t occur (e.g., lock ordering).  
- **Avoidance**: dynamically decide whether to grant resource based on safe state (e.g., Banker’s algorithm).  
- **Detection**: allow deadlocks, then detect and recover (kill one process).

In application code, we typically use **prevention + timeout**.

---

## 8. Common Misconceptions

### ❌ “Deadlock only occurs with threads”
No — can happen with:
- DB transactions  
- Distributed locks  
- Microservices waiting on each other  

### ❌ “Using ‘synchronized’ or ‘lock’ automatically makes code safe”
Naive locking can easily create deadlocks.

---

## 9. Interview Questions

1. What is a deadlock?  
2. What four conditions are required for deadlock?  
3. Show an example of code that can deadlock.  
4. How would you prevent deadlocks?  
5. How do databases handle deadlocks?  
6. What is lock ordering?  
7. Difference between deadlock, livelock, and starvation?  

---

## 10. Winning Interview Answer Template

> “A deadlock is when two or more threads are permanently blocked, each waiting for resources held by the others.  
Deadlock requires mutual exclusion, hold-and-wait, no preemption, and circular wait.  
I prevent deadlocks by using consistent lock ordering, timeouts, short-lived locks, and sometimes redesigning to avoid shared locks altogether.”

---

## 11. One-Minute Summary

- Deadlock = permanent waiting cycle  
- Requires four Coffman conditions  
- Prevent via lock ordering, timeouts, careful design  
- Common in threading, DBs, distributed systems  
