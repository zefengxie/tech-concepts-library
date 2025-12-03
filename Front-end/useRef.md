# useRef Hook

## 1. What Is `useRef`?

`useRef` is a React Hook that provides a **mutable container** whose `.current` property persists across renders.

```tsx
const ref = useRef(initialValue);
```

It is used for:
- Accessing DOM elements
- Storing mutable values that do **not** trigger re-renders
- Keeping values between renders without causing state updates

---

## 2. Why `useRef` Exists

React re-runs function components on every render.  
Normal variables inside components:

- **Reset on every render**
- Do not persist values
- Cannot hold DOM references safely

`useRef` solves this by giving a **stable, persistent object**:

- The `.current` value survives re-renders
- Updating `.current` does **not** cause a re-render
- It can store any value: DOM element, number, object, timeout ID, etc.

---

## 3. Accessing DOM Elements

Most common use:

```tsx
function InputFocus() {
  const inputRef = useRef<HTMLInputElement | null>(null);

  function handleClick() {
    inputRef.current?.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>Focus input</button>
    </>
  );
}
```

### Why useRef works here:

- The DOM node is stored in `inputRef.current`
- React sets this ref after rendering  
- Accessible from event handlers

---

## 4. Storing Values That Persist Across Renders

Unlike `useState`, updating `useRef.current` **does not re-render** the component.

```tsx
const renderCount = useRef(0);

useEffect(() => {
  renderCount.current++;
});
```

Useful for:
- Tracking renders
- Tracking previous values
- Storing IDs or external handles

---

## 5. Storing Previous Value Example

```tsx
function Example({ value }) {
  const previous = useRef(value);

  useEffect(() => {
    previous.current = value;
  }, [value]);

  return (
    <p>
      Now: {value}, Before: {previous.current}
    </p>
  );
}
```

---

## 6. `useRef` vs `useState`

| Feature | useRef | useState |
|---------|--------|----------|
| Persists across renders | ✔ Yes | ✔ Yes |
| Causes re-render when updated | ❌ No | ✔ Yes |
| Holds DOM references | ✔ Yes | ❌ No |
| Best for | mutable values | reactive UI data |

---

## 7. When to Use `useRef`

### ✔ A DOM node must be accessed or manipulated  
(e.g., focus, scroll, measure size)

### ✔ A mutable value must persist without re-rendering  
Examples:
- A timeout ID  
- WebSocket instance  
- Previous state  
- Mutable counters  
- Flags (e.g., “isMounted”)  

### ✔ Avoiding re-renders for performance  
Using `useState` would cause unnecessary UI updates.

---

## 8. When NOT to Use `useRef`

### ❌ Storing values that should trigger a re-render  
Use state instead.

### ❌ Trying to sync UI with ref value  
Refs do not update the UI—state does.

### ❌ Replacing proper component design  
Refs should not be used to bypass React's data flow.

---

## 9. Good Example: Debounce with useRef

```tsx
function Search() {
  const timeoutRef = useRef<NodeJS.Timeout | null>(null);

  function handleChange(e) {
    const value = e.target.value;

    if (timeoutRef.current) {
      clearTimeout(timeoutRef.current);
    }

    timeoutRef.current = setTimeout(() => {
      console.log("Searching:", value);
    }, 300);
  }

  return <input onChange={handleChange} />;
}
```

✔ Stores a mutable timeout  
✔ Avoids unnecessary renders  
✔ Correct use case

---

## 10. Bad Example: Using useRef Instead of State

```tsx
const count = useRef(0);

return (
  <>
    <p>{count.current}</p>   // ❌ UI will not update
    <button onClick={() => count.current++}>Add</button>
  </>
);
```

Why it's wrong:
- Mutating the ref does **not** trigger a re-render
- UI shows stale values

Correct version:

```tsx
const [count, setCount] = useState(0);
```

---

## 11. Common Mistakes

### ❌ Mistake 1: Expecting updates to cause re-render  
`ref.current++` does nothing to UI.

### ❌ Mistake 2: Overusing refs instead of state  
State is the correct tool for reactive data.

### ❌ Mistake 3: Forgetting ref initial value  
```tsx
ref.current?.something
```
can throw if initial value wasn't set properly.

### ❌ Mistake 4: Using ref for derived values  
Refs should hold **mutable storage**, not computations.

---

## 12. Advanced Pattern: Imperative Handle (with forwardRef)

```tsx
const Input = forwardRef((props, ref) => {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    focusInput() {
      inputRef.current?.focus();
    }
  }));

  return <input ref={inputRef} />;
});
```

Parent:

```tsx
const ref = useRef(null);
ref.current?.focusInput();
```

Used when components need to expose **imperative methods**.

---

## 13. Interview Questions

1. What is `useRef` used for?  
2. How is `useRef` different from `useState`?  
3. Why does updating `useRef` not re-render the component?  
4. When do you use `useRef` for DOM access?  
5. How do refs behave across renders?  
6. What are common use cases for `useRef`?  
7. Explain a bug you solved using `useRef`.  

---

## 14. Quick Summary

- `useRef` stores **mutable values** that persist across renders  
- Changing `.current` does **not** cause re-render  
- Ideal for DOM nodes, timeouts, previous values, and external instances  
- Not a replacement for state  
