# 🧠 React & JSX — Structured Notes

---

## 1️⃣ A React File Is Just a JavaScript Module

A React file is **not special syntax**. It is simply:

```
JavaScript (ES Modules)
+ React APIs (hooks, components)
+ Optional JSX
```

Example:

```js
export function useCounter() {}
```

This is **pure JavaScript** using ES Module syntax.

React does not modify JavaScript itself.

---

## 2️⃣ What JSX Actually Is

### Definition

JSX is:

> JavaScript syntax that looks like HTML and compiles to `React.createElement()` calls.

Example:

```jsx
<h1>Hello</h1>
```

Compiles to:

```js
React.createElement("h1", null, "Hello");
```

So JSX is **syntactic sugar** over JavaScript function calls.

---

## 3️⃣ JSX Is Determined by Syntax (Not Location)

❌ JSX is NOT determined by being inside `return()`.

✅ JSX is determined by syntax.

If your code contains:

```jsx
<Tag />
<div></div>
<Component />
```

→ That is JSX.

It does not matter where it appears.

---

## 4️⃣ JSX Outside `return()` (Still JSX)

```jsx
const element = <h1>Hello</h1>;

const list = [<li>A</li>, <li>B</li>];
```

These are valid JSX expressions.

JSX is just an expression that evaluates to a React element.

---

## 5️⃣ What Is NOT JSX

These are all plain JavaScript:

```js
const count = 5;

useState(0);

export default function App() {}

if (count > 5) {
  console.log("High");
}
```

Hooks, exports, logic, and conditionals are all normal JS.

---

## 6️⃣ Hooks and JSX

Hooks:

```js
useState()
useEffect()
useMemo()
```

* Are normal JavaScript function calls
* Are not JSX
* Do not create UI

They manage logic and state.

---

## 7️⃣ Custom Hooks

```js
export function useFetch() {}
```

This is still:

* A normal JavaScript function
* Following React rules
* Using React hooks

There is no JSX unless it returns something like:

```jsx
return <div>Hello</div>;
```

But custom hooks are meant for **logic**, not UI.

---

## 8️⃣ Minimal Complete Example

```jsx
import { useState, useEffect } from "react";

// Custom Hook (Logic only)
export function useCounter(initial = 0) {
  const [count, setCount] = useState(initial);

  useEffect(() => {
    console.log("Count changed:", count);
  }, [count]);

  return { count, setCount };
}

// Component (Contains JSX)
export default function App() {
  const { count, setCount } = useCounter(0);

  const double = count * 2; // Normal JS

  const message = <p>Current count is {count}</p>; // JSX

  return (
    <div>
      {message}
      <h1>{double}</h1>
      <button onClick={() => setCount(count + 1)}>
        Increase
      </button>
    </div>
  );
}
```

Notice:

* JSX exists because `<p>`, `<div>`, `<h1>`, `<button>` exist.
* It is not about `return()`.
* It is about syntax.

---

## 9️⃣ Clean Mental Model

A React file contains:

### 1️⃣ JavaScript Module System

```js
import / export
```

### 2️⃣ JavaScript Logic

```js
variables
functions
conditionals
loops
```

### 3️⃣ React APIs (Still JavaScript)

```js
useState
useEffect
useMemo
```

### 4️⃣ JSX (Optional)

```jsx
<div />
<Component />
```

JSX is simply syntax inside JavaScript.

---

## 🔟 Final Rule (Precise Version)

JSX is determined by syntax, not location.

If your code contains HTML-like tags inside JavaScript → it is JSX.

If it does not → it is normal JavaScript.

---

# 🎯 Interview-Ready Summary

**Question:**
Is JSX decided by `return()`?

**Correct Answer:**

> No. JSX is determined by syntax — any HTML-like tag inside JavaScript is JSX, whether it is inside `return`, assigned to a variable, or passed as an argument.

---

If you'd like, I can now create structured notes on:

* Why component names must start with capital letters
* How React distinguishes `<div>` vs `<MyComponent>`
* How JSX becomes Virtual DOM
* Why `if` cannot be used directly inside JSX

Tell me which topic you want next.
