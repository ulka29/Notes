# ⚛️ `useState` – Complete Practical + Advanced Notes

> Covers **state updates, event handling, mutation rules, arrays, lists, patterns, performance, and edge cases.**

---

# 1️⃣ Basic Syntax

```js
const [state, setState] = useState(initialValue);
```

Returns:

* Current state value
* Setter function

Example:

```js
const [count, setCount] = useState(0);
```

Explanation:

* `count` → current value
* `setCount` → function that updates the value
* `0` → initial state value

---

# 2️⃣ Lazy Initial State (Performance Optimization) 🔥

If the initial value requires **expensive computation**, use a function.

### ❌ Normal Initialization

```js
const [value, setValue] = useState(expensiveFunction());
```

Problem:

* `expensiveFunction()` runs **every render**

---

### ✅ Lazy Initialization

```js
const [value, setValue] = useState(() => expensiveFunction());
```

Now:

* Function runs **only on first render**

---

# 3️⃣ Arrow Functions in Event Handlers

React expects:

```
function reference
```

Example:

```jsx
onChange={handleChange}
```

---

## ❌ Wrong

```jsx
onChange={setInput(e.target.value)}
```

Runs during render and causes errors.

---

## ✅ Correct

```jsx
onChange={(e) => setInput(e.target.value)}
```

---

## Named Handler

```jsx
const handleChange = (e) => {
  setInput(e.target.value);
};

<input onChange={handleChange} />
```

---

### Internal Flow

```
Component renders
↓
React attaches handler
↓
User triggers event
↓
Handler executes
↓
setState called
↓
React schedules re-render
↓
Component function runs again
```

---

# 4️⃣ Functional Updates (`prev`)

React state updates are:

* Asynchronous
* Batched

---

## ❌ Problem

```js
setCount(count + 1);
setCount(count + 1);
```

Expected:

```
+2
```

Actual:

```
+1
```

Reason:

Both updates use the **same stale value**.

---

## ✅ Correct

```js
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Now:

```
prev = 0 → 1
prev = 1 → 2
```

---

# 5️⃣ Automatic Batching (React 18+) 🔥

React groups multiple state updates into **one render cycle**.

Example:

```js
setCount(c => c + 1);
setUser("Ulka");
setTheme("dark");
```

React performs:

```
1 re-render instead of 3
```

Improves performance.

---

# 6️⃣ Professional Update Patterns

---

## Direct Update

Used when value **does not depend on previous state**.

```js
setInput("Hello");
```

---

## Functional Update

Used when **state depends on previous value**.

```js
setCount(prev => prev + 1);
```

---

## Object Update

```js
setUser(prev => ({
  ...prev,
  name: "Ulka"
}));
```

Why spread?

Without spread:

```js
setUser({ name: "Ulka" });
```

Other properties are lost.

Example state:

```
{
  name: "John",
  age: 25
}
```

After update:

```
{
  name: "Ulka"
}
```

Age disappears.

---

## Array Update

```js
setTodos(prev => [...prev, newTodo]);
```

Creates new array reference.

---

# 7️⃣ Why State Mutation Is Wrong

❌ Wrong:

```js
prev.push(newTodo);
```

React checks changes using:

```
oldReference !== newReference
```

If reference is same → React assumes nothing changed.

---

## ✅ Correct

```js
[...prev, newTodo]
```

New reference triggers re-render.

---

# 8️⃣ Immutable Update Patterns (Important)

### Add Item

```js
setTodos(prev => [...prev, newTodo]);
```

---

### Remove Item

```js
setTodos(prev => prev.filter(todo => todo.id !== id));
```

---

### Update Item

```js
setTodos(prev =>
  prev.map(todo =>
    todo.id === id
      ? { ...todo, completed: true }
      : todo
  )
);
```

---

# 9️⃣ List Rendering with `map()`

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

# 🔟 Why `key` is Important

Without key:

* Elements matched by index
* Removing first shifts all items
* Many unnecessary re-renders

Example:

```
[A, B, C]
remove A
```

React thinks:

```
A → B
B → C
C → removed
```

Many updates occur.

---

With key:

```
A removed
B unchanged
C unchanged
```

Efficient update.

---

# 1️⃣1️⃣ Conditional Rendering

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

### Early Return Pattern

Cleaner UI logic.

```js
if (!isLoggedIn) {
  return <Login />;
}
```

---

# 1️⃣2️⃣ Dynamic Attribute Rendering

Example:

```jsx
type={isShow ? "text" : "password"}
```

Only attribute changes.

Input element is **not recreated**.

React only updates the **attribute**.

---

# 1️⃣3️⃣ preventDefault()

Used in forms.

```js
e.preventDefault();
```

Example:

```jsx
<form onSubmit={handleSubmit}>
```

Prevents:

* Page reload
* State reset
* Form submission redirect

---

# 1️⃣4️⃣ Multiple State vs Single State Object

### Option 1

```js
const [name, setName] = useState("");
const [age, setAge] = useState(0);
```

Better when states are **independent**.

---

### Option 2

```js
const [user, setUser] = useState({
  name: "",
  age: 0
});
```

Better when data is **related**.

---

# 1️⃣5️⃣ Derived State (Common Mistake)

❌ Wrong

```js
const [total, setTotal] = useState(price * quantity);
```

State becomes **out of sync**.

---

✅ Correct

```js
const total = price * quantity;
```

Derived values should be **computed**, not stored.

---

# 1️⃣6️⃣ When to Use `useReducer` Instead

If state logic becomes complex.

Example:

```
multiple fields
multiple actions
complex updates
```

Then prefer:

```
useReducer
```

---

# 1️⃣7️⃣ Debugging State Updates

Use React DevTools.

You can inspect:

* component state
* props
* re-renders

Helpful for debugging.

---

# 1️⃣8️⃣ Common Beginner Mistakes

### ❌ Mutating state

```
state.push()
```

---

### ❌ Using state immediately after update

```
setCount(1)
console.log(count)
```

Still old value.

---

### ❌ Missing key in list

```
.map()
```

---

### ❌ Updating state in render

```js
setCount(1)
```

Causes infinite loop.

---

# 🎯 Final Mental Model of `useState`

When `setState` runs:

```
setState called
↓
React schedules update
↓
Component function runs again
↓
JSX recreated
↓
Virtual DOM rebuilt
↓
Diffing compares old vs new
↓
Minimal Real DOM updates
```

---

# 🔥 Interview Summary

Key points:

* `useState` manages component state
* `setState` triggers re-render
* Updates are asynchronous
* Functional updates prevent stale state
* Never mutate state
* Always create new references
* Keys enable efficient list rendering
* React batches state updates
* Lazy initialization improves performance

---

💡 **Suggestion for your React repo**

Add this structure:

```
React-Deep-Notes
│
├── 01-react-hooks-system.md
├── 02-usestate-deep-guide.md
├── 03-useeffect-complete.md
├── 04-useref-guide.md
├── 05-react-rendering-system.md
├── 06-react-performance.md
```

---

If you want, I can also give you the **🔥 next note: `useEffect` Complete Deep Guide (20x more advanced than typical tutorials)** covering:

* dependency array deep logic
* cleanup functions
* race conditions
* API fetching patterns
* strict mode double execution
* real interview traps.

Just tell me.
