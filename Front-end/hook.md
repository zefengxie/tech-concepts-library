# Hooks in Modern Frontend (React-focused)

## 1. What Are Hooks?

In React, **Hooks** are special functions that let you **“hook into” React features** from functional components.

They allow you to:
- Store and update **state**
- Run **side effects** (data fetching, subscriptions, timers)
- Access **context**
- Optimize **performance**
- Reuse **logic** between components (custom hooks)

Hooks **do not work** inside class components.  
They are designed for **function components only**.

---

## 2. Why Hooks Were Introduced

Before hooks, React had:
- Function components: simple, but **stateless**
- Class components: powerful (state, lifecycle), but:
  - verbose
  - harder to reuse logic
  - confusing `this` binding
  - tricky lifecycle methods (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`)

Hooks solve these problems by:
- Putting **state and lifecycle** in function components
- Allowing **logic reuse via custom hooks**
- Making components **smaller and more focused**

---

## 3. Rules of Hooks (Very Important)

1. **Only call hooks at the top level**
   - Do *not* call hooks inside loops, conditions, or nested functions.
   - Reason: React relies on **call order** to match hooks to state.

2. **Only call hooks from React functions**
   - React function components
   - Custom hooks (functions that start with `use`)

These rules are enforced by linters (ESLint plugin).

---

## 4. Core Built-in Hooks

### 4.1 `useState` — Local Component State

```tsx
const [count, setCount] = useState(0);

function increment() {
  setCount(prev => prev + 1);
}
```

- `useState(initialValue)` returns `[state, setState]`
- State persists across renders
- Calling `setState` triggers a re-render

---

### 4.2 `useEffect` — Side Effects

```tsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

Use `useEffect` for:
- Data fetching
- Subscriptions
- DOM manipulation
- Timers

The dependency array `[...]` controls when the effect runs.

---

### 4.3 `useContext` — Global-like Values

```tsx
const ThemeContext = createContext("light");

function Button() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Click</button>;
}
```

Allows components to access values without props drilling.

---

### 4.4 `useMemo` — Memoized Values

```tsx
const total = useMemo(() => {
  return items.reduce((sum, x) => sum + x.price, 0);
}, [items]);
```

Use `useMemo` to:
- Avoid expensive recalculations
- Only recompute when dependencies change

---

### 4.5 `useCallback` — Memoized Functions

```tsx
const handleClick = useCallback(() => {
  console.log("Clicked", id);
}, [id]);
```

Use `useCallback` when:
- Passing callbacks to child components
- Preventing unnecessary re-renders with `React.memo`

---

### 4.6 `useRef` — Mutable Reference

```tsx
const inputRef = useRef<HTMLInputElement | null>(null);

function focusInput() {
  inputRef.current?.focus();
}
```

Use `useRef` for:
- Accessing DOM elements
- Storing mutable values that do **not** trigger re-renders

---

## 5. Custom Hooks

A **custom hook** is a function that:
- Starts with `use` (e.g. `useAuth`, `useTodos`)
- Uses other hooks (`useState`, `useEffect`, etc.)
- Encapsulates reusable logic (not UI)

Example:

```tsx
function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    function handleResize() {
      setWidth(window.innerWidth);
    }
    window.addEventListener("resize", handleResize);
    return () => window.removeEventListener("resize", handleResize);
  }, []);

  return width;
}
```

Usage:

```tsx
function Component() {
  const width = useWindowWidth();
  return <div>Width: {width}</div>;
}
```

Benefits:
- Logic reuse across components
- Cleaner, smaller components
- Testable units of logic

---

## 6. Good Hook Usage Example

```tsx
function TodoList() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    let cancelled = false;

    async function fetchTodos() {
      try {
        const res = await fetch("/api/todos");
        const data = await res.json();
        if (!cancelled) {
          setTodos(data);
        }
      } finally {
        if (!cancelled) setLoading(false);
      }
    }

    fetchTodos();
    return () => {
      cancelled = true;
    };
  }, []);

  if (loading) return <p>Loading...</p>;

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}
```

### ✔ Why it's good:
- `useState` for UI states (`todos`, `loading`)
- `useEffect` for data fetching
- Cleanup function prevents state updates after unmount

---

## 7. Bad Hook Usage Example (Anti-patterns)

### ❌ Calling hooks inside a conditional

```tsx
function BadComponent({ show }) {
  if (show) {
    const [count, setCount] = useState(0); // ❌ breaks hook rules
  }
  // ...
}
```

This breaks hook ordering.

### ❌ Missing dependencies in `useEffect`

```tsx
useEffect(() => {
  console.log(value);
}, []); // ❌ value is used but not in dependency array
```

### ❌ Using hooks like lifecycle methods from classes

```tsx
useEffect(() => {
  // doing too much here (mount + update + cleanup logic mixed)
});
```

Better: split into multiple effects with different dependencies.

---

## 8. Hooks vs Class Lifecycle

| Class Lifecycle          | Rough Hook Equivalent             |
|--------------------------|-----------------------------------|
| `componentDidMount`      | `useEffect(..., [])`             |
| `componentDidUpdate`     | `useEffect(..., [deps])`         |
| `componentWillUnmount`   | cleanup function in `useEffect`  |
| `this.setState`          | `useState` / `useReducer`        |

However, hooks are more **fine-grained** and composable.

---

## 9. Best Practices with Hooks

- Respect the **Rules of Hooks**  
- Keep hooks at the **top level** of the component  
- Extract complex logic into **custom hooks**  
- Use `useEffect` only for **side effects**  
- Avoid overusing `useMemo`/`useCallback` prematurely  
- Prefer **small, focused components** with simple hooks

---

## 10. Common Interview Questions About Hooks

1. What are hooks in React and why were they introduced?  
2. Explain the Rules of Hooks.  
3. Difference between `useState` and `useRef`.  
4. How does `useEffect` work and what is the dependency array for?  
5. When would you use `useCallback` vs `useMemo`?  
6. What is a custom hook? Give an example.  
7. What problems do hooks solve compared to class components?  
8. How do you avoid infinite loops with `useEffect`?  
9. Why can’t you call hooks in loops or conditionals?  

---

## 11. Quick Summary

- Hooks add state, side effects, and other React features to function components  
- Must follow strict rules (top-level, React-only)  
- Core hooks: `useState`, `useEffect`, `useContext`, `useRef`, `useMemo`, `useCallback`  
- Custom hooks move reusable logic out of components  
- Proper hook usage leads to cleaner, testable, and maintainable code  

