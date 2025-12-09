# Promise

## 1. Definition

A **Promise** is an object that represents the **eventual completion or failure** of an asynchronous operation and its resulting value.

A Promise is always in one of three states:
- **Pending** — initial state, neither fulfilled nor rejected
- **Fulfilled** — operation completed successfully
- **Rejected** — operation failed

Once a Promise is fulfilled or rejected, its state is **immutable**.

```js
const promise = new Promise((resolve, reject) => {
  // async work
});
```

---

## 2. Why Promises Exist (Engineering Motivation)

Promises were introduced to solve fundamental problems with callbacks:

1. Callback Hell (deep nesting)
2. Fragmented error handling
3. Inversion of control complexity
4. No standardized async composition
5. Difficulty reasoning about async flows

Promises provide:
- Linear async composition
- Centralized error handling
- Predictable execution semantics
- Standard async abstraction

Promises **do not replace async** — they **structure it**.

---

## 3. Core Promise Mechanics

### 3.1 Creation

```js
const p = new Promise((resolve, reject) => {
  if (success) resolve(value);
  else reject(error);
});
```

### 3.2 Consumption

```js
p.then(value => {
  // fulfilled
}).catch(err => {
  // rejected
});
```

### 3.3 Chaining

Each `.then()`:
- Returns a **new Promise**
- Can transform values
- Can return another Promise

```js
fetchUser()
  .then(user => fetchProfile(user.id))
  .then(profile => console.log(profile))
  .catch(handleError);
```

---

## 4. Promise States & Resolution Rules (Critical)

### 4.1 Resolution Is Not the Same as Fulfillment

```js
resolve(anotherPromise);
```

- The outer Promise **adopts** the inner Promise’s state
- This is called **Promise resolution**

### 4.2 Only the First Call Wins

```js
resolve(1);
resolve(2); // ignored
reject(err); // ignored
```

Promise state can change **once only**.

---

## 5. Microtask Semantics (Very Important)

Promise callbacks (`then`, `catch`, `finally`) are always scheduled as **microtasks**.

```js
console.log("start");

Promise.resolve().then(() => console.log("promise"));

setTimeout(() => console.log("timeout"), 0);

console.log("end");
```

Output:
```
start
end
promise
timeout
```

This guarantees:
- Predictable ordering
- Promises run before macrotasks
- Avoids starvation issues common in callbacks

---

## 6. Good Example — Sequential Async Logic

```js
readFile("a.txt")
  .then(data => parse(data))
  .then(result => save(result))
  .catch(err => handle(err));
```

✅ Linear
✅ Single error exit
✅ No nesting

---

## 7. Bad Example — Swallowing Errors

```js
doWork()
  .then(() => risky())
  .catch(() => {}) // ❌ swallowed error
  .then(() => continue());
```

Problems:
- Errors disappear silently
- Debugging becomes impossible
- False success state

Correct:

```js
.catch(err => {
  log(err);
  throw err;
});
```

---

## 8. Promise Utilities

### Promise.all

Waits for all promises or fails fast.

```js
Promise.all([p1, p2, p3]);
```

- Rejects if any fails
- Order preserved

### Promise.allSettled

Waits for all regardless of outcome.

### Promise.race

First promise to settle wins.

### Promise.any

First fulfilled promise wins.

---

## 9. Promise vs Callback vs async/await

| Aspect | Callback | Promise | async/await |
|------|--------|--------|-------------|
| Readability | Poor when nested | Good | Best |
| Error handling | Manual | Chain-based | try/catch |
| Composability | Weak | Strong | Strong |
| Debugging | Hard | Medium | Easiest |
| Foundation | Base | Abstraction | Syntax sugar |

Promises are the **foundation**; async/await is syntax on top.

---

## 10. Common Pitfalls

### ❌ Forgetting to return a Promise

```js
.then(() => {
  fetchData(); // ❌ not returned
});
```

Breaks chain.

### ❌ Mixing callbacks and promises

### ❌ Creating Promises unnecessarily

```js
new Promise(resolve => resolve(value)); // anti-pattern
```

Prefer `Promise.resolve`.

---

## 11. Relationship to Other Concepts

| Concept | Relationship |
|-------|--------------|
| Callback Hell | Promises solve it |
| async/await | Built on Promises |
| Event Loop | Promises use microtasks |
| Error Propagation | Promise chains |
| Functional composition | Promises compose operations |

---

## 12. Interview Questions

### Beginner
- What is a Promise?
- Promise states?

### Intermediate
- Explain promise chaining
- Promise.all vs allSettled

### Senior
- Explain promise resolution vs fulfillment
- Why promises use microtasks

### Expert
- Implement Promise.all
- Explain how async/await compiles to promises

---

## 13. High-Score Interview Answer Template

> “A Promise represents the eventual result of an asynchronous operation and provides a standardized way to compose async logic.  
Promises solve callback hell by flattening control flow and centralizing error handling.  
They are immutable once settled and schedule callbacks as microtasks, which guarantees predictable execution order.  
Async/await is syntax sugar on top of Promises.”

---

## 14. Summary

- Promise = async result container
- States: pending → fulfilled / rejected
- Chainable and composable
- Errors propagate naturally
- Foundation of async/await
- Executes in microtask queue
