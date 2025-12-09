# Hoisting

## What Hoisting Is

Hoisting is JavaScript's behavior of moving variable and function declarations to the top of their scope during the compilation phase.
Only declarations are hoisted, not initializations.

This affects how and when identifiers can be accessed.

---

## How JavaScript Executes Code

JavaScript execution has two phases:
1. Creation phase (compilation)
2. Execution phase

During the creation phase:
- Memory is allocated for variables and functions
- Declarations are registered in scope

---

## Function Hoisting

Function declarations are fully hoisted.

```js
sayHello();

function sayHello() {
  console.log("Hello");
}
```

This works because the entire function definition is hoisted.

---

## Variable Hoisting with var

Variables declared with var are hoisted but initialized to undefined.

```js
console.log(x); // undefined
var x = 5;
```

Equivalent to:

```js
var x;
console.log(x);
x = 5;
```

---

## let and const Hoisting

let and const are hoisted but not initialized.
They exist in the Temporal Dead Zone until the declaration is evaluated.

```js
console.log(a); // ReferenceError
let a = 10;
```

This prevents access before declaration.

---

## Temporal Dead Zone

The Temporal Dead Zone is the period between:
- Entering scope
- Variable declaration

During this time, accessing the variable throws an error.

This enforces safer coding patterns.

---

## Function Expressions and Hoisting

Function expressions are not hoisted like declarations.

```js
sayHi(); // TypeError
var sayHi = function () {
  console.log("Hi");
};
```

The variable is hoisted, but the function assignment is not.

---

## Arrow Functions and Hoisting

Arrow functions behave like function expressions.

```js
test(); // ReferenceError
const test = () => {};
```

---

## Class Hoisting

Classes are hoisted but also subject to the Temporal Dead Zone.

```js
const obj = new MyClass(); // ReferenceError
class MyClass {}
```

---

## Common Misconceptions

- Hoisting moves code physically (it does not)
- let and const are not hoisted (they are, but inaccessible)
- Hoisting is only about variables

---

## Best Practices

- Declare variables at the top of scope
- Prefer let and const
- Avoid relying on hoisting behavior
- Use function declarations when order matters

---

## Interview Questions

- What is hoisting?
- Why does var behave differently from let?
- What is the Temporal Dead Zone?
- Are functions hoisted?
- Are arrow functions hoisted?
- How are classes hoisted?

---

## Key Takeaways

- Declarations are hoisted, not assignments
- var is initialized to undefined
- let and const are inaccessible before declaration
- Function declarations are fully hoisted
- Understanding hoisting prevents subtle bugs
