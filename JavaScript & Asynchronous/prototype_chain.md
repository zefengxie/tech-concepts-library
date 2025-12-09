# Prototype Chain

## What the Prototype Chain Is

The prototype chain is JavaScript’s core inheritance mechanism.
Objects can access properties and methods that are not defined on themselves
through a linked chain of prototype objects.

Every JavaScript object has an internal [[Prototype]] reference.

---

## Prototype Basics

```js
const obj = {};
Object.getPrototypeOf(obj) === Object.prototype; // true
```

If a property is not found on the object itself, JavaScript looks up the prototype chain.

---

## Property Lookup Mechanism

When accessing `obj.prop`:
1. Check `obj`
2. Check `obj.__proto__`
3. Continue up the chain
4. Stop at `null`

```js
const parent = { a: 1 };
const child = Object.create(parent);

child.a; // 1
```

---

## Function Prototypes

Functions have a `prototype` property used when creating objects with `new`.

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function () {
  return "Hi " + this.name;
};

const p = new Person("Alice");
p.sayHi();
```

---

## Constructor and Prototype Relationship

```js
p.__proto__ === Person.prototype; // true
Person.prototype.constructor === Person; // true
```

---

## Prototype Chain with new

`new` does the following:
1. Create a new object
2. Link it to the constructor’s prototype
3. Bind `this`
4. Return the object

---

## Object.create

```js
const base = { type: "base" };
const obj = Object.create(base);
```

Creates an object with explicit prototype linkage.

---

## Prototype Chain Termination

```js
Object.getPrototypeOf(Object.prototype); // null
```

The chain ends at `null`.

---

## Method Overriding

```js
const parent = { greet() { return "hello"; } };
const child = Object.create(parent);

child.greet = function () {
  return "hi";
};
```

Own properties override prototype properties.

---

## Performance Implications

- Long prototype chains increase lookup cost
- Frequent prototype mutation deoptimizes engines

---

## Common Mistakes

- Confusing `prototype` with `__proto__`
- Mutating built-in prototypes
- Assuming class syntax removes prototype behavior

---

## Prototype Chain with Classes

```js
class Animal {
  speak() {}
}

class Dog extends Animal {}
```

Classes are syntactic sugar over prototypes.

---

## Interview Questions

- What is the prototype chain?
- Difference between `prototype` and `__proto__`?
- How does `new` work internally?
- How does inheritance work in JavaScript?
- Does JavaScript have classical inheritance?

---

## Key Takeaways

- JavaScript uses prototype-based inheritance
- Objects delegate property access
- `class` is syntax sugar
- Prototype chain ends at null
- Understanding prototypes is essential
