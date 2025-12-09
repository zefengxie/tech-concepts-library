# Scope

## What Scope Defines

Scope determines where variables, functions, and objects are accessible in a program.
It is the set of rules that governs how identifiers are resolved at runtime.

Understanding scope is essential for:
- Predicting variable access
- Avoiding name collisions
- Writing maintainable and secure code

---

## Types of Scope in JavaScript

### Global Scope
Variables declared outside any function or block.

```js
const appName = "MyApp";
```

Accessible from anywhere in the program.

Risks:
- Name collisions
- Harder debugging
- Hidden dependencies

---

### Function Scope
Variables declared inside a function are only accessible within that function.

```js
function foo() {
  const x = 10;
  console.log(x);
}
```

---

### Block Scope
Introduced with `let` and `const`.
Variables are limited to `{}` blocks.

```js
if (true) {
  let count = 1;
}
```

`count` is not accessible outside the block.

---

## Lexical Scope

JavaScript uses lexical (static) scoping:
- Scope is determined by **where variables are written**
- Not by where functions are called

```js
const x = 5;

function outer() {
  const x = 10;
  function inner() {
    console.log(x);
  }
  inner();
}

outer(); // 10
```

---

## Scope Chain

When resolving an identifier:
1. Check current scope
2. Check parent scope
3. Continue up to global scope
4. Throw ReferenceError if not found

This lookup path is the scope chain.

---

## var vs let vs const

### var
- Function-scoped
- Hoisted
- Can be redeclared
- Causes unexpected behavior in loops

### let
- Block-scoped
- Hoisted but in temporal dead zone
- Cannot be redeclared in same scope

### const
- Block-scoped
- Must be initialized
- Binding is immutable (object contents can change)

---

## Temporal Dead Zone (TDZ)

Accessing `let` or `const` before declaration causes an error.

```js
console.log(x); // ReferenceError
let x = 5;
```

This prevents unsafe usage.

---

## Scope and Closures

Closures capture variables from surrounding scopes.

```js
function makeAdder(x) {
  return function (y) {
    return x + y;
  };
}
```

The inner function retains access to `x`.

---

## Shadowing

A variable in an inner scope can shadow an outer variable.

```js
let value = 1;
function test() {
  let value = 2;
}
```

Shadowing can reduce clarity if overused.

---

## Common Mistakes

- Using var in loops
- Overusing global variables
- Shadowing unintentionally
- Accessing variables in TDZ

---

## Interview Questions

- What is scope?
- Difference between function scope and block scope?
- What is lexical scope?
- How does the scope chain work?
- What is the temporal dead zone?
- How do closures relate to scope?

---

## Key Takeaways

- Scope controls variable visibility
- JavaScript uses lexical scoping
- let and const introduce block scope
- var is function-scoped
- Scope chain resolves identifiers
