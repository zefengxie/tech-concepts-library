# useCallback Hook

## 1. What Is `useCallback`?

`useCallback` is a React Hook that **memoizes a function**.

```tsx
const memoizedFn = useCallback(() => {
  doSomething();
}, [deps]);
```

It returns the **same function reference** between renders **as long as its dependencies don’t change**.

---

## 2. Why `useCallback` Exists

In React, when a component re-renders:

- All functions defined inside the component are **re-created**
- If these functions are passed to child components, they get **new references** every time

This breaks referential equality and can cause:

- Unnecessary re-renders in child components (even if their props don’t logically change)
- Performance issues in large component trees

`useCallback` helps when:

- You pass callbacks to `React.memo`-wrapped child components
- You depend on stable function identity (e.g. for dependencies of `useEffect` or `useMemo`)

---

## 3. Basic Usage

```tsx
function SearchBox({ onSearch }: { onSearch: (q: string) => void }) {
  const [query, setQuery] = useState("");

  const handleChange = useCallback(
    (e: React.ChangeEvent<HTMLInputElement>) => {
      const value = e.target.value;
      setQuery(value);
      onSearch(value);
    },
    [onSearch] // dependencies
  );

  return <input value={query} onChange={handleChange} />;
}
```

Here:

- `handleChange` keeps the **same reference** unless `onSearch` changes.

---

## 4. When To Use `useCallback`

### Good use cases:

- You pass a callback down to a **memoized child**:

```tsx
const handleSelect = useCallback((id: string) => {
  setSelectedId(id);
}, []);

return <List items={items} onSelect={handleSelect} />;
```

- You want to avoid re-running effects that depend on a function:

```tsx
const fetchData = useCallback(async () => {
  // ...
}, [id]);

useEffect(() => {
  fetchData();
}, [fetchData]);
```

### Not necessary when:

- The child is **not** memoized  
- The callback is not passed down deeply  
- The component tree is small and performance is fine  

---

## 5. `useCallback` vs `useMemo`

They are conceptually similar:

- `useMemo(fn, deps)` → memoizes the **result of fn**  
- `useCallback(fn, deps)` → memoizes the **fn itself**  

These two are equivalent:

```tsx
const memoFn1 = useCallback(fn, deps);
const memoFn2 = useMemo(() => fn, deps);
```

But `useCallback` is semantically clearer for functions.

---

## 6. Good Example

```tsx
const Item = React.memo(function Item({
  item,
  onToggle,
}: {
  item: { id: string; done: boolean; label: string };
  onToggle: (id: string) => void;
}) {
  console.log("render:", item.label);
  return (
    <li onClick={() => onToggle(item.id)}>
      {item.done ? "✅ " : "⬜️ "}
      {item.label}
    </li>
  );
});

function TodoList({ items }: { items: { id: string; done: boolean; label: string }[] }) {
  const [selectedId, setSelectedId] = useState<string | null>(null);

  const handleToggle = useCallback((id: string) => {
    setSelectedId(id);
  }, []);

  return (
    <ul>
      {items.map(item => (
        <Item key={item.id} item={item} onToggle={handleToggle} />
      ))}
      <p>Selected: {selectedId}</p>
    </ul>
  );
}
```

✔ Why it’s good:

- `Item` uses `React.memo`  
- `onToggle` maintains stable identity via `useCallback`  
- `Item` only re-renders when its **props actually change**

---

## 7. Bad Example (Overusing `useCallback`)

```tsx
function Counter() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => setCount(c => c + 1), []);
  const decrement = useCallback(() => setCount(c => c - 1), []);
  const reset = useCallback(() => setCount(0), []);

  return (
    <>
      <button onClick={decrement}>-</button>
      <span>{count}</span>
      <button onClick={increment}>+</button>
      <button onClick={reset}>Reset</button>
    </>
  );
}
```

Even though this works, `useCallback` brings **no real benefit** here because:

- There are no memoized children relying on these function identities
- Component is simple, re-renders are cheap

Simpler version:

```tsx
function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => setCount(c => c + 1);
  const decrement = () => setCount(c => c - 1);
  const reset = () => setCount(0);

  // ...
}
```

---

## 8. Dependency Array

Like `useEffect` and `useMemo`, `useCallback` takes a dependency array:

```tsx
const fn = useCallback(() => {
  doSomething(id, name);
}, [id, name]);
```

You **must** include all variables from the closure; otherwise:

- The function may use **stale values**
- Behavior can be confusing

Tools like ESLint (`react-hooks/exhaustive-deps`) can help.

---

## 9. Common Mistakes

### ❌ Using `useCallback` for every function
- Adds complexity and overhead
- Makes code harder to read

### ❌ Missing dependencies
```tsx
const fn = useCallback(() => {
  console.log(value);
}, []); // ❌ value should be in dependency list
```

### ❌ Using `useCallback` instead of structuring components correctly  
Performance issues are often better solved by:

- Splitting components  
- Avoiding unnecessary parents re-renders  
- Using memoization at the **right level**

---

## 10. Best Practices

- Use `useCallback` when:
  - A **memoized child** receives a callback prop  
  - A callback is part of a **dependency list**  
- Don’t micro-optimize prematurely  
- Keep dependency arrays complete  
- Combine with `React.memo` and `useMemo` thoughtfully  

---

## 11. Interview Questions

1. What does `useCallback` do?  
2. How is `useCallback` different from `useMemo`?  
3. When should you use `useCallback`?  
4. Why is it not always an optimization?  
5. How do dependencies work in `useCallback`?  
6. How do `React.memo` and `useCallback` work together?  
7. Can `useCallback` fix all performance problems? Why not?  

---

## 12. Quick Summary

- `useCallback` memoizes a **function**  
- Useful when passing callbacks to memoized children or as dependencies  
- Should be used **selectively** for performance, not everywhere  
- Always use correct dependency arrays  
