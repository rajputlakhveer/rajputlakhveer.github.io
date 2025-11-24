---
layout: home
title: "20 React Performance Hacks"
date: 2025-11-24
categories: "JavaScript"
tags: [ReactJS, JavaScript, Hacks, Programming, Performance, UI]
image: 'https://github.com/user-attachments/assets/74ee7e4a-71d0-4eae-952f-76ac2ef568c4'
---

# 🚀 **20 React Performance Hacks for Building Ultra-Fast UIs**

### ⚡ Supercharge Your Components, Speed Up Renders & Deliver Lightning-Fast Experiences!

If your React application is slowing down as it grows… you’re not alone 😅
React is powerful, but without tuning, even small mistakes can cause **re-renders, laggy UI, and wasted computation**.

This guide includes **20 powerful performance hacks** — each with examples — to help you build *ultra-fast* React apps 🚀💨

<img width="1024" height="1536" alt="ChatGPT Image Nov 24, 2025, 09_46_20 PM" src="https://github.com/user-attachments/assets/74ee7e4a-71d0-4eae-952f-76ac2ef568c4" />

---

# ⭐ **1. Use `React.memo()` to Prevent Unnecessary Re-renders**

**When to use:** Component receives the same props repeatedly.

**Example:**

```jsx
const UserCard = React.memo(function UserCard({ name }) {
  return <h2>{name}</h2>;
});
```

---

# ⭐ **2. Use `useCallback` for Stable Function References**

Re-created functions trigger re-renders.

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

---

# ⭐ **3. Use `useMemo` for Expensive Computations**

Memoize heavy logic.

```jsx
const result = useMemo(() => slowFunction(data), [data]);
```

---

# ⭐ **4. Lazy Load Components (`React.lazy`)**

Load pages only when needed.

```jsx
const Dashboard = React.lazy(() => import("./Dashboard"));
```

---

# ⭐ **5. Use Code Splitting with `Suspense`**

Make bundles smaller.

```jsx
<Suspense fallback={<Loader />}>
  <Dashboard />
</Suspense>
```

---

# ⭐ **6. Avoid Anonymous Functions in JSX**

Anonymous functions create new references.

❌ Bad:

```jsx
<button onClick={() => send()}>Click</button>
```

✅ Good:

```jsx
<button onClick={handleClick}>Click</button>
```

---

# ⭐ **7. Avoid Inline Objects in JSX**

Inline objects recreate new memory references.

```jsx
const style = { color: "red" };
return <h1 style={style}>Hi</h1>;
```

---

# ⭐ **8. Use Pagination + Infinite Scroll for Large Lists**

Never render 10,000 items at once.

---

# ⭐ **9. Use React Window / Virtualization for Huge Lists**

**`react-window`** renders only visible items.

```jsx
import { FixedSizeList as List } from "react-window";
```

---

# ⭐ **10. Debounce Inputs to Avoid Repeated State Updates**

Prevent rapid state changes.

```jsx
const debounced = useCallback(debounce(fn, 500), []);
```

---

# ⭐ **11. Use Throttling for Scroll Events**

Avoid 1000 scroll events/second.

---

# ⭐ **12. Batch Multiple State Updates**

React automatically batches updates.

```jsx
setCount(c => c + 1);
setFlag(f => !f);
```

---

# ⭐ **13. Avoid Storing Derived Data in State**

Computable values = **don’t store in state**.

❌ Bad:

```jsx
const [fullName, setFullName] = useState(first + last);
```

---

# ⭐ **14. Keep Components Small & Focused**

Large components re-render too often.

Break UI into reusable pieces.

---

# ⭐ **15. Use Production Build (`npm run build`)**

Minified + tree-shaken + optimized.

---

# ⭐ **16. Load Images Efficiently → Lazy Load, WebP, CDN**

Speed up UI visually.

```jsx
<img loading="lazy" src="image.webp" />
```

---

# ⭐ **17. Use `key` Properly in Lists**

Keys help React identify changed items.

```jsx
<ul>
  {users.map(u => <li key={u.id}>{u.name}</li>)}
</ul>
```

---

# ⭐ **18. Memoize Context Values (`useMemo`)**

Context triggers re-renders → stabilize values.

```jsx
const value = useMemo(() => ({ user }), [user]);
```

---

# ⭐ **19. Keep State Local, Not Global**

Avoid putting everything in Context or Redux.

Local state renders fewer components.

---

# ⭐ **20. Use Chrome Performance Profiler to Identify Bottlenecks**

Real devs **measure first**.
The profiler reveals:

* Slow renders
* Repeated renders
* Heavy logic
* Memory issues

---

# 🧠 **Mini Example: Slow Component → Optimized**

### ❌ Slow:

```jsx
function ProductList({ products }) {
  return products.map(p => (
    <ProductCard product={p} onClick={() => buy(p)} />
  ));
}
```

### ✅ Optimized:

```jsx
const ProductCard = React.memo(function ProductCard({ product, onClick }) {
  return <div onClick={onClick}>{product.name}</div>;
});

function ProductList({ products }) {
  const handleBuy = useCallback((p) => buy(p), []);

  return products.map(p => (
    <ProductCard key={p.id} product={p} onClick={() => handleBuy(p)} />
  ));
}
```

---

# 🎉 **Final Thoughts**

React is powerful — but performance needs **intentional engineering**.
Applying even **5–7** of these hacks can make your UI noticeably faster.
Apply all **20** → You get an *ultra-responsive*, *smooth*, *Google-friendly* UI 🚀✨
