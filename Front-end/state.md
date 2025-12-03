# State in Frontend Components

## 1. What Is State?

**State** is data that is **owned and managed by the component itself**.  
It represents information that changes over time and affects what the UI displays.

State is:
- **Mutable** (unlike props)
- **Internal** to the component
- Used to store dynamic data such as:
  - user input
  - toggles
  - counters
  - lists fetched from APIs
  - loading/error states

---

## 2. Why State Exists (Purpose)

### ✔ Enables Interactive UI  
Apps need to respond to:
- user actions  
- keyboard input  
- mouse movement  
- form submission  

### ✔ Allows UI to Update Automatically  
When state changes, the component re-renders.

### ✔ Encapsulates Component Behavior  
A component can manage its own lifecycle and transitions without exposing internal logic.

---

## 3. Typical Uses of State

### • UI toggles  
```tsx
const [open, setOpen] = useState(false);
```

### • Form input 
```tsx
const [email, setEmail] = useState("");
```

### • Lists and API data  
```tsx
const [items, setItems] = useState([]);
```

### • Status signals  
```tsx
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);
```

### • Controlled components  
```tsx
<input value={name} onChange={e => setName(e.target.value)} />
```

---

## 4. State vs Props

| Props | State |
|-------|-------|
| Controlled by parent | Controlled by component |
| Read-only | Mutable |
| External input | Internal data |
| Makes components reusable | Makes components dynamic |
| Does not trigger updates by itself | Triggers re-renders |

---

## 5. Good State Example

```tsx
function Toggle() {
  const [isOn, setIsOn] = useState(false);

  return (
    <button onClick={() => setIsOn(!isOn)}>
      {isOn ? "ON" : "OFF"}
    </button>
  );
}
```

### ✔ Why it's good:
- Clear responsibility  
- State only stores what is necessary  
- Easy to understand and extend  

---

## 6. Bad State Example

### ❌ Storing derived values in state  
```tsx
const [total, setTotal] = useState(price * quantity); // ❌ NO
```

Better:
```tsx
const total = price * quantity;
```

### ❌ Storing large or unnecessary objects  
```tsx
const [user, setUser] = useState({ ...bigData }); // ❌ expensive & unnecessary
```

### ❌ Duplicating state  
```tsx
const [firstName, setFirstName] = useState("");
const [fullName, setFullName] = useState(""); // ❌ derived state
```

Better:
```tsx
const fullName = firstName + " " + lastName;
```

---

## 7. Rules of State (React-specific)

### ✔ Rule 1: Never mutate state directly  
```tsx
count++; // ❌
setCount(count + 1); // ✔
```

### ✔ Rule 2: Updates may be async  
```tsx
setCount(count + 1);
setCount(count + 1); // ❌ not guaranteed to increment twice
```

Use functional updates:
```tsx
setCount(c => c + 1);
setCount(c => c + 1); // ✔ increments twice
```

### ✔ Rule 3: State should be minimal  
Store **only what cannot be computed**.

---

## 8. Lifting State Up

When multiple components need the same state:

Bad:
```tsx
<TodoItem completed={true} />
<TodoItem completed={true} />
```

Better:
```tsx
<TodoList todos={todos} onToggle={toggleTodo} />
```

The parent becomes the state owner.

---

## 9. When to Use State vs When NOT to Use State

### Use state when:
- UI responds to user interaction  
- Data changes over time  
- You need controlled inputs  
- Component needs internal memory  

### Do NOT use state for:
- Constants  
- Derived values  
- Data that belongs to the parent  
- Temporary variables  
- Computed layout or styling  

---

## 10. Best Practices

### ✔ Keep state minimal  
Store only fundamental values.

### ✔ Group related state  
Use objects when necessary.

### ✔ Avoid deeply nested state  
Hard to update reliably.

### ✔ Use state machines (optional)  
For complex flows, XState or reducers help.

### ✔ Clean state on unmount  
Avoid memory leaks.

---

## 11. Interview Questions About State

1. What is state in React/Vue?  
2. How is state different from props?  
3. Why should you avoid derived state?  
4. Why can’t we mutate state directly?  
5. When do you lift state up?  
6. What is a controlled component?  
7. Why are state updates asynchronous?  
8. How does state cause re-renders?  

---

## 12. Quick Summary

- State is internal, mutable data  
- Used for UI interactions & dynamic behavior  
- Props = external input; State = internal data  
- Minimal, clean state design improves performance  
- Avoid duplicating or mutating state  
- Lift state when multiple components need it  

