# Memoization (Ultra Deep Version)

## 1. Formal Definition

**Memoization** is an optimization technique where the results of **pure function calls** are cached so that:

> When the same inputs appear again, the function returns the cached result instead of recomputing.

Formally:

- Let `f: X → Y` be a deterministic, side-effect-free function.
- Memoization stores a mapping `M: X → Y` such that:
  - If `x ∈ domain(M)`, return `M[x]`.
  - Otherwise, compute `f(x)`, store `M[x] = f(x)`, then return it.

Memoization transforms an **expensive function** into a **lookup + occasional compute**.

---

## 2. Engineering Purpose — What Problem Does Memoization Solve?

Memoization addresses three core problems:

### ✔ Computational cost
- Expensive operations (e.g. recursion, heavy math, data transformations) run many times with the same input.
- Memoization reduces repeated work.

### ✔ Performance bottlenecks in UI
- React components or selectors recompute derived data on every render.
- Memoization prevents redundant calculations.

### ✔ Algorithmic complexity
- Many recursive algorithms go from exponential → polynomial with memoization (e.g. Fibonacci, DP, knapsack).

> **Memoization trades memory for speed.**

---

## 3. Internal Mechanism — How Memoization Works

Basic pattern:

```js
function memoize(fn) {
  const cache = new Map();

  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}
```

Steps:

1. Normalize arguments into a **cache key**.
2. If result is cached → return immediately.
3. Otherwise:
   - Call original function.
   - Store result.
   - Return result.

---

## 4. Memoization vs Caching (Important Distinction)

- **Memoization**:  
  - Usually function-scoped.  
  - Key = function arguments.  
  - Transparent to caller (same interface).

- **Caching**:  
  - More general (e.g. HTTP cache, Redis, CDN).  
  - Keys may be URLs, IDs, tokens, etc.  
  - Often cross-process or cross-request.

Memoization is a *special case of caching*, focused specifically on **function calls**.

---

## 5. Good Examples — Where Memoization Shines

### ✔ 5.1 Recursive algorithms (Fibonacci)

Naïve recursive Fibonacci:

```js
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}
```

Time complexity: **O(2ⁿ)**.

Memoized:

```js
function fibMemo() {
  const cache = {};

  function fib(n) {
    if (n in cache) return cache[n];
    if (n <= 1) return n;
    const result = fib(n - 1) + fib(n - 2);
    cache[n] = result;
    return result;
  }

  return fib;
}

const fib = fibMemo();
```

Time complexity: **O(n)**.

---

### ✔ 5.2 Derived data in UI (e.g. selectors)

Example: computing visible todos based on filters.

```js
const selectVisibleTodos = memoize((todos, filter) => {
  // heavy filtering logic
});
```

Use in React:

```jsx
const visibleTodos = selectVisibleTodos(todos, filter);
```

Memoization prevents recalculation when `todos` and `filter` references don’t change.

---

### ✔ 5.3 Expensive formatting / computations

Example:

```js
const formatPrice = memoize((value, locale, currency) =>
  new Intl.NumberFormat(locale, { style: "currency", currency }).format(value)
);
```

`Intl` objects are expensive; memoization saves work.

---

## 6. Bad Examples — Misusing Memoization

### ❌ 6.1 Memoizing functions with side effects

```js
function sendAnalyticsEvent(userId) {
  // network call
}

const memoizedSend = memoize(sendAnalyticsEvent);
```

Problem:

- Later identical calls never send analytics again.
- Violates semantics → memoization is unsafe.

**Rule:** Only memoize **pure, deterministic functions**.

---

### ❌ 6.2 Using memoization on cheap functions

```js
const increment = memoize(x => x + 1);
```

You pay overhead for:

- Map lookup
- JSON.stringify or hashing
- Memory usage

But you save almost nothing → **net slower**.

---

### ❌ 6.3 Unbounded cache growth

Memoizing with:

```js
const cache = {};
// never cleared
```

In long-lived apps:

- Cache grows indefinitely.
- Memory usage spikes.
- GC overhead increases.

Fix: Use LRU or limit size.

---

### ❌ 6.4 Memoizing with unstable keys (React)

```jsx
const result = memoizedCompute(obj);
```

If `obj` is a *new reference every render*:

- Cache miss every time.
- Memoization provides zero benefit.

---

## 7. Best Practices

### ✔ Only memoize **pure**, deterministic functions

- No side effects
- Same input → same output

### ✔ Use appropriate key strategies

- Simple arguments: join or JSON.stringify  
- Complex: custom hashing or referential equality in UI frameworks.

### ✔ Limit cache size

Implement LRU cache:

```js
class LRUCache {
  // ...
}
```

Or use libraries that support max size and TTL.

### ✔ In React / UI frameworks

Use:

- `useMemo` for derived values
- `useCallback` for stable function references
- `memo` / `PureComponent` for component-level memoization
- Reselect in Redux for memoized selectors

### ✔ Measure first

Profile before/after:

- If memoization brings no benefit → remove it.
- Don’t prematurely optimize.

---

## 8. Memoization in React (And Similar Frameworks)

### 8.1 `useMemo`

```jsx
const filteredList = useMemo(
  () => heavyFilter(data, filter),
  [data, filter]
);
```

Runs `heavyFilter` only when dependencies change.

---

### 8.2 `useCallback`

```jsx
const handleClick = useCallback(() => {
  // ...
}, [id]);
```

Memoizes a function reference → avoids re-render cascades in child components.

---

### 8.3 Component memoization

```jsx
const ListItem = React.memo(function ListItem(props) {
  // ...
});
```

Skips re-rendering when props are shallow equal.

---

## 9. Memoization vs Other Techniques

| Concept        | What It Does                                  |
|----------------|-----------------------------------------------|
| **Memoization**| Caches function results                       |
| **Throttling** | Limits frequency of execution                 |
| **Debouncing** | Delays execution until inactivity             |
| **Caching**    | Generic data storage (may cross process)      |
| **Virtualization** | Limits rendered DOM elements             |

Memoization is **not** about reducing how often we call a function, but about **reusing computed results**.

---

## 10. Common Misconceptions & Pitfalls

### ❌ “Memoization always makes code faster”
Not true:

- For cheap functions, the overhead can make it slower.
- Poor cache strategy can cause memory leaks.

---

### ❌ “Memoization is only for recursion”
No:

- It’s critical in UIs for derived state.
- Common in data processing, ML pipelines, and expensive formatting.

---

### ❌ “useMemo is for performance only”
Over-using `useMemo` can make code harder to read with little benefit.

---

### ❌ “Memoization is transparent & safe for any function”
Not safe if function:

- Has side effects
- Depends on external mutable state
- Uses time, random values, or global variables

---

## 11. Interview Questions (Beginner → Senior)

### 🔹 Beginner
1. What is memoization?
2. How is memoization different from caching?

### 🔹 Intermediate
3. Show how to implement a simple memoize function.
4. How does memoization improve recursive algorithms?
5. How is memoization used in React?

### 🔹 Senior
6. When is memoization harmful or unnecessary?
7. How would you design an LRU-based memoization system?
8. How do you memoize functions with multiple complex arguments?
9. How does memoization interact with referential equality in React and Redux?
10. How would you identify candidates for memoization in a large codebase?

---

## 12. Winning Interview Answer Template

> “Memoization caches the result of pure functions so repeated calls with the same inputs return instantly without recomputing.  
I use it both in algorithms (e.g. DP, recursive functions) and in UIs for expensive derived data and selectors.  
In React, I rely on `useMemo`, `useCallback`, and memoized selectors to avoid unnecessary recomputations and re-renders.  
I only memoize deterministic, side-effect-free logic, and I monitor cache size and performance impact.”

---

## 13. One-minute Summary

- Memoization = remember function outputs for given inputs  
- Great for expensive, deterministic functions  
- Critical in recursion (e.g. Fibonacci, DP) and UI derived data  
- Must not be used with side-effectful functions  
- Needs proper key design and cache management  

