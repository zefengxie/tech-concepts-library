# React Render Cycle

## 1. What Is the Render Cycle?

The **render cycle** in React describes what happens from the moment something triggers an update (state/props/context change) to the moment the UI on the screen reflects that change.

At a high level:

1. Something triggers an update (state/props/context)
2. React **renders** components (calls function components)
3. React **reconciles** the new virtual tree with the previous one
4. React applies the **minimal set of changes** to the real DOM
5. Effects and layout work run (`useEffect`, `useLayoutEffect`)

---

## 2. What Triggers a Render?

A render is triggered when:

- A component’s **state** changes (`setState`, `useState`, `useReducer`)
- A component’s **props** change (parent re-renders with new props)
- A component’s **context value** changes (`useContext`)
- In React 18+, React may re-render for **Strict Mode double-invocation** in development

Important: **Calling `setState` schedules a render**. It does not immediately update the DOM.

---

## 3. Phases of the Render Cycle

Conceptually, React’s work is split into 2 main phases:

1. **Render (or Reconciliation) Phase**  
   - React calls your components  
   - Builds the new virtual tree  
   - Calculates which parts of the UI need to change  
   - This phase must be **pure** (no side effects)

2. **Commit Phase**  
   - React applies changes to the real DOM  
   - Runs `useLayoutEffect` cleanup → `useLayoutEffect` setup  
   - Browser paints the UI  
   - Then runs `useEffect` cleanup → `useEffect` setup

---

## 4. Detailed Step-by-Step Flow

### Step 1: Update is Scheduled

Examples:

```tsx
setCount(c => c + 1);       // state update
setUser({ name: "Alice" }); // state update
```

React marks the component as **“needs re-render”**.

### Step 2: Render Phase

- React calls function components from root → leaf
- For each component:
  - React evaluates JSX → virtual elements
  - Hooks (`useState`, `useEffect`, etc.) are read
  - No DOM is mutated yet

In this phase:

- The code must be **pure**  
- You should **not** cause side effects (no subscriptions, timers, manual DOM)  

### Step 3: Reconciliation

React compares:

- **Previous virtual DOM tree**
- **New virtual DOM tree**

It computes a minimal **diff**:

- Which elements are added / removed / updated  
- Which attributes/styles changed  
- Which event handlers changed  

### Step 4: Commit Phase

React:

- Applies DOM mutations (insert/update/remove nodes)
- Runs `useLayoutEffect` cleanup → setup
- Browser paints the updated UI
- Then runs `useEffect` cleanup → setup (async after paint)

---

## 5. Render Does NOT Always Mean DOM Update

React can **render** a component (call its function) without committing any DOM changes if:

- The resulting virtual tree is identical
- Or React decides to bail out for performance

So:
- Render = React **thinks about what to show**
- Commit = React **actually updates the DOM**

---

## 6. Example: Simple Render Cycle

```tsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Effect: count is", count);
  }, [count]);

  console.log("Render: count is", count);

  return (
    <button onClick={() => setCount(c => c + 1)}>
      {count}
    </button>
  );
}
```

Click flow:

1. User clicks the button  
2. `setCount` schedules an update  
3. React renders `Counter`:
   - logs: `Render: count is 1`
4. React commits DOM change (text `0` → `1`)  
5. After paint, `useEffect` runs:
   - logs: `Effect: count is 1`

---

## 7. React 18 & Strict Mode (Development Only)

In development Strict Mode:

- React may **intentionally call components twice** in the render phase
- This is done to detect unsafe side effects during rendering

This means:

- You might see logs like `Render` running twice  
- But commit still happens properly once

This does **not** happen in production builds.

---

## 8. Common Misunderstandings

### ❌ “State updates immediately change the variable”

```tsx
setCount(count + 1);
console.log(count); // still old
```

Reason:
- Render is scheduled
- The new value appears in the **next render**

### ❌ “useEffect runs before render”

In reality:

- Render → Commit DOM → Browser paint → `useEffect`

### ❌ “Every render means DOM update”

React may skip DOM updates if nothing changed.

---

## 9. Performance and the Render Cycle

To optimize the render cycle:

- Reduce needless renders:
  - Use `React.memo` for pure components
  - Use `useCallback` / `useMemo` thoughtfully
- Avoid heavy work directly in render:
  - Move expensive calculations into `useMemo`
- Avoid unnecessary deep trees:
  - Split components, isolate state

React’s **Fiber** architecture (covered in a separate file) helps:

- Break renders into chunks  
- Prioritize important updates (like user input)

---

## 10. Lifecycle Hooks and the Render Cycle

| Moment                                     | Related Hook / Concept              |
|-------------------------------------------|-------------------------------------|
| Before render                             | Function body runs                  |
| After commit and paint                    | `useEffect`                         |
| After DOM mutation but before paint       | `useLayoutEffect`                   |
| On cleanup (before next effect / unmount) | Cleanup functions from effects      |

---

## 11. Interview Questions

1. What triggers a React render?  
2. Explain the difference between **render phase** and **commit phase**.  
3. Does a state update always lead to a DOM update? Why or why not?  
4. When do `useEffect` and `useLayoutEffect` run?  
5. Why should render functions be pure?  
6. What is reconciliation?  
7. How does Strict Mode affect the render cycle in development?  

---

## 12. Quick Summary

- Render cycle: update → render → diff → commit → effects  
- Render ≠ DOM change; commit = DOM update  
- State/props/context changes schedule renders  
- Hooks are evaluated in the render phase; effects run after commit  
- Understanding the cycle is key to debugging and performance tuning  
