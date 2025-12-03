# useContext Hook

## 1. What Is `useContext`?

`useContext` is a React Hook that lets components **read values from a Context object** without passing props manually through multiple levels.

```tsx
const value = useContext(MyContext);
```

It solves the problem of **props drilling**—passing data through components that do not need it.

---

## 2. Why Context Exists

Context allows React apps to share data that is considered *global* or *app-wide*, such as:

- Theme (light/dark)
- Authentication status
- User profile
- Language (i18n)
- Global settings
- Router information
- Global caches / stores
- UI states shared across distant components

Without context, these values would need to be passed through many layers of props.

---

## 3. Creating and Using Context

### Step 1: Create a Context

```tsx
const ThemeContext = createContext("light");
```

### Step 2: Provide a value

```tsx
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>
```

### Step 3: Consume it with `useContext`

```tsx
function Button() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Click</button>;
}
```

---

## 4. How Context Works Internally

- Each context has a **Provider**  
- Components inside the provider can use `useContext`  
- When the provider's `value` changes → all consumers re-render  
- Context is *not* a state management solution by itself, but can be combined with:
  - `useState`
  - `useReducer`
  - external stores

---

## 5. When To Use Context

### ✔ Good Use Cases:

- Theme switching
- Authentication (user object, auth tokens)
- Global modals / notifications
- Language settings
- Shared UI state across distant components
- Global caching

### ❌ Bad Use Cases:

- High-frequency updates (e.g., animations, resize events)
- Large datasets (context re-renders all consumers)
- Component-specific state (should stay local)

---

## 6. Good Example of useContext

```tsx
const AuthContext = createContext<{ user: User | null }>({ user: null });

function Navbar() {
  const { user } = useContext(AuthContext);
  return (
    <nav>
      {user ? <p>Welcome, {user.name}</p> : <p>Please sign in</p>}
    </nav>
  );
}
```

✔ Context is used for global authentication  
✔ No props drilling required  
✔ Simple and readable  

---

## 7. Bad Example (Misusing Context)

```tsx
// ❌ storing rapidly changing state in context
const MouseContext = createContext({ x: 0, y: 0 });

function App() {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  window.addEventListener("mousemove", e => {
    setPosition({ x: e.clientX, y: e.clientY });
  });

  return (
    <MouseContext.Provider value={position}>
      <Dashboard />
    </MouseContext.Provider>
  );
}
```

❌ This is problematic because:  
- Every mouse movement triggers context change  
- Every context consumer re-renders  
- Causes massive performance issues  

Better:  
Use event listeners or refs directly inside components that need the data.

---

## 8. Avoiding Unnecessary Re-renders

### Problem:
Changing `value` causes **ALL** consumers to re-render.

### Solution 1: Memoize value

```tsx
const value = useMemo(() => ({ user, setUser }), [user]);
<AuthContext.Provider value={value}>
```

### Solution 2: Split Contexts

Instead of:

```tsx
<AuthContext.Provider value={{ user, theme, language }}>
```

Use:

```tsx
<UserContext.Provider value={user}>
<ThemeContext.Provider value={theme}>
<LanguageContext.Provider value={language}>
```

---

## 9. Integrating Context with useReducer

```tsx
const AuthContext = createContext(null);

function authReducer(state, action) {
  switch (action.type) {
    case "login": return { ...state, user: action.payload };
    case "logout": return { user: null };
  }
}

function AuthProvider({ children }) {
  const [state, dispatch] = useReducer(authReducer, { user: null });

  return (
    <AuthContext.Provider value={{ state, dispatch }}>
      {children}
    </AuthContext.Provider>
  );
}
```

This is a common pattern for medium-size applications.

---

## 10. Best Practices

- Keep context values **minimal**
- Prefer multiple small contexts over one giant context
- Memoize the provider value
- Do not store large objects in context
- Do not store fast-changing state in context
- Use state management tools (Zustand, Redux) when scale increases

---

## 11. Common Mistakes

### ❌ Mistake 1: Forgetting `.Provider`
`useContext` without a provider returns the *default value*, not the real value.

### ❌ Mistake 2: Putting too much in context  
Large objects → frequent re-renders.

### ❌ Mistake 3: Using context for everything  
Not necessary—lifting state or passing props is often better.

### ❌ Mistake 4: Unmemoized provider values  
Causes re-renders even if values haven't logically changed.

---

## 12. Interview Questions

1. What is `useContext` and why is it used?  
2. What problem does context solve?  
3. What is props drilling?  
4. When should you avoid using context?  
5. Does updating context re-render components? Why?  
6. How do you optimize context performance?  
7. How do you combine context with `useReducer`?  
8. How does `useContext` compare to Redux or Zustand?  

---

## 13. Quick Summary

- `useContext` reads values from a context provider  
- Prevents props drilling  
- Re-renders consumers when provider value changes  
- Should be kept minimal and optimized  
- Works well with `useState`, `useReducer`, and memoization  
