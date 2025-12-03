# React Fiber Architecture

## 1. What Is Fiber?

**Fiber** is React’s internal engine for rendering — a reimplementation of React’s core algorithm, introduced in React 16.

It is essentially:

- A **data structure** (linked list of work units)
- A **scheduler** (decides when to do work)
- A **renderer** (applies DOM changes)

Fiber gives React:

- Interruptible rendering  
- Prioritized updates  
- Ability to pause, resume, and restart work  
- Smooth UI even during heavy computation  
- Foundation for Concurrent Mode  

Before Fiber, React rendering was **synchronous** and **blocking**.  
Fiber made React **asynchronous** and **interruptible**.

---

## 2. Why React Needed Fiber

### Before Fiber (React <16):
React render was:

- Synchronous
- Non-interruptible
- Recursive (call stack–based)
- Could freeze the UI if the component tree was large

If rendering took 200ms, the UI froze for 200ms.

### Fiber solves this by:

✔ Breaking rendering work into small units  
✔ Allowing React to pause rendering  
✔ Yielding to more important work (like user input)  
✔ Resuming work later  
✔ Aborting and restarting work  

This lets React handle large UIs without dropping frames.

---

## 3. What Exactly Is a Fiber Node?

A Fiber Node is a **JavaScript object** representing:

- A component (function/class/host element)
- Its state
- Its props
- Its children
- DOM information
- Effects to run
- Links to other fiber nodes

Key fields inside a Fiber Node:

```
fiber = {
  stateNode,
  child,
  sibling,
  return,
  pendingProps,
  memoizedProps,
  memoizedState,
  effectTag,
  alternate,
}
```

Important pointers:

- **child** → first child  
- **sibling** → next sibling  
- **return** → parent  

This forms a **linked list tree**.

---

## 4. Fiber = Work Unit

React breaks rendering into tiny tasks:

Each fiber node = a small piece of work React can start, pause, resume, or discard.

This is what allows React to avoid blocking the main thread.

---

## 5. Two Phases of Fiber Rendering

### **1. Render Phase (Reconciliation)**  
- React builds a new Fiber tree  
- Work is interruptible  
- Can be paused and resumed  
- No DOM updates happen here  

### **2. Commit Phase**  
- React applies DOM mutations  
- Work is synchronous  
- Runs effects (`useLayoutEffect` then `useEffect`)  
- Cannot be interrupted  

📝 Only the commit phase touches the actual DOM.

---

## 6. The Fiber Double Tree (Current vs Work-in-Progress)

React keeps two copies of the Fiber tree:

1. **Current Fiber Tree**  
   - Represents UI currently on screen

2. **Work-In-Progress (WIP) Tree**  
   - React builds this during the render phase  
   - Becomes “current” after commit phase

Each node links to its counterpart via:

```ts
fiber.alternate
```

This architecture allows:

- Fast updates  
- Incremental rendering  
- Efficient diffing  
- Rollback if needed  

---

## 7. Scheduling and Prioritization

Fiber introduced React's **scheduler**.

Different updates get different **priorities**, e.g.:

- 🟥 *High priority:* user input (typing, clicking)
- 🟧 *Medium priority:* state updates affecting visible content
- 🟩 *Low priority:* background rendering, prefetching, transitions

React decides:

- Which update should run now
- Which should wait
- Which should be abandoned and restarted

This is why React can maintain 60fps interactive performance.

---

## 8. Fiber and Concurrent Rendering

Fiber is the foundation of **React Concurrent Mode** (React 18):

- Transitions
- Suspense
- Idle rendering
- Streaming SSR
- Selective hydration

Without Fiber, concurrent features would be impossible.

---

## 9. How Fiber Improves Performance (Real Example)

### Without Fiber:
A large component updates → React must render everything synchronously  
→ UI freezes during render  
→ Typing becomes laggy

### With Fiber:
React can:
✔ Pause the heavy render  
✔ Process the keystroke event first  
✔ Resume rendering later  

This is how React keeps UI responsive even in large apps.

---

## 10. Example Showing Fiber at Work

```tsx
function App() {
  const [value, setValue] = useState("");

  function handleChange(e) {
    setValue(e.target.value);
    expensiveComputation(); // heavy render work
  }

  return <input value={value} onChange={handleChange} />;
}
```

Before Fiber:
- Typing would lag
- UI frozen while expensiveComputation runs

With Fiber:
- React interrupts rendering
- Handles the keystroke immediately
- Schedules the expensive work for later

User gets smooth typing experience.

---

## 11. Fiber vs Virtual DOM vs Reconciliation

| Concept | Role |
|--------|------|
| Virtual DOM | Tree representing UI |
| Reconciliation | Diffing algorithm comparing old vs new VDOM |
| Fiber | Engine + scheduler that performs rendering work incrementally |

### In short:

**Fiber is the engine.  
Reconciliation is the algorithm.  
Virtual DOM is the data.**

---

## 12. Common Misconceptions

### ❌ “Fiber = Virtual DOM”  
No. Fiber is the internal architecture enabling async rendering.

### ❌ “Fiber makes rendering faster”  
Fiber makes rendering *smoother* by allowing interruption, not by magically speeding work.

### ❌ “Fiber is only about performance”  
Fiber enables:
- Suspense  
- Streaming rendering  
- Transitions  
- Concurrent features  

---

## 13. When Fiber Matters in Real Development

- Understanding slow renders  
- Debugging async behavior of effects  
- Performance tuning under React 18  
- Understanding why Strict Mode double-invokes renders  
- Building custom renderers (React Native, Ink, Three Fiber)

---

## 14. Interview Questions

1. What is React Fiber?  
2. Why did React introduce Fiber?  
3. What problem does Fiber solve?  
4. Explain the difference between render phase and commit phase.  
5. What does it mean that React rendering is “interruptible”?  
6. How does Fiber relate to Concurrent Mode?  
7. What is the alternate fiber tree used for?  
8. Does Fiber make rendering faster? Why or why not?  
9. How does Fiber improve user input responsiveness?  
10. How are priorities used in the Fiber scheduler?

---

## 15. Quick Summary

- Fiber is React’s internal architecture for scheduling & rendering  
- Enables interruptible rendering and task prioritization  
- Core for Concurrent Mode features  
- Uses dual trees (current & work-in-progress)  
- Improves responsiveness, not raw speed  
- Works together with Virtual DOM and reconciliation  

