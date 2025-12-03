# React Strict Mode

## 1. What Is Strict Mode?

**Strict Mode** in React is a development-only tool that helps you:

- Detect unsafe lifecycle patterns  
- Detect side effects during rendering  
- Highlight potential bugs early  
- Prepare your code for future React features (like concurrent rendering)

It is enabled with:

```tsx
import React from "react";

<React.StrictMode>
  <App />
</React.StrictMode>
```

Strict Mode **does not affect production builds** — it only runs in development.

---

## 2. What Strict Mode Actually Does

When enabled, Strict Mode:

1. **Intentionally double-invokes certain functions in development**, such as:
   - Function components (render function)
   - Class component `constructor`
   - `render` method
   - `useState` initializers
   - `useMemo`/`useCallback` initializers
   - `useEffect` cleanup + setup sequence

2. **Warns about legacy or unsafe APIs**, like:
   - `componentWillMount`
   - `componentWillReceiveProps`
   - `componentWillUpdate`
   - Deprecated string refs
   - Legacy context API

3. **Detects side-effects during render** by simulating mount/unmount/remount.

---

## 3. Why React Double Renders in Strict Mode (Dev Only)

In development, for components wrapped in `<React.StrictMode>`:

- React **renders the component twice** (back-to-back), but:
  - Only **commits once to the DOM**
  - The double render is only for detecting side effects and checking purity

This simulates how components behave under future concurrent features where:

- React may start rendering
- Pause
- Discard the work
- Restart rendering

If your code performs side effects directly in the render function, they will be visible as **duplicated effects** in development.

---

## 4. What Is Called Twice in Strict Mode?

The following are **intentionally invoked twice** in dev:

- Function components’ body  
- `useState` initializer functions  
- `useMemo`/`useCallback` initializer functions  
- Class constructors  
- Class `render` methods  
- `useEffect` cleanup + setup on initial mount  
- `useLayoutEffect` cleanup + setup on initial mount  

**But:**  
DOM updates are *not* applied twice; React only commits once.

---

## 5. What Strict Mode Does NOT Do

Strict Mode **does not**:

- Affect production builds  
- Change how components render in production  
- Guarantee correctness or performance  
- Replace testing or type checking  

It is a **development helper**, not a runtime feature.

---

## 6. Example: Strict Mode Double Render

```tsx
function Counter() {
  const [count, setCount] = useState(0);
  console.log("render", count);

  useEffect(() => {
    console.log("effect run");
    return () => console.log("effect cleanup");
  }, []);

  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

With Strict Mode in development:

On first mount you may see console logs like:

```text
render 0
render 0        // second render (dev only)
effect cleanup  // cleanup of simulated mount
effect run      // real effect after commit
```

This is **expected behavior**.

---

## 7. Common Issues You See Because of Strict Mode

### 7.1 Duplicate API Calls in Dev

If you do:

```tsx
useEffect(() => {
  fetch("/api/data");
}, []);
```

And your effect has **side effects with no idempotency**, you will observe:

- API called twice in development  
- In production → it will only run once

Correct approach:

- Make effects idempotent  
- Use abort controllers  
- Use guards (e.g., `didRunRef`) but only when truly needed  

---

### 7.2 Side Effects Inside Render

Bad:

```tsx
function BadComponent() {
  console.log("render");
  localStorage.setItem("key", "value"); // ❌ side effect in render
  return <div>Hi</div>;
}
```

In Strict Mode:

- This side effect runs twice  
- Reflects that it's unsafe to do side effects in render  

Correct: move side effects to `useEffect` or `useLayoutEffect`.

---

## 8. When To Use Strict Mode

You should **keep Strict Mode ON** in development for:

- New projects  
- Existing projects migrating to React 18+  
- Codebases that want to be compatible with future React versions  

It helps you:

- Find unsafe patterns early  
- Prepare for concurrent features  
- Identify double-render issues caused by side effects  

You can wrap:

```tsx
<React.StrictMode>
  <App />
</React.StrictMode>
```

Or selectively:

```tsx
<React.StrictMode>
  <SomeNewPartOfApp />
</React.StrictMode>
```

---

## 9. When You Might Temporarily Disable It

- When debugging logs become too noisy  
- When working with legacy libraries that misbehave under double rendering  
- When verifying one-time side effects (e.g., tracking, analytics), but only temporarily

Important:  
Turn it **back on** once debugging is done.

---

## 10. Strict Mode and Fiber / Concurrent Rendering

Strict Mode is tightly related to Fiber:

- Fiber allows React to interrupt, discard, and restart renders  
- Strict Mode **simulates** this by re-running render logic in development  
- This helps you find components that:
  - Depend on side effects during render  
  - Assume a render runs only once  
  - Mutate external state directly in render  

If your code works correctly under Strict Mode, it's more likely to behave well under concurrent rendering.

---

## 11. Best Practices with Strict Mode

✔ Treat renders as pure functions  
✔ Keep side effects in `useEffect` / `useLayoutEffect` only  
✔ Make effects idempotent / handle cleanup correctly  
✔ Avoid using module-level mutable state for UI logic  
✔ Test behavior in production build when verifying “single-shot” actions  

---

## 12. Common Interview Questions

1. What is React Strict Mode and why is it used?  
2. Why does React render components twice in Strict Mode (development)?  
3. Does Strict Mode run in production?  
4. What kind of issues can Strict Mode help detect?  
5. How does Strict Mode relate to concurrent rendering and Fiber?  
6. How would you handle duplicate API calls caused by Strict Mode in development?  
7. Should Strict Mode be used in all new React applications? Why?  

---

## 13. Quick Summary

- Strict Mode is a development-only feature  
- It double-invokes certain functions to detect unsafe side effects  
- It prepares apps for concurrent rendering  
- It does not affect production behavior  
- It is recommended to leave it enabled during development  
