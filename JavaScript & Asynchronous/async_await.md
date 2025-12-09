# Async / Await

## What Problem async/await Solves

Before async/await, asynchronous JavaScript relied on callbacks and Promise chains.
async/await is syntactic sugar built on top of Promises that allows asynchronous code to be written in a synchronous style.

async/await improves:
- Readability
- Error handling
- Control flow
- Debugging experience

It does not make code synchronous or block the event loop.

---

## Core Definitions

### async Function
- Declares a function as asynchronous
- Always returns a Promise

```js
async function foo() {
  return 42;
}
```

### await Expression
- Can only be used inside an async function (except top-level await)
- Pauses execution of the async function until the Promise settles

```js
async function getUser() {
  const res = await fetch("/api/user");
  const data = await res.json();
  return data;
}
```

---

## How async/await Works Internally

1. The async function executes synchronously until the first await
2. Execution pauses and returns control to the event loop
3. The remaining function body is scheduled as a microtask
4. When the awaited Promise resolves, execution resumes

await pauses the function, not the JavaScript thread.

---

## Sequential vs Parallel Execution

### Sequential

```js
async function load() {
  const a = await fetchA();
  const b = await fetchB();
  const c = await fetchC();
}
```

### Parallel

```js
async function load() {
  const [a, b, c] = await Promise.all([
    fetchA(),
    fetchB(),
    fetchC(),
  ]);
}
```

---

## Error Handling

```js
async function loadData() {
  try {
    const res = await fetch("/api/data");
    if (!res.ok) throw new Error(res.status);
    return await res.json();
  } catch (err) {
    throw err;
  }
}
```

Throwing inside an async function creates a rejected Promise.

---

## async/await in Loops

### Incorrect

```js
items.forEach(async item => {
  await save(item);
});
```

### Sequential

```js
for (const item of items) {
  await save(item);
}
```

### Parallel

```js
await Promise.all(items.map(save));
```

---

## Top-Level await

```js
const config = await loadConfig();
startApp(config);
```

Works only in ES modules.

---

## Common Pitfalls

- Forgetting await
- Unintended sequential awaits
- Mixing await and then
- Using await in performance-critical loops

---

## Interview Questions

- Does await block the event loop?
- How does async/await use the microtask queue?
- How do you run async operations in parallel?
- Why does async + forEach fail?
- What happens when an async function throws?

---

## Key Takeaways

- async functions always return Promises
- await pauses only the async function
- async/await relies on Promises
- Use Promise.all for concurrency
