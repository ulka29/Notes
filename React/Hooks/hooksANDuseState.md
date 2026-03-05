# ⚛️ Complete Deep + Practical Notes on React Hooks & Rendering

> Built around real-world React apps (Todo, Password Toggle, Counter)  
> Structured for deep understanding + interviews.

---

## 📚 Table of Contents

1. Introduction to Hooks  
2. Rules of Hooks  
3. useState – Core Hook  
4. Event Handling & Arrow Functions  
5. Functional Updates (`prev`)  
6. Professional State Update Patterns  
7. Why State Mutation Is Wrong  
8. List Rendering & Keys  
9. Removing Items with filter()  
10. Conditional Rendering  
11. Dynamic Attributes  
12. preventDefault()  
13. Component Lifecycle  
14. What Happens on State Update?  
15. Re-render Behavior  
16. Virtual DOM vs Real DOM  
17. Reconciliation  
18. Parent Re-render Example  
19. What Triggers Re-render?  
20. Same State Value Case  
21. Hook Storage Mental Model  
22. Final Mental Model  
23. Interview Summary  

---

# 1️⃣ Introduction to Hooks

Hooks were introduced in **React v16.8**.

They allow:

- State management  
- Lifecycle handling  
- Side effects  
- Reusable logic  

inside **functional components**.

---

## 🔹 Before Hooks

| Component Type | Capability        |
|----------------|------------------|
| Functional     | UI only           |
| Class          | State + Lifecycle |

---

## 🔹 After Hooks

Functional components can now:

- Manage state  
- Handle lifecycle  
- Replace class components  
- Reuse logic cleanly  

---

# 2️⃣ Rules of Hooks (Critical)

### Rule 1: Call Hooks at Top Level Only

❌ Not inside loops  
❌ Not inside conditions  
❌ Not inside nested functions  

Reason: React tracks hooks by **order of execution**.

---

### Rule 2: Only Call Hooks Inside

- Functional components  
- Custom hooks  

---

# 3️⃣ useState – Core Hook

```js
const [state, setState] = useState(initialValue);
```

Returns:

- Current state  
- Setter function  

State updates are:

- Asynchronous  
- Batched  
- Trigger re-render  

---

# 4️⃣ Arrow Function in Events

### React expects:

```jsx
onChange={functionReference}
```

NOT:

```jsx
onChange={functionCall()}
```

---

## ❌ Wrong

```jsx
onChange={setInput(e.target.value)}
```

This executes immediately during render and causes errors or infinite re-renders.

---

## ✅ Correct

```jsx
onChange={(e) => setInput(e.target.value)}
```

Now React stores the function and calls it when the event occurs.

---

## Named Handler Version

```jsx
const handleChange = (e) => {
  setInput(e.target.value);
};

<input onChange={handleChange} />
```

Internal Flow:

1. Component renders  
2. React attaches handler  
3. User types  
4. Event fires  
5. React calls handler  
6. setState runs  
7. Re-render occurs  

---

# 5️⃣ Why Use `prev` in State Updates?

React updates are:

- Asynchronous  
- Batched  

---

## ❌ Dangerous

```js
setCount(count + 1);
setCount(count + 1);
```

Expected: +2  
Actual: +1  

---

## ✅ Correct

```js
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Each update receives the latest value.

---

# 6️⃣ Professional State Update Patterns

### 1. Direct Update (Not Dependent)

```js
setInput("Hello");
```

---

### 2. Functional Update (Dependent)

```js
setCount(prev => prev + 1);
```

Use when:

- Based on previous state  
- Inside async logic  
- Multiple updates  

---

### 3. Object Update

```js
setUser(prev => ({
  ...prev,
  name: "Ulka"
}));
```

Without spread:

```js
setUser({ name: "Ulka" });
```

Other properties are lost.

---

### 4. Array Update

```js
setTodos(prev => [...prev, newTodo]);
```

Creates new reference.

---

# 7️⃣ Why Not Mutate State?

❌ Wrong:

```js
prev.push(newTodo);
```

React uses shallow comparison:

```
oldReference !== newReference
```

If reference unchanged → no re-render.

---

## ✅ Correct

```js
[...prev, newTodo]
```

New reference → React detects change.

---

# 8️⃣ List Rendering with map()

```jsx
<ul>
  {todos.map((todo) => (
    <li key={todo.id}>
      {todo.id}. {todo.text}
      <button onClick={() => removeTodo(todo.id)}>
        Remove
      </button>
    </li>
  ))}
</ul>
```

---

## Why `key` Is Mandatory

Without key:

- Matched by index  
- Removing first shifts all  
- Unnecessary re-renders  

With key:

- Matched by ID  
- Only changed element updates  

---

# 9️⃣ Removing Item with filter()

```js
setTodos(prev => prev.filter(todo => todo.id !== id));
```

Creates new array → triggers re-render → React removes only that `<li>`.

---

# 🔟 Conditional Rendering

### Ternary

```jsx
{isPara ? "Hide Details" : "Show Details"}
```

---

### Logical AND

```jsx
{isPara && <p>Details...</p>}
```

---

# 1️⃣1️⃣ Dynamic Attribute Rendering

```jsx
type={isShow ? "text" : "password"}
```

Only attribute updates, input element is not recreated.

---

# 1️⃣2️⃣ preventDefault()

```js
e.preventDefault();
```

Used inside:

```jsx
<form onSubmit={handleSubmit}>
```

Prevents page reload and state loss.

---

# 1️⃣3️⃣ Component Lifecycle

## 🟢 Mounting
- Function runs  
- useState initializes  
- JSX returned  
- DOM created  

## 🟡 Updating
Triggered by:
- useState  
- useReducer  
- useContext change  
- Parent re-render  
- Prop change  

## 🔴 Unmounting
Cleanup from useEffect runs.

---

# 1️⃣4️⃣ What Happens When useState Updates?

1. React schedules update  
2. Component function re-runs  
3. Virtual DOM recreated  
4. Diffing occurs  
5. Minimal DOM updates  

---

# 1️⃣5️⃣ Does Entire App Re-render?

No.

Only:
- Component where state changed  
- Its children  

---

# 1️⃣6️⃣ Virtual DOM vs Real DOM

React compares:

Old:
```html
<h1>0</h1>
```

New:
```html
<h1>1</h1>
```

Only `textContent` updates.

---

# 1️⃣7️⃣ Re-Running vs Re-Creating DOM

| Phase              | What Happens               |
|------------------|--------------------------|
| Component function | Runs fully               |
| Virtual DOM        | Recreated               |
| Diffing            | Compared                |
| Real DOM           | Minimal updates         |

---

# 1️⃣8️⃣ Parent Re-render Example

```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <Header />
      <Counter count={count} />
      <Footer />
    </div>
  );
}
```

Unless optimized (React.memo, useMemo, useCallback), all children re-render.

---

# 1️⃣9️⃣ What Triggers Re-render?

Triggers:
- useState  
- useReducer  
- useContext change  
- Parent re-render  
- Prop change  

Does NOT trigger:
- useRef change  
- Normal variable change  
- Mutating state without new reference  

---

# 2️⃣0️⃣ Same State Value Case

```js
setCount(0);
```

React uses:

```
Object.is(oldValue, newValue)
```

If same → no re-render.

---

# 2️⃣1️⃣ Hook Storage Mental Model

React internally stores hooks in order:

```
Slot 1 → input
Slot 2 → todos
Slot 3 → nextId
```

Order must not change.

---

# 🎯 Final Mental Model

On every state change:

✔ Component function runs  
✔ JSX recreated  
✔ Virtual DOM rebuilt  
✔ Diffing happens  
✔ Only minimal Real DOM updates  

---

# 🔥 Interview-Level Summary

- Arrow function → pass reference, not execute  
- Functional update → avoid stale state  
- Spread operator → create new reference  
- No mutation → shallow comparison  
- key → efficient reconciliation  
- Mount → first render  
- Update → state change  
- Unmount → cleanup  
- Whole function runs  
- Real DOM minimally updated  

---

## 🚀 Next Level Topics

- React Fiber deep dive  
- Hook internal implementation  
- Batching & concurrent rendering  
- Advanced performance optimization  

---

⭐ If this helped you, consider starring your repository!
