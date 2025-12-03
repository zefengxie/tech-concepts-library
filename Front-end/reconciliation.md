# React Reconciliation

## 1. What Is Reconciliation?

**Reconciliation** is the process React uses to determine **what changed** in the virtual DOM and how to update the **real DOM** in the most efficient way.

React compares:

- **Previous virtual DOM tree**
- **New virtual DOM tree**

Then determines:

- What to keep  
- What to update  
- What to remove  
- What to add  

It is React’s **diffing algorithm**, optimized for speed and UI performance.

---

## 2. Why Reconciliation Exists

Direct DOM operations are slow.  
Re-rendering the entire page every time state changes would be too expensive.

React solves this by:

1. Keeping a lightweight virtual DOM representation
2. Comparing it with the new one
3. Updating only what changed

This gives React:

- High performance
- Declarative programming model
- Predictable state-driven UI updates

---

## 3. React’s Diffing Algorithm (Core Rules)

React’s reconciliation algorithm is built on two assumptions:

### **Rule 1 — Different element types produce different trees**

Changing:

```tsx
<div>...</div>
```

to:

```tsx
<span>...</span>
```

forces React to:

- Destroy the old `<div>`
- Create a new `<span>`
- Apply new props
- Recreate subtree under it

It does **not** diff children because the type changed.

---

### **Rule 2 — Keys define identity for list items**

Keys are the **only way** React knows:

- Which list item corresponds to which previous list item
- Whether to reuse, move, update, or delete an item

Without stable keys, React treats elements incorrectly.

---

## 4. How Reconciliation Works Step-by-Step

### Step 1: Component re-renders (virtual DOM created)

React calls the component function again and creates a **new virtual tree**.

### Step 2: Compare element type

- Same type → reuse DOM node  
- Different type → replace entire subtree  

### Step 3: Compare and update props

React checks:

- Has style changed?
- Has event handler changed?
- Has className changed?
- Has value changed?

Only changed props are updated on the real DOM.

### Step 4: Reconcile children

React compares child nodes recursively.

---

## 5. Reconciliation and Keys (Very Important!)

### Good Example

```tsx
items.map(item => <li key={item.id}>{item.text}</li>)
```

Stable unique keys → React knows identities → minimal DOM operations.

---

### Bad Example: Using Index As Key

```tsx
items.map((item, i) => <li key={i}>{item.text}</li>);
```

Why it’s bad:

- React cannot track item identity correctly
- Causes unnecessary re-renders
- Breaks animations
- Breaks input state inside lists
- Items appear to “jump” if array is sorted, filtered, or inserted

---

### Horrible Example: No Keys

```tsx
items.map(item => <li>{item.text}</li>);
```

React gives a warning, because:

- Every update causes mismatched nodes
- React may reuse wrong elements
- Performance drastically decreases

---

## 6. Example: How Incorrect Keys Break UI

Initial list:

| index | key | value |
|-------|-----|--------|
| 0     | 0   | A      |
| 1     | 1   | B      |
| 2     | 2   | C      |

If you insert “X” at the top:

| index | key | value |
|-------|-----|--------|
| 0     | 0   | X      |
| 1     | 1   | A      |
| 2     | 2   | B      |
| 3     | 3   | C      |

React thinks:

- A moved → but keys say A is “1”, so React thinks A is actually B
- B moved → React thinks B is C
- C moved → React thinks C is new

The entire list updates incorrectly.

Correct solution: use **stable IDs**.

---

## 7. Component Reconciliation

React also tracks component identity by **the component type**.

### Same component type:

```tsx
<MyButton />
<MyButton />
```

→ Reuse component instance  
→ Preserve state  

### Different component type:

```tsx
<MyButton />
<YourButton />
```

→ Destroy old  
→ Mount new  
→ State resets  

---

## 8. Reconciliation and State Preservation

React preserves state **only when identity is preserved**:

### Good:

```tsx
{isOpen && <Modal />}
```

Closing and reopening Modal:

- Component unmounts → state resets  
- New instance mounts → fresh state  

### Good with key changes:

```tsx
<Modal key={user.id} />
```

Changing user resets Modal state intentionally.

---

## 9. Reconciliation With Nested Elements

Example:

```tsx
<div>
  <button>Click</button>
</div>
```

If the button changes to:

```tsx
<div>
  <button disabled>Click</button>
</div>
```

React:

- Keeps the same `<button>` DOM node  
- Updates only the `disabled` attribute  
- Avoids re-creating the element

---

## 10. Reconciliation and Performance Optimization

You can reduce reconciliation cost by:

### ✔ Splitting components (component boundaries)
Smaller components → smaller diff calculation.

### ✔ Using `React.memo`
Prevents unnecessary re-renders.

### ✔ Using stable keys
Prevents mismatches and re-render storms.

### ✔ Avoiding inline objects/arrays/functions unless memoized
These create new references every render.

---

## 11. Common Misconceptions

### ❌ “React updates the whole DOM every time”
Wrong.  
React updates only what changed.

### ❌ “Keys are for performance only”
Wrong.  
Keys are for **identity tracking**, and performance is a side effect.

### ❌ “Reconciliation is slow”
React Fiber makes it extremely fast with prioritized scheduling.

---

## 12. Interview Questions

1. What is reconciliation in React?  
2. How does React’s diffing algorithm work?  
3. Why are keys important in lists?  
4. What happens if you use array index as key?  
5. How does React decide whether to reuse or replace a DOM node?  
6. How does reconciliation interact with state preservation?  
7. What performance optimizations relate to reconciliation?  
8. How does React treat different component types in diffing?  

---

## 13. Quick Summary

- Reconciliation = diffing algorithm comparing old & new virtual trees  
- React updates DOM minimally and efficiently  
- Keys determine identity and correctness in lists  
- Component identity controls state preservation  
- Best performance comes from stable structures, memoization, and proper keys  
