# useEffect Hook

## 1. What Is `useEffect`?

`useEffect` is a React Hook that allows components to perform **side effects**, such as:
- Fetching data  
- Working with browser APIs (DOM, localStorage, events)  
- Managing subscriptions  
- Running timers  
- Synchronizing state with external systems  

Side effects are operations that **cannot happen during render**.

---

## 2. Why useEffect Exists

React's render phase must stay **pure**, meaning:
- No side effects  
- No mutations  
- No asynchronous operations  

`useEffect` moves these operations into a separate lifecycle cycle.

---

## 3. Basic Syntax

```tsx
useEffect(() => {
  // side effect here
});
```

Runs after **every render** (mount + update).

---

## 4. Dependency Array (Critical)

### 4.1 Run Once (on Mount)

```tsx
useEffect(() => {
  console.log("mounted");
}, []);
```

### 4.2 Run When Dependencies Change

```tsx
useEffect(() => {
  console.log("value changed");
}, [value]);
```

### 4.3 Run on Every Render (rarely used)

```tsx
useEffect(() => {
  console.log("runs every time");
});
```

---

## 5. Cleanup Function

Used for:
- removing event listeners  
- clearing timers  
- unsubscribing from streams  
- canceling async requests  

```tsx
useEffect(() => {
  const handler = () => console.log("resize");
  window.addEventListener("resize", handler);

  return () => {
    window.removeEventListener("resize", handler);
  };
}, []);
```

Cleanup runs before unmount or before the next effect.

---

## 6. Fetching Data with useEffect

```tsx
useEffect(() => {
  let cancelled = false;

  async function load() {
    const res = await fetch("/api/data");
    if (!cancelled) setData(await res.json());
  }

  load();
  return () => { cancelled = true; };
}, []);
```

Prevents setting state after unmount.

---

## 7. Common Mistakes

### ❌ Missing dependencies

```tsx
useEffect(() => {
  console.log(value);
}, []); // ❌ value should be a dependency
```

### ❌ Infinite loops

```tsx
useEffect(() => {
  setCount(count + 1); // ❌ updating state inside effect
}, [count]);
```

### ❌ Doing too much in one effect  
Better to split effects based on purpose.

---

## 8. When NOT to Use useEffect

- For computing derived values (useMemo instead)  
- For focusing input (useRef sometimes better)  
- For updating state unnecessarily  

---

## 9. Best Practices

- Group related logic, split unrelated effects  
- Include all dependencies (React guarantees safety)  
- Use cleanup for subscriptions and timers  
- Use abort signals for async fetches  

---

## 10. Interview Questions

1. What does useEffect do?  
2. When does useEffect run?  
3. What is the dependency array?  
4. How does cleanup work?  
5. How to prevent infinite loops?  
6. Why is useEffect not the same as lifecycle methods?  
7. When should you avoid useEffect?  

---

## 11. Quick Summary

- `useEffect` handles side effects  
- Dependency array controls when it runs  
- Cleanup runs before re-runs and unmount  
- Avoid missing dependencies  
- Avoid infinite loops  
