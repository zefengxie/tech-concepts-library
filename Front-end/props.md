# Props in Frontend Components

## 1. What Are Props?

Props (short for *properties*) are **inputs passed from a parent component to a child component**.  
They define how the child behaves and what data it displays.

Props are:
- **Read‑only**
- **Controlled by the parent**
- The **public API** of a component
- Used for **data**, **callbacks**, **UI configuration**, and **children**

---

## 2. Why Props Are Important

### ✔ One‑way Data Flow  
Props enforce predictable flow: **Parent → Child**.

### ✔ Reusability  
A component behaves differently based on props, making it reusable.

### ✔ Encapsulation  
The child does not manage or mutate external data.

### ✔ Easier Testing  
Components can be tested by passing mocked props.

---

## 3. What Props Are Used For

### • Data  
```tsx
<UserCard name="Alice" age={22} />
```

### • Behavior (callbacks)  
```tsx
<Button onClick={handleSave} />
```

### • UI configuration  
```tsx
<Modal size="lg" backdrop="static" />
```

### • Children  
```tsx
<Card>
  <p>Hello world</p>
</Card>
```

### • Render props  
```tsx
<DataProvider render={(data) => <Chart data={data} />} />
```

---

## 4. Props Are Read‑Only

Mutating props is an anti‑pattern:

```tsx
function Bad({ count }) {
  count++; // ❌ do not mutate props
}
```

Correct approach:
- Child **receives** data  
- Child **requests** updates via callbacks  
- Parent **owns and updates** the data

---

## 5. Good Props Example

```tsx
type CounterProps = {
  count: number;
  onIncrement: () => void;
};

function Counter({ count, onIncrement }: CounterProps) {
  return <button onClick={onIncrement}>Count: {count}</button>;
}
```

### ✔ Why it's good:
- Clear prop API
- Parent controls state
- Child is reusable and predictable

---

## 6. Bad Props Example

### ❌ Child hard‑codes logic  
```tsx
function TodoItem({ title }) {
  const completed = true; // ❌ should come from props
}
```

### ❌ Too many props  
Indicates the component does too much.

### ❌ Mutating props  
Breaks React’s data flow.

---

## 7. Props vs State

| Props | State |
|-------|-------|
| Controlled by parent | Controlled by component |
| Read‑only | Mutable |
| External input | Internal data |
| Predictable | Dynamic |

---

## 8. Props Drilling Problem

Passing props through many unnecessary layers:

```tsx
<GrandParent user={user}>
  <Parent user={user}>
    <Child user={user} />
  </Parent>
</GrandParent>
```

Solutions:
- Context
- State management libraries (Redux, Zustand)
- Composition
- Lifting state

---

## 9. Best Practices

- Keep props minimal and meaningful  
- Use TypeScript for strict prop typing  
- Name callbacks with **onX**  
- Use default props for optional settings  
- Avoid passing unnecessary objects or functions  

---

## 10. Interview Questions

1. What are props in React/Vue?  
2. Why are props read‑only?  
3. Explain props vs state.  
4. What is props drilling?  
5. How do you pass a callback as a prop?  
6. When should you use context instead of props?  

---

## 11. Quick Summary

- Props = component inputs  
- Parent → Child data flow  
- Read‑only  
- Used for data, configuration, callbacks, children  
- Avoid props drilling  
- Design clean, predictable prop APIs  
