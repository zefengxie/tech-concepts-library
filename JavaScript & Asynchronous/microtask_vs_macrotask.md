# Microtask vs Macrotask

## Why This Distinction Matters

Understanding microtasks and macrotasks is essential for:
- Predicting execution order
- Avoiding race conditions
- Preventing UI freezes
- Writing correct async code with Promises and timers

They define *when* asynchronous callbacks run relative to each other.

---

## Definitions

### Macrotask
Macrotasks represent larger, coarse-grained tasks scheduled by the runtime.

Common macrotask sources:
- setTimeout
- setInterval
- setImmediate (Node.js)
- I/O events
- UI events (click, scroll)
- MessageChannel

Each macrotask runs **one at a time**.

---

### Microtask
Microtasks represent high-priority, fine-grained tasks that must complete before rendering or the next macrotask.

Common microtask sources:
- Promise.then / catch / finally
- async/await continuation
- queueMicrotask
- MutationObserver

Microtasks are executed **immediately after the current call stack empties**.

---

## Event Loop Priority Rules

Execution order per loop iteration:

1. Run all synchronous code
2. Flush **entire microtask queue**
3. Perform rendering (browser)
4. Run **one macrotask**
5. Repeat

Key rule:
**All microtasks run before any macrotask.**

---

## Simple Execution Order Example

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

Output:
```
A
D
C
B
```

Explanation:
- A and D run synchronously
- Promise callback goes to microtask queue
- setTimeout goes to macrotask queue
- Microtasks run before macrotasks

---

## Multiple Microtasks

```js
Promise.resolve()
  .then(() => console.log(1))
  .then(() => console.log(2))
  .then(() => console.log(3));
```

Output:
```
1
2
3
```

All chained .then callbacks are microtasks and execute in order before any macrotask.

---

## async/await and Microtasks

```js
async function example() {
  console.log("X");
  await null;
  console.log("Y");
}

example();
console.log("Z");
```

Output:
```
X
Z
Y
```

await schedules the continuation as a microtask.

---

## Macrotask Delay Example

```js
setTimeout(() => console.log("timeout 1"), 0);
setTimeout(() => console.log("timeout 2"), 0);
```

Only one macrotask executes per event loop turn, maintaining order.

---

## Starvation Scenario

Microtasks can starve macrotasks:

```js
function loop() {
  Promise.resolve().then(loop);
}
loop();
```

Effects:
- Timers never fire
- Rendering is blocked
- UI freezes

---

## Browser Rendering Interaction

In browsers:
- Rendering happens **after** microtasks
- Long microtask queues delay paint
- Heavy Promise chains can cause frame drops

---

## Node.js Differences

Node.js has additional behavior:
- process.nextTick runs before Promise microtasks
- setImmediate executes in a separate phase
- Event loop has multiple phases (timers, poll, check)

But the microtask > macrotask priority rule still applies.

---

## Common Mistakes

- Assuming setTimeout(fn, 0) runs immediately
- Forgetting that await queues a microtask
- Blocking rendering with long Promise chains
- Mixing macrotasks and microtasks without understanding order

---

## When to Use Each

### Use Microtasks for:
- Promise resolution logic
- Chaining async operations
- Ensuring immediate continuation

### Use Macrotasks for:
- Scheduling future work
- Yielding control back to the browser
- Deferring non-urgent work

---

## Interview Questions

- What is the difference between microtasks and macrotasks?
- Why do Promises run before setTimeout?
- Where does async/await fit?
- How can microtasks cause starvation?
- When should you prefer macrotasks?
- Does rendering happen before or after microtasks?

---

## Key Takeaways

- Microtasks have higher priority than macrotasks
- All microtasks must finish before the next macrotask
- await schedules a microtask
- Excessive microtasks can block rendering
- Understanding queues is critical for async correctness
