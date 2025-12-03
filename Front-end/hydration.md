# React Hydration

## 1. What Is Hydration?

**Hydration** is the process where React takes over a server-rendered HTML page and attaches event listeners, internal state, and Fiber nodes to turn static HTML into an interactive React application.

It is used when rendering is done on the **server (SSR)** and the client needs to “activate” the HTML.

Hydration =  
**Server creates HTML → Browser loads HTML → React attaches functionality**

---

## 2. Why Hydration Exists

Without hydration:

- The server sends HTML
- The browser displays HTML
- But **nothing is interactive**

Hydration allows React to:

✔ Reuse the server-rendered HTML  
✔ Avoid re-rendering the whole DOM from scratch  
✔ Attach event handlers  
✔ Build the React Fiber tree on top of existing DOM  
✔ Enable full interactivity without rebuilding the UI  

---

## 3. The Hydration Flow (Step-by-Step)

### **1. Server renders HTML**
Example:

```html
<div id="root">
  <button>0</button>
</div>
```

This is **static**. No interactivity.

---

### **2. Browser loads HTML**
User sees content instantly (good for SEO + performance).

---

### **3. React hydrates on the client**

```tsx
import { hydrateRoot } from 'react-dom/client';

hydrateRoot(document.getElementById("root"), <App />);
```

React:

- Reads the **existing DOM**
- Creates Fiber nodes matching the DOM
- Attaches event handlers
- Prepares for future updates

---

### **4. App becomes interactive**
Clicking the button now triggers React logic:

```tsx
setCount(count + 1);
```

---

## 4. Hydration vs Client-Side Rendering (CSR)

| Feature | CSR | SSR + Hydration |
|--------|-----|------------------|
| Initial load | Blank screen → JS builds UI | HTML appears instantly |
| SEO | Poor | Excellent |
| Interactivity | Fast after JS loads | Fast after hydration |
| JS required to see content | Yes | No |
| Performance | Slower initial | Faster initial |

---

## 5. Hydration vs Rehydration vs Rendering

### **Rendering**  
React builds both VDOM and DOM.

### **Hydration**  
React builds Fiber + VDOM and attaches to **existing DOM**.

### **Rehydration**  
Occurs when hydration cannot match existing DOM → React replaces it completely.

---

## 6. Hydration Mismatch

### A “hydration mismatch” happens when:

- Server-rendered HTML
- Does not match
- What the client React tree expects

Example mismatch:

Server HTML:

```html
<p>Count: 0</p>
```

Client initial render:

```tsx
<p>Count: 1</p>
```

The numbers differ → React logs:

```
Warning: Text content did not match.
```

### Causes of hydration mismatch:

❌ Using `Date.now()` or `Math.random()` in SSR  
❌ State differences between server and client  
❌ Browser-only APIs used on server  
❌ Conditional rendering depending on `window`  
❌ Race conditions in data fetching  

### Correct pattern:

```tsx
const [clientOnly, setClientOnly] = useState(false);

useEffect(() => setClientOnly(true), []);
```

---

## 7. Example of Correct Hydration

```tsx
export default function App({ serverCount }) {
  const [count, setCount] = useState(serverCount);

  return (
    <button onClick={() => setCount(c => c + 1)}>
      {count}
    </button>
  );
}
```

Server and client begin with the **same initial value** → no mismatch.

---

## 8. Example of a Bad Hydration (Mismatch)

```tsx
function App() {
  return <p>{Date.now()}</p>;
}
```

Server renders:

```
<p>1712159000000</p>
```

Client renders:

```
<p>1712159100000</p>
```

Mismatch → hydration breaks → React re-renders the entire subtree.

---

## 9. Hydration Performance and Streaming

React 18 introduced:

### **Streaming SSR**
Server can progressively stream HTML to the client.

### **Selective Hydration**
React hydrates only parts of the UI that are visible or interactive.

### **Suspense + SSR**
Hydrate components independently as data loads.

This improves:

- First contentful paint (FCP)
- Time to interactive (TTI)
- Perceived performance  

---

## 10. Partial Hydration (Conceptual)

Hydrating only parts of the page:

- Islands architecture (Astro, Qwik)
- App splits into independent interactive chunks
- React traditionally hydrates the entire tree

React 18 moves closer to islands via:

- Use of Suspense
- Selective hydration
- Lazy loading boundaries  

---

## 11. Hydration and Fiber

Hydration is deeply tied to Fiber:

- Fiber nodes are created to **match existing DOM nodes**
- No DOM creation happens during hydration
- Fiber must verify DOM is correct
- If mismatch is detected → React falls back and rebuilds the DOM

Fiber makes hydration:

✔ Interruptible  
✔ Selective  
✔ Concurrent  
✔ Recoverable  

---

## 12. Common Hydration Mistakes

### ❌ Server and client rendering produce different UI  
Use deterministic logic only.

### ❌ Using browser-only APIs during SSR  
Examples: `localStorage`, `window.innerWidth`

### ❌ Calling `useEffect` for initial server values  
Effects run **after hydration**, not before.

### ❌ CSS-in-JS mismatch  
Styled-components / Emotion must embed server-side CSS correctly.

---

## 13. Best Practices for Hydration

✔ Make initial server and client renders identical  
✔ Use `useEffect` for client-only logic  
✔ Avoid generating random values during SSR  
✔ Avoid reading browser APIs on server  
✔ Use Suspense for streaming hydration  
✔ Preload critical JS to speed up hydration  

---

## 14. Interview Questions

1. What is hydration in React?  
2. Why do we need hydration after SSR?  
3. What causes hydration mismatch?  
4. How does React recover from hydration mismatches?  
5. What is the difference between hydration and rehydration?  
6. How do Suspense and streaming SSR relate to hydration?  
7. Why must server and client render the same initial UI?  
8. How does Fiber enable selective hydration?  

---

## 15. Quick Summary

- Hydration turns static server-rendered HTML into an interactive React app  
- React attaches event listeners and builds a Fiber tree  
- Hydration mismatches occur when server and client renders differ  
- React 18 supports streaming, selective, and concurrent hydration  
- Essential for performance, SEO, and large-scale apps  

