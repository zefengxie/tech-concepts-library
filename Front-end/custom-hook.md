# Custom Hooks in React

## 1. What Is a Custom Hook?

A **custom hook** is a JavaScript function that:
- **Starts with** `use` (e.g. `useAuth`, `useForm`, `useFetch`)
- **Calls one or more React hooks** inside (`useState`, `useEffect`, `useContext`, etc.)
- Encapsulates **reusable logic**, not UI

Custom hooks let you **extract and share stateful logic** between components  
without duplicating code or resorting to complex patterns.

---

## 2. Why Custom Hooks Are Important

### ✔ Logic Reuse
Before hooks, reusing logic required:
- Higher-Order Components (HOCs)
- Render props
- Mixins (older patterns)

Custom hooks provide a **simple, function-based way** to reuse logic.

### ✔ Cleaner Components
Instead of large components with many effects and states,  
you extract logic into custom hooks:

- Component becomes focused on **rendering**
- Hook handles **behaviour** and **side effects**

### ✔ Better Separation of Concerns
- UI code (JSX) stays inside components
- Data fetching, form handling, subscriptions live in custom hooks

---

## 3. Anatomy of a Custom Hook

A custom hook:
- Is just a function
- Must start with `use`  
- Can accept arguments
- Returns values (state, handlers, flags)

Example shape:

```tsx
function useSomething(param: Type) {
  const [state, setState] = useState(initialValue);

  useEffect(() => {
    // side effect logic
  }, [param]);

  function handler() {
    // change state or call APIs
  }

  return { state, handler };
}
```

---

## 4. Simple Example: `useToggle`

```tsx
import { useState } from "react";

export function useToggle(initial: boolean = false) {
  const [value, setValue] = useState(initial);

  function toggle() {
    setValue(v => !v);
  }

  return { value, toggle, setValue };
}
```

Usage:

```tsx
function ToggleButton() {
  const { value: on, toggle } = useToggle();

  return <button onClick={toggle}>{on ? "ON" : "OFF"}</button>;
}
```

### ✔ Why this is good:
- Logic is reusable (`useToggle` can be used anywhere)
- Component is clean and focused on JSX
- Hook returns a **small, clear API**

---

## 5. More Advanced Example: `useFetch`

```tsx
import { useEffect, useState } from "react";

type FetchState<T> = {
  data: T | null;
  loading: boolean;
  error: Error | null;
};

export function useFetch<T = unknown>(url: string, options?: RequestInit) {
  const [state, setState] = useState<FetchState<T>>({
    data: null,
    loading: true,
    error: null,
  });

  useEffect(() => {
    let cancelled = false;

    async function fetchData() {
      setState(s => ({ ...s, loading: true, error: null }));
      try {
        const res = await fetch(url, options);
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const json = (await res.json()) as T;
        if (!cancelled) {
          setState({ data: json, loading: false, error: null });
        }
      } catch (err) {
        if (!cancelled) {
          setState({ data: null, loading: false, error: err as Error });
        }
      }
    }

    fetchData();
    return () => {
      cancelled = true;
    };
  }, [url]);

  return state;
}
```

Usage:

```tsx
function UserList() {
  const { data, loading, error } = useFetch<User[]>("/api/users");

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <ul>
      {data?.map(u => (
        <li key={u.id}>{u.name}</li>
      ))}
    </ul>
  );
}
```

### ✔ Benefits:
- Any component can reuse the fetch logic
- Clear contract: `{ data, loading, error }`
- Components remain focused on rendering

---

## 6. What Custom Hooks Are NOT

- They are **not** components (no JSX inside)
- They do **not** return JSX (should return data/functions, not UI)
- They are **not** lifecycle alternatives; they *use* hooks that manage lifecycle

If your function returns JSX, it’s a **component**, not a hook.

---

## 7. Common Anti-patterns

### ❌ 7.1 Naming without `use`
```tsx
function fetchUsersHook() {  // ❌ missing 'use'
  const [users, setUsers] = useState([]);
  // ...
}
```

React’s lint rules rely on `use` prefix to detect hooks.

### ✔ Correct:
```tsx
function useUsers() {
  // ...
}
```

---

### ❌ 7.2 Calling Custom Hooks Conditionally

```tsx
function Component({ enabled }: { enabled: boolean }) {
  if (enabled) {
    const value = useCustomHook(); // ❌ violates Rules of Hooks
  }
}
```

Hooks (including custom ones) must always be called in the same order.

---

### ❌ 7.3 Mixing UI and Logic in a Custom Hook

```tsx
function useBadModal() {
  const [open, setOpen] = useState(false);

  const modal = open ? <div>Modal</div> : null; // ❌ returning JSX here is not ideal

  return modal;
}
```

Better: return state + handlers, let the component decide JSX.

```tsx
function useModal() {
  const [open, setOpen] = useState(false);
  const openModal = () => setOpen(true);
  const closeModal = () => setOpen(false);
  return { open, openModal, closeModal };
}
```

---

## 8. Good Design Principles for Custom Hooks

- **Prefix with `use`**  
  e.g. `useAuth`, `useForm`, `usePagination`

- **Return a clear API**  
  Typically an object `{ state, handlers }`

- **Keep them focused**  
  One responsibility per hook.

- **Handle cleanup properly**  
  Use cleanup functions in `useEffect`.

- **Keep them UI-agnostic**  
  They should not know about specific DOM elements or JSX structure if possible.

---

## 9. When to Create a Custom Hook

Create a custom hook when:

- You see repeated hook logic in multiple components  
- Components are getting too large due to repeated state + effects  
- You want to separate business logic from presentation  
- You want a reusable pattern (e.g. `useDebounce`, `useLocalStorage`, `usePrevious`)

Do **not** create a custom hook if:

- Logic is only used in one place and is simple  
- You are abstracting too early (YAGNI)

---

## 10. Custom Hooks and Testing

Custom hooks can be tested by:
- Using React Testing Library’s `renderHook` utilities  
- Asserting returned values after simulated updates  

They are easier to test than large components since they focus purely on logic.

---

## 11. Interview Questions About Custom Hooks

1. What is a custom hook in React?  
2. Why would you create a custom hook?  
3. What are the benefits compared to HOCs or render props?  
4. How do custom hooks help with code reuse?  
5. What rules still apply to custom hooks?  
6. Give an example of a custom hook you wrote.  
7. Why must a custom hook’s name start with `use`?  
8. Can custom hooks return JSX? Why or why not?  
9. How would you test a custom hook?  
10. How would you refactor duplicated `useEffect` logic into a custom hook?

---

## 12. Quick Summary

- Custom hooks are **functions that reuse hook logic**, not components  
- Must start with `use` and follow the Rules of Hooks  
- Help with **logic reuse**, **cleaner components**, **testability**  
- Should return data and handlers, not JSX  
- Create them when you see repeated hook-based logic across components  
