# useMemo Hook

## 1. What Is `useMemo`?

`useMemo` is a React Hook that **memoizes a computed value**.

```tsx
const memoizedValue = useMemo(() => computeExpensiveValue(dep1, dep2), [dep1, dep2]);
```

It:
- Runs the function on the first render
- Caches the result
- Recomputes only when its dependencies change

---

## 2. Why `useMemo` Exists

Without `useMemo`, any expensive calculation inside a component would run on **every render**, even if its inputs didn’t change.

`useMemo` helps when:
- A calculation is **expensive** (e.g. large loops, heavy transforms)
- The result is passed to **child components** that rely on referential equality
- You want to avoid unnecessary work for performance reasons

---

## 3. Basic Usage

```tsx
function ProductList({ products, filter }) {
  const visibleProducts = useMemo(() => {
    console.log("Filtering products...");
    return products.filter(p => p.category === filter);
  }, [products, filter]);

  return (
    <ul>
      {visibleProducts.map(p => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

- `visibleProducts` is recomputed only when `products` or `filter` changes.

---

## 4. When to Use `useMemo`

Use `useMemo` when:

- A computation is **expensive** and runs often
- A derived value is passed as a **prop** to a memoized child (`React.memo`)
- You want to avoid recomputing values that rarely change

Do **not** use `useMemo` for:

- Simple arithmetic
- Readable but cheap computations
- Premature optimization

---

## 5. Good Example

```tsx
function SearchResults({ query, items }) {
  const filtered = useMemo(() => {
    // expensive: imagine 10k items
    return items.filter(item =>
      item.name.toLowerCase().includes(query.toLowerCase())
    );
  }, [items, query]);

  return (
    <ul>
      {filtered.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

✔ Why it's good:
- Filtering large lists can be expensive
- Only recomputes when inputs change
- Keeps render fast when unrelated state changes

---

## 6. Bad Example (Overusing `useMemo`)

```tsx
const doubled = useMemo(() => value * 2, [value]); // ❌ unnecessary
```

This adds complexity without any real performance gain.

Better:

```tsx
const doubled = value * 2; // ✔ simple & clear
```

---

## 7. Dependency Array

`useMemo` depends on **stable dependencies**:

```tsx
const memoizedValue = useMemo(() => {
  return compute(a, b);
}, [a, b]);
```

If dependencies are **objects or arrays** that change reference on every render, `useMemo` will still recompute.

To fix that:
- Stabilize dependencies with `useMemo` / `useCallback` in the parent
- Or avoid passing freshly-created objects/arrays as dependencies unless needed

---

## 8. Relation to `useCallback`

- `useMemo` memoizes **values**
- `useCallback` memoizes **functions**

Equivalent patterns:

```tsx
const memoizedValue = useMemo(() => compute(), [deps]);
const memoizedFn = useCallback(() => doSomething(), [deps]);
```

---

## 9. Common Mistakes

### ❌ Mistake 1: Using `useMemo` everywhere  
Adds noise and cognitive load without benefits.

### ❌ Mistake 2: Wrong dependencies  
Leaving dependencies out can cause stale values:

```tsx
const x = useMemo(() => compute(a, b), [a]); // ❌ missing b
```

### ❌ Mistake 3: Using `useMemo` to “fix” logic bugs  
`useMemo` is a performance tool, not a state management tool.

---

## 10. Best Practices

- Use `useMemo` **only for performance**  
- Measure first (DevTools, profiling)  
- Keep dependency lists correct and complete  
- Avoid micro-optimizing trivial code  
- Think of `useMemo` as a **hint** to React, not a guarantee

---

## 11. Interview Questions

1. What is `useMemo` used for?  
2. How does `useMemo` differ from `useCallback`?  
3. When should you use `useMemo` and when should you avoid it?  
4. What happens if you pass an empty dependency array to `useMemo`?  
5. What issues can arise from incorrect dependencies?  
6. Does `useMemo` guarantee that the function will not re-run?  

---

## 12. Quick Summary

- `useMemo` memoizes **computed values**  
- Use it for expensive derived data or props for memoized children  
- Avoid overusing it; it’s for optimization, not correctness  
- Always keep dependency arrays accurate  
