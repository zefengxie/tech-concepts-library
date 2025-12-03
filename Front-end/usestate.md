# useState Hook

## 1. What Is `useState`?

`useState` is a React Hook that allows function components to store and update **local state**.  
It provides React’s reactivity: **when state changes, the component re-renders**.

```tsx
const [state, setState] = useState(initialValue);
```

- `state` → the current value  
- `setState` → a function that updates the value  
- `initialValue` → the default value used in the first render  

---

## 2. Why `useState` Is Needed

Before hooks, React required class components for:

- storing state
- updating state with `this.setState`
- triggering UI re-renders

`useState` allows **function components** to do all of this cleanly.

### Solves:
- No more `this` binding confusion  
- Encapsulated local UI logic  
- Smaller, cleaner components  

---

## 3. How `useState` Works Internally (Important Concept)

### ✔ `useState` preserves the value across renders  
Even though the component function runs multiple times, React stores the state *outside* the function body.

### ✔ Updating state triggers a re-render  
When `setState` is called:
1. Component schedules re-render  
2. React re-executes the function  
3. The new state value is returned  

### ✔ State updates are **asynchronous**  
React batches updates for performance.

---

## 4. Basic Usage

```tsx
const [count, setCount] = useState(0);

function increment() {
  setCount(count + 1);
}
```

---

## 5. Using Functional Updates (Important)

When new state depends on previous state:

### ❌ Wrong

```tsx
setCount(count + 1);
setCount(count + 1); 
// Might still end up increasing once
```

### ✔ Correct

```tsx
setCount(c => c + 1);
setCount(c => c + 1);
// Always increases twice
```

Functional updates avoid stale state problems.

---

## 6. Initial State Best Practices

### 6.1 Lazy initialization  
When computing the initial value is expensive:

```tsx
const [value, setValue] = useState(() => computeInitialValue());
```

The function runs **only once**.

### 6.2 Initial state from props (only on first render)

```tsx
function Example({ initialCount }) {
  const [count, setCount] = useState(initialCount);
}
```

---

## 7. Updating Object / Array State

### 7.1 Updating object state

```tsx
const [user, setUser] = useState({ name: "Wendy", age: 24 });

setUser({
  ...user,
  age: 25
});
```

### 7.2 Updating array state

```tsx
const [items, setItems] = useState<string[]>([]);

setItems([...items, "new item"]);
```

### ❌ Wrong mutation example

```tsx
items.push("new"); // ❌ does not re-render
setItems(items);    // ❌ same reference
```

React requires **new objects** for state changes.

---

## 8. Common Mistakes with `useState`

### ❌ Mistake 1: Mutating state directly  
```tsx
user.age = 30; // ❌ no re-render
```

### ❌ Mistake 2: Expecting immediate state update  
```tsx
setCount(count + 1);
console.log(count); // ❌ still old value
```

### ❌ Mistake 3: Storing derived state  
```tsx
const [fullName, setFullName] = useState(first + last); // ❌ unnecessary
```

Better:
```tsx
const fullName = first + last;
```

### ❌ Mistake 4: Using state for temporary calculations  
Avoid polluting state with values that can be computed inline.

---

## 9. When to Use `useState` vs `useReducer`

### Use `useState` when:
- State is simple  
- Updates are independent  
- No complex transitions  

### Use `useReducer` when:
- State has multiple sub-values  
- Updates depend on each other  
- Logic is complex or structured  

Example:
- forms  
- wizards  
- state machines  
- complex dashboard interactions  

---

## 10. Good Example of `useState` in Real UI

```tsx
function LoginForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");

  async function handleSubmit(e) {
    e.preventDefault();
    try {
      await login(email, password);
    } catch {
      setError("Invalid credentials");
    }
  }

  return (
    <form onSubmit={handleSubmit}>
      {error && <p>{error}</p>}

      <input
        value={email}
        onChange={e => setEmail(e.target.value)}
        placeholder="Email"
      />

      <input
        value={password}
        type="password"
        onChange={e => setPassword(e.target.value)}
        placeholder="Password"
      />

      <button type="submit">Login</button>
    </form>
  );
}
```

---

## 11. Bad Example of `useState` (Anti-pattern)

### ❌ Too many separate state variables

```tsx
const [x1, setX1] = useState(0);
const [x2, setX2] = useState(0);
const [x3, setX3] = useState(0);
const [x4, setX4] = useState(0);
```

Better:

```tsx
const [values, setValues] = useState({ x1: 0, x2: 0, x3: 0, x4: 0 });
```

---

## 12. Interview Questions About `useState`

1. How does `useState` work?  
2. Why are updates asynchronous?  
3. What is stale state?  
4. When should you use a functional update?  
5. Why can't we mutate state directly?  
6. How do you update array/object state correctly?  
7. How does lazy initialization work?  
8. When should you use `useReducer` instead of `useState`?  
9. Why does React re-render when state changes?  

---

## 13. Quick Summary

- `useState` adds reactivity to function components  
- State persists across renders  
- Updates trigger re-renders  
- Never mutate state directly  
- Use functional updates for dependent state  
- Keep state minimal and clean  
