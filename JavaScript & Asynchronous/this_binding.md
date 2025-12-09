# This Binding

## What `this` Represents

`this` is a runtime-bound reference that points to the execution context of a function.
Its value is determined by *how a function is called*, not where it is defined.

Understanding `this` is critical for object-oriented patterns, event handling, and class-based code.

---

## The Four Binding Rules

### 1. Default Binding

In non-strict mode, `this` refers to the global object.
In strict mode, `this` is `undefined`.

```js
function foo() {
  console.log(this);
}
foo();
```

---

### 2. Implicit Binding

When a function is called as a method, `this` refers to the object before the dot.

```js
const user = {
  name: "Alice",
  greet() {
    console.log(this.name);
  }
};

user.greet(); // Alice
```

---

### 3. Explicit Binding

`call`, `apply`, and `bind` explicitly set `this`.

```js
function greet() {
  console.log(this.name);
}

const user = { name: "Bob" };
greet.call(user);
greet.apply(user);
const bound = greet.bind(user);
bound();
```

---

### 4. new Binding

When a function is called with `new`, `this` refers to the newly created object.

```js
function Person(name) {
  this.name = name;
}

const p = new Person("Eve");
```

---

## Arrow Functions

Arrow functions do not have their own `this`.
They inherit `this` from the surrounding lexical scope.

```js
const obj = {
  value: 10,
  method() {
    setTimeout(() => {
      console.log(this.value);
    }, 0);
  }
};
```

This avoids common callback binding issues.

---

## this in Classes

Inside a class, `this` refers to the instance.

```js
class Counter {
  constructor() {
    this.count = 0;
  }

  increment() {
    this.count++;
  }
}
```

Methods lose `this` when detached.

```js
const inc = counter.increment;
inc(); // undefined in strict mode
```

---

## this in Event Handlers

### Traditional Functions
`this` refers to the DOM element.

### Arrow Functions
`this` refers to the outer lexical context.

---

## Common Pitfalls

- Losing `this` when passing methods as callbacks
- Using arrow functions as object methods incorrectly
- Assuming `this` is determined lexically
- Forgetting strict mode behavior

---

## Best Practices

- Prefer arrow functions for callbacks
- Use bind when passing methods
- Avoid relying on default binding
- Understand call-site behavior

---

## Interview Questions

- What determines the value of `this`?
- Difference between arrow functions and regular functions?
- How does `bind` work?
- What happens to `this` when a method is passed as a callback?
- How does `this` behave in strict mode?

---

## Key Takeaways

- `this` is determined at call time
- Four rules define `this` binding
- Arrow functions inherit `this`
- Classes rely on correct binding
- Misunderstanding `this` causes many bugs
