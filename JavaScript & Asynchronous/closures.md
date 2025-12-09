# Closures

## What Problem Closures Solve

Closures allow functions to retain access to variables from their lexical scope even after the outer function has finished execution.
They are fundamental to JavaScript’s function-based design and enable data encapsulation, state preservation, and functional patterns.

---

## Formal Definition

A closure is created when a function is defined inside another function and retains access to:
- Its own scope
- The outer function’s scope
- The global scope

This access persists even after the outer function has returned.

---

## Basic Example

```js
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();
counter(); // 1
counter(); // 2
```

The inner function forms a closure over `count`.

---

## How Closures Work Internally

- JavaScript uses lexical scoping
- Functions capture variable references, not values
- The garbage collector preserves captured variables as long as the closure exists

---

## Closures and Lexical Scope

```js
function outer() {
  const x = 10;
  function inner() {
    console.log(x);
  }
  return inner;
}

const fn = outer();
fn(); // 10
```

`inner` remembers the scope in which it was defined, not where it is executed.

---

## Practical Use Cases

### Data Encapsulation

```js
function createUser(name) {
  let loggedIn = false;

  return {
    login() {
      loggedIn = true;
    },
    isLoggedIn() {
      return loggedIn;
    }
  };
}
```

### Function Factories

```js
function multiplyBy(x) {
  return function (y) {
    return x * y;
  };
}

const double = multiplyBy(2);
double(5); // 10
```

### Event Handlers

```js
function setup(button) {
  let clicks = 0;

  button.addEventListener("click", () => {
    clicks++;
    console.log(clicks);
  });
}
```

---

## Common Pitfall: Loop Closures

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

Output:
```
3
3
3
```

Fix with block scope:

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

---

## Memory Implications

Closures can cause memory leaks if they:
- Capture large objects
- Remain referenced unnecessarily

Example risk:

```js
function handler() {
  const largeData = new Array(1e6);
  return () => console.log(largeData.length);
}
```

---

## When Closures Are Not Ideal

- Performance-critical inner loops
- Long-lived closures capturing unnecessary state
- Codebases where simpler state structures suffice

---

## Common Misconceptions

- Closures copy values (they capture references)
- Closures are slow by default
- Closures are unique to JavaScript

---

## Interview Questions

- What is a closure?
- How does lexical scoping enable closures?
- Why does `var` behave differently in loops?
- How do closures affect memory?
- Give real-world use cases for closures

---

## Key Takeaways

- Closures preserve lexical scope
- They enable encapsulation and functional patterns
- Variables are captured by reference
- They must be used carefully to avoid memory leaks
