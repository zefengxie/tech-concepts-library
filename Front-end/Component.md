# Frontend Component 

## 1. What is a Frontend Component?

In modern frontend development (React, Vue, Angular, etc.), a **component** is a **self-contained, reusable UI unit** that:

- Encapsulates **markup (view)**
- Encapsulates **logic (behaviour)**
- Optionally encapsulates **state (data it manages)**

Components act like **LEGO blocks** to build complex UIs.

Examples:
- `<Button />`
- `<Navbar />`
- `<UserProfileCard />`
- `<ProductList />`

Each component defines:
- **What it looks like** (HTML / JSX / template)
- **How it behaves** (interactions, events)
- **What data it receives** (props / inputs)

---

## 2. Why Components Matter

### **2.1 Reusability**
Write once → use everywhere.  
Ensures consistency and reduces duplication.

### **2.2 Encapsulation**
Hides internal logic from parent components.  
Parents interact only via props or events.

### **2.3 Maintainability**
Smaller pieces of code are easier to:
- Debug
- Test
- Extend
- Refactor

### **2.4 Composability**
Components can contain other components, forming a UI tree.

---

## 3. Core Concepts Around Components

### **3.1 Props (Inputs)**
Props are **read-only data** passed from parent to child.

```tsx
function UserCard({ name, age }) {
  return (
    <div className="card">
      <h2>{name}</h2>
      <p>{age} years old</p>
    </div>
  );
}
```

### **3.2 State (Internal Data)**
State is **local mutable data**.

```tsx
function ToggleButton() {
  const [on, setOn] = useState(false);
  return <button onClick={() => setOn(!on)}>{on ? "On" : "Off"}</button>;
}
```

### **3.3 One-way Data Flow**
Parent → Child (via props)  
Child → Parent (via callbacks)

---

## 4. Good Component Example

```tsx
export function TodoList({ items, onToggle }) {
  if (items.length === 0) return <p>No tasks yet.</p>;

  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>
          <label>
            <input
              type="checkbox"
              checked={item.completed}
              onChange={() => onToggle(item.id)}
            />
            <span style={{ textDecoration: item.completed ? "line-through" : "none" }}>
              {item.title}
            </span>
          </label>
        </li>
      ))}
    </ul>
  );
}
```

### ✔ Why this is good:
- Stateless → reusable
- Clear responsibility
- Easy to test
- Predictable behaviour

---

## 5. Bad Component Example (Anti-pattern)

```tsx
export function TodoListBad() {
  const [items, setItems] = useState([
    { id: "1", title: "Study React", completed: false },
    { id: "2", title: "Wash dishes", completed: true }
  ]);

  function toggle(id) {
    setItems(items =>
      items.map(i => (i.id === id ? { ...i, completed: !i.completed } : i))
    );
  }

  return (
    <>
      <ul>
        {items.map(i => (
          <li key={i.id}>
            <input
              type="checkbox"
              checked={i.completed}
              onChange={() => toggle(i.id)}
            />
            {i.title}
          </li>
        ))}
      </ul>
    </>
  );
}
```

### ✘ Problems:
- Hard-coded data (not reusable)
- Manages logic + UI
- Hard to test
- Violates SRP (Single Responsibility Principle)

---

## 6. Smart vs Dumb Components

### **Presentational Components (Dumb)**
- Focus on UI
- Stateless or minimal state
- Reusable
- Receive data via props

### **Container Components (Smart)**
- Manage data fetching
- Handle business logic
- Pass processed data to children

---

## 7. Controlled vs Uncontrolled Components

### **Controlled:**
React state controls the value:

```tsx
function NameInput() {
  const [name, setName] = useState("");
  return <input value={name} onChange={e => setName(e.target.value)} />;
}
```

### **Uncontrolled:**
DOM controls the value:

```tsx
function NameInput() {
  const inputRef = useRef();
  return <input ref={inputRef} />;
}
```

---

## 8. Pure vs Impure Components

### **Pure Component**
Same props → same output  
No side effects in render.

```tsx
const Price = React.memo(function Price({ value }) {
  return <span>${value.toFixed(2)}</span>;
});
```

### **Impure Component (bad)**

```tsx
function Price({ value }) {
  localStorage.setItem("price", value); // ❌ side effect
  return <span>${value.toFixed(2)}</span>;
}
```

Side effects belong in `useEffect`.

---

## 9. How to Design Good Frontend Components

### **9.1 Principles**
- Single responsibility
- Small & focused
- Predictable behaviour
- Explicit props
- No hidden side effects

### **9.2 Do / Don’t**

**✔ Do**
- Use props for data
- Use callbacks for events
- Extract repeated UI into components

**✘ Don’t**
- Mix business logic with UI
- Mutate props
- Hard-code data into reusable components

---

## 10. Common Interview Questions About Components

1. What is a frontend component?
2. Difference between props and state?
3. What is a controlled component?
4. What is a pure component?
5. How do you design reusable components?
6. What is the container vs presentational pattern?
7. How do you handle side effects in components?
8. What are anti-patterns in component design?

---

## 11. One-Minute Summary

- Component = reusable UI building block  
- Encapsulates view + logic + optional state  
- Uses props (input) & state (internal data)  
- Predictable one-way data flow  
- Good components are small, testable, composable  
