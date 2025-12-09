# Currying

## What Currying Is

Currying is a functional programming technique where a function with multiple arguments
is transformed into a sequence of functions, each taking a single argument.

Instead of:
f(a, b, c)

You get:
f(a)(b)(c)

---

## Why Currying Exists

Currying enables:
- Partial application
- Function reuse
- Better composition
- Clear separation of concerns

It is widely used in functional programming and modern JavaScript libraries.

---

## Basic Example

```js
function add(a) {
  return function (b) {
    return a + b;
  };
}

add(2)(3); // 5
```

Equivalent non-curried version:

```js
function add(a, b) {
  return a + b;
}
```

---

## Arrow Function Version

```js
const add = a => b => a + b;
```

---

## Partial Application

Currying allows fixing part of the arguments:

```js
const multiply = a => b => a * b;

const double = multiply(2);
double(5); // 10
```

This creates reusable specialized functions.

---

## Practical Use Case

### Logging Utility

```js
const log = level => message => {
  console.log(`[${level}] ${message}`);
};

const errorLog = log("ERROR");
errorLog("Something failed");
```

---

## Currying vs Partial Application

- Currying transforms a function into unary functions
- Partial application fixes some arguments of a function

JavaScript currying often supports both.

---

## Implementing a Curry Helper

```js
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    }
    return (...next) => curried(...args, ...next);
  };
}
```

Usage:

```js
const sum = (a, b, c) => a + b + c;
const curriedSum = curry(sum);

curriedSum(1)(2)(3);
curriedSum(1, 2)(3);
```

---

## Currying in Libraries

Popular libraries that use currying:
- Ramda
- Lodash/fp
- Redux (functional patterns)

Example from Ramda style:

```js
const filter = predicate => list => list.filter(predicate);
```

---

## Benefits

- Improves composability
- Encourages small, reusable functions
- Makes function pipelines expressive

---

## Drawbacks

- Can reduce readability for newcomers
- Harder debugging stack traces
- Overuse leads to unnecessary complexity

---

## Common Misconceptions

- Currying is the same as partial application
- Currying always improves code
- Currying is only for functional languages

---

## Interview Questions

- What is currying?
- Difference between currying and partial application?
- Why is currying useful?
- How does currying improve composition?
- Implement a curry function

---

## Key Takeaways

- Currying transforms multi-argument functions
- Enables partial application
- Common in functional programming
- Powerful but should be used judiciously
