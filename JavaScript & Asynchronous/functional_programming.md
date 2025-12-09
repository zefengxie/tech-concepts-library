# Functional Programming

## What Functional Programming Is

Functional Programming (FP) is a programming paradigm that treats computation as the evaluation of mathematical functions
and avoids changing state and mutable data. It emphasizes declarative code, predictability, and composition.

---

## Core Principles

### Pure Functions
A pure function:
- Produces the same output for the same input
- Has no side effects

```js
function add(a, b) {
  return a + b;
}
```

Impure example:

```js
let total = 0;
function addToTotal(x) {
  total += x; // side effect
}
```

---

### Immutability
Data is not modified after creation. Instead, new data is produced.

```js
const arr = [1, 2, 3];
const newArr = [...arr, 4];
```

Avoid:

```js
arr.push(4);
```

---

### First-Class Functions
Functions are values that can be:
- Assigned to variables
- Passed as arguments
- Returned from other functions

```js
function apply(fn, value) {
  return fn(value);
}
```

---

### Higher-Order Functions
Functions that operate on other functions.

```js
const nums = [1, 2, 3];
const doubled = nums.map(n => n * 2);
```

Common examples:
- map
- filter
- reduce

---

## Declarative vs Imperative

Imperative:

```js
let result = [];
for (let i = 0; i < nums.length; i++) {
  if (nums[i] > 2) result.push(nums[i] * 2);
}
```

Declarative:

```js
const result = nums.filter(n => n > 2).map(n => n * 2);
```

---

## Function Composition

Combining small functions to build more complex ones.

```js
const compose = (f, g) => x => f(g(x));

const double = x => x * 2;
const increment = x => x + 1;

const doubleThenIncrement = compose(increment, double);
```

---

## Avoiding Shared State

Shared mutable state leads to:
- Race conditions
- Hidden bugs
- Unpredictable behavior

FP avoids shared state by passing data explicitly.

---

## FP in JavaScript

JavaScript supports FP through:
- Arrow functions
- Array methods
- Closures
- Immutable patterns

Libraries:
- Ramda
- Lodash/fp
- Redux (inspired by FP)

---

## Benefits

- Easier reasoning
- Improved testability
- Fewer bugs
- Better concurrency handling

---

## Trade-Offs

- More allocations
- Learning curve
- Sometimes less intuitive for state-heavy logic

---

## Common Misconceptions

- FP forbids all mutation
- FP is slow
- FP replaces OOP entirely

---

## Interview Questions

- What is a pure function?
- Why is immutability important?
- What are higher-order functions?
- Explain function composition
- FP vs OOP trade-offs
- How does FP help with concurrency?

---

## Key Takeaways

- FP emphasizes pure functions and immutability
- Functions are first-class citizens
- Declarative style improves readability
- Composition builds complex behavior
- FP improves correctness and maintainability
