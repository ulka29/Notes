Perfect — here is a **clean, combined, concise version** covering:

* ✅ Module vs Component scope
* ✅ Ideal structure
* ✅ Calling outside functions
* ✅ How export + scope works

Clear. Architectural. Interview-ready.

---

# 🧠 React Component Structure + Scope (Clean Notes)

---

# 1️⃣ The Three Scopes in a React File

```jsx
// 🌍 MODULE SCOPE (outside component)

import ...
const CONSTANT = ...
function helper() {}

export default function MyComponent() {

  // 🧠 COMPONENT SCOPE

  const state = ...
  const derived = ...

  return (
    // 🎨 JSX
  );
}
```

---

## 🌍 Module Scope (Outside Component)

Everything outside the component function.

### 🔥 Runs:

* ✅ Once when file loads
* ❌ Not on every render

### ✅ Put Here:

* Static constants
* Utility/helper functions
* Static data
* Config values
* Custom hook definitions

Example:

```js
const TAX_RATE = 0.1;

function calculateTotal(price) {
  return price * 1.1;
}
```

---

## 🧠 Component Scope (Inside Function)

Runs on every render.

### ✅ Ideal Order

```text
1️⃣ Hooks
2️⃣ Derived values
3️⃣ Event handlers
4️⃣ Guard clauses
5️⃣ JSX return
```

Example:

```jsx
export default function Product() {

  const [price, setPrice] = useState(100);

  const total = calculateTotal(price);

  function increase() {
    setPrice(p => p + 10);
  }

  if (price < 0) return <p>Invalid</p>;

  return (
    <div>
      <h1>{price}</h1>
      <h2>{total}</h2>
      <button onClick={increase}>Increase</button>
    </div>
  );
}
```

---

# 2️⃣ Calling a Function Defined Outside

### ✅ Does it work?

Yes.

```jsx
function calculateTotal(price) {
  return price * 1.1;
}

export default function Product() {
  const [price] = useState(100);
  const total = calculateTotal(price);
  return <h1>{total}</h1>;
}
```

### Why?

Because of **JavaScript lexical scoping**.

A function can access anything defined in its outer (module) scope.

Exporting the component:

```js
export default function Product() {}
```

does NOT remove access to module-scope functions.
It only exports the component reference — not the whole file.

---

# 3️⃣ When to Put Functions Outside vs Inside

### 🌍 Put Outside If:

* It does NOT depend on component state via closure
* It is pure (no side effects)
* It can receive data as arguments

### 🧠 Keep Inside If:

* It relies directly on component state via closure
* It is tightly coupled to that component

---

# 4️⃣ Execution Model Clarity

When state updates:

```js
setState(...)
```

React:

* Re-runs the component function
* Recreates variables and inner functions
* Re-evaluates JSX

But:

* Code outside component does NOT re-run

That’s the architectural difference.

---

# 🎯 Final Mental Model

🌍 Outside = reusable tools (runs once)
🧠 Inside = state-aware render logic (runs every render)
🎨 Return = pure UI declaration

---

# 🎤 Interview Summary

> A React file has module scope and component scope. Code outside the component runs once and should contain static or reusable logic. Code inside runs on every render and should contain hooks and state-dependent logic. Components can safely call functions defined outside because of JavaScript’s lexical scoping, and this is good practice when the function is pure and state-independent.

---

If you want next depth:

* Stale closures
* `useCallback`
* Multiple component instances + module scope
* Deep render cycle

Pick your next level.
