# Event Loop

## What the Event Loop Solves

JavaScript runs on a single thread, meaning it can execute only one piece of code at a time.
The Event Loop enables JavaScript to handle asynchronous operations without blocking the main thread,
allowing responsive user interfaces and scalable I/O.

---

## Core Components

### Call Stack
The call stack is where synchronous code is executed.
Functions are pushed onto the stack when called and popped off when they return.

If the call stack is blocked, nothing else can execute.

---

### Web APIs
Web APIs are provided by the browser (or Node.js runtime) to handle asynchronous work such as:
- Timers (setTimeout, setInterval)
- Network requests (fetch)
- DOM events

These operations run outside the JavaScript engine.

---

### Task Queues

#### Macrotask Queue
Contains tasks such as:
- setTimeout
- setInterval
- setImmediate (Node.js)
- UI rendering events

#### Microtask Queue
Contains:
- Promise.then / catch / finally
- async/await continuations
- queueMicrotask

Microtasks have higher priority than macrotasks.

---

## How the Event Loop Works

1. Execute all synchronous code in the call stack
2. Check the microtask queue
3. Execute all microtasks until empty
4. Perform rendering if needed
5. Take one macrotask from the queue
6. Repeat the process

Key rule:
Microtasks always run before the next macrotask.

---

## Execution Order Example

```js
console.log("start");

setTimeout(() => {
  console.log("timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("promise");
});

console.log("end");
```

Output:
```
start
end
promise
timeout
```

Explanation:
- Synchronous code runs first
- Promise callback goes to microtask queue
- setTimeout goes to macrotask queue
- Microtasks are executed before macrotasks

---

## async/await and the Event Loop

```js
async function test() {
  console.log(1);
  await Promise.resolve();
  console.log(2);
}

test();
console.log(3);
```

Output:
```
1
3
2
```

Explanation:
- Code before await runs synchronously
- Code after await is queued as a microtask

---

## Starvation Risk

If microtasks keep scheduling more microtasks, macrotasks may be delayed indefinitely.

```js
function loop() {
  Promise.resolve().then(loop);
}

loop();
```

This blocks rendering and timers.

---

## Browser vs Node.js Event Loop

### Browser
- Rendering happens between macrotasks
- Microtasks always flush before rendering

### Node.js
- Event loop has multiple phases (timers, I/O, idle, poll, check)
- process.nextTick runs before microtasks
- setImmediate differs from setTimeout

---

## Common Misconceptions

- The Event Loop is not a thread
- async/await does not create new threads
- JavaScript does not run tasks in parallel on the main thread
- Promises do not execute immediately

---

## Debugging Tips

- Use console logs with ordering tests
- Avoid long synchronous loops
- Prefer async APIs for heavy work
- Move CPU-heavy tasks to Web Workers

---

## Interview Questions

- What is the Event Loop?
- Why is JavaScript single-threaded?
- Difference between microtasks and macrotasks?
- Where does async/await fit in the Event Loop?
- Why does Promise resolve before setTimeout?
- How can the Event Loop be blocked?
- What is task starvation?

---

## Key Takeaways

- The Event Loop enables non-blocking JavaScript
- Synchronous code blocks everything
- Microtasks have higher priority than macrotasks
- Rendering happens between macrotasks
- Understanding execution order is critical for debugging and performance
