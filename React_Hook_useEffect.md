# React Hook: `useEffect`

## What is `useEffect`?

`useEffect` is a **React Hook used to handle side effects in functional components**.

In React, components are primarily responsible for **rendering UI**. However, many operations in an application involve interacting with **systems outside the rendering process**. These operations are called **side effects**.

The `useEffect` hook allows developers to **perform such operations after the component has rendered**.

Therefore, `useEffect` helps manage tasks that need to occur **after the UI is updated in the DOM**.

---

# What is a “Side Effect”?

A **side effect** is any operation performed by a component that **interacts with systems outside of the component itself**.

In simple terms, it is an operation that:

* Affects something outside the component
* Executes **after rendering**
* Is not purely related to **calculating UI**

React components ideally should **only describe what the UI should look like**.

Any additional work that interacts with external systems is considered a **side effect**.

---

# Why `useEffect` is Important

Without `useEffect`, developers would face difficulties performing operations that must occur **after rendering**.

For example:

* API calls might execute during rendering
* Event listeners might be attached repeatedly
* Timers might never be cleaned up

`useEffect` ensures that these operations occur in a **controlled and predictable manner**, keeping the component rendering process **clean and pure**.

---

# Purpose of `useEffect`

`useEffect` is commonly used for performing operations such as:

* Fetching data from APIs
* Subscribing to external data sources
* Managing timers (`setTimeout`, `setInterval`)
* Adding or removing event listeners
* Updating the browser document title
* Reading from or writing to `localStorage`
* Interacting with the DOM

These operations **cannot be handled directly during rendering** because rendering should remain **pure and predictable**.

---

# Examples of Side Effects

Common examples of side effects in React applications include:

* Making API requests to retrieve data
* Starting or clearing timers
* Registering event listeners
* Updating the document title
* Reading or writing local storage
* Subscribing to WebSocket or real-time services

These operations typically occur **after a component renders**, which is why they are handled using `useEffect`.

---

# Basic Syntax of `useEffect`

```javascript
useEffect(() => {
  // Side effect logic
});
```

The function passed to `useEffect` is called the **effect function**.

React executes this function **after the component renders**.

---

# Key Characteristics

* Runs **after the component renders**
* Used for **side effects**
* Can run **once**, **on updates**, or **on every render**
* Supports **cleanup logic**
* Replaces lifecycle methods in **class components**

---

# Basic Syntax with Dependency Array

```javascript
useEffect(() => {
  // side effect logic
}, [dependencies]);
```

It takes:

1. **Effect function (callback)**
2. **Dependency array (optional but critical)**

---

# When Does `useEffect` Run?

## Case 1: No Dependency Array

```javascript
useEffect(() => {
  console.log("Runs after every render");
});
```

Runs:

* After first render
* After every re-render

⚠️ Dangerous if it updates state → **infinite loop**

---

## Case 2: Empty Dependency Array

```javascript
useEffect(() => {
  console.log("Runs once");
}, []);
```

Runs:

* Only once (after first render)

Equivalent to:

```
componentDidMount()
```

---

## Case 3: With Dependencies

```javascript
useEffect(() => {
  console.log("Runs when count changes");
}, [count]);
```

Runs:

* After first render
* Whenever `count` changes

---

# The Execution Timeline (Very Important)

Example:

```javascript
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Effect runs");
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

On click:

1. `setCount` called
2. React schedules update
3. Component re-runs
4. Virtual DOM updated
5. Real DOM updated
6. 🔥 **THEN `useEffect` runs**

👉 Effects run **after paint (after DOM updates)**.

---

# Cleanup Function (Critical Concept)

`useEffect` can return a function.

```javascript
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Running");
  }, 1000);

  return () => {
    clearInterval(timer);
  };
}, []);
```

The returned function is called the **cleanup function**.

---

# When Cleanup Runs

Cleanup runs:

1. **Before the next effect runs**
2. **When the component unmounts**

---

# Example with Dependency

```javascript
useEffect(() => {
  console.log("Effect runs");

  return () => {
    console.log("Cleanup runs");
  };
}, [count]);
```

If `count` changes:

1. Cleanup runs first
2. Then new effect runs

---

# Infinite Loop Problem

❌ Example:

```javascript
useEffect(() => {
  setCount(count + 1);
});
```

What happens:

* Effect runs
* `setCount` triggers re-render
* Effect runs again
* **Infinite loop**

---

# Fix

```javascript
useEffect(() => {
  setCount(prev => prev + 1);
}, []);
```

Now it runs **only once**.

---

# Dependency Array Rules

You must include:

* All **state variables used**
* All **props used**
* All **functions defined outside the effect**

Example:

```javascript
useEffect(() => {
  console.log(count);
}, [count]);
```

---

# Why?

React compares dependency values using:

```
Object.is()
```

If value changed → effect runs.

---

# Objects & Arrays in Dependencies (Common Mistake)

```javascript
useEffect(() => {
  console.log(user);
}, [user]);
```

If `user` is recreated every render:

```javascript
const user = { name: "John" };
```

Effect runs every time because:

```
oldReference !== newReference
```

---

# Solution

Use `useMemo` or move the object outside the component.

---

# Data Fetching Pattern

```javascript
useEffect(() => {
  async function fetchData() {
    const response = await fetch("/api/data");
    const data = await response.json();
    setData(data);
  }

  fetchData();
}, []);
```

### Why not make effect async?

❌ Wrong:

```javascript
useEffect(async () => {});
```

Because an effect must return:

* Nothing
* OR a cleanup function

Not a **Promise**.

---

# Real-World Patterns

## 1️⃣ Fetch on Mount

```javascript
useEffect(() => {
  fetchData();
}, []);
```

---

## 2️⃣ Sync with LocalStorage

```javascript
useEffect(() => {
  localStorage.setItem("count", count);
}, [count]);
```

---

## 3️⃣ Add Event Listener

```javascript
useEffect(() => {
  const handleResize = () => console.log(window.innerWidth);

  window.addEventListener("resize", handleResize);

  return () => {
    window.removeEventListener("resize", handleResize);
  };
}, []);
```

---

# Multiple `useEffect` Calls

You can use multiple effects in one component.

```javascript
useEffect(() => {
  console.log("Runs for count");
}, [count]);

useEffect(() => {
  console.log("Runs for name");
}, [name]);
```

This improves **separation of concerns**.

---

# Order of Execution

If multiple effects exist:

* They run **in the order they are declared**
* Cleanup runs **in reverse order**

---

# `useEffect` vs `useLayoutEffect`

| useEffect        | useLayoutEffect   |
| ---------------- | ----------------- |
| Runs after paint | Runs before paint |
| Non-blocking     | Blocking          |
| Most common      | Rare use          |

Use `useLayoutEffect` when:

* Measuring DOM
* Preventing UI flicker

---

# Strict Mode Behavior (Important)

In **development mode under Strict Mode**:

Effects run **twice**.

Why?

To detect **side-effect bugs**.

Production mode **does NOT double run**.

---

# Mental Model of `useEffect`

Think of it as:

> **“After React updates the DOM, run this code.”**

Not:

> “Run during rendering.”

Rendering must stay **pure**.

Effects run **after rendering**.

---

# Full Lifecycle Mapping

| Class Lifecycle      | useEffect Equivalent   |
| -------------------- | ---------------------- |
| componentDidMount    | useEffect(..., [])     |
| componentDidUpdate   | useEffect(..., [deps]) |
| componentWillUnmount | cleanup function       |

---

# Common Interview Questions

### When does `useEffect` run?

After render and after DOM update.

---

### Why does infinite loop happen?

Because effect updates state **without proper dependencies**.

---

### Why is dependency array important?

It **controls when the effect re-runs**.

---

### Can we use multiple `useEffect` hooks?

Yes.

It is recommended for **separating logic**.

---

### Does `useEffect` block the UI?

No.

It runs **after paint**.

---

# Complete Execution Flow Example

## Initial Render

1. Component runs
2. JSX returned
3. DOM updated
4. `useEffect` runs

---

## On State Change

1. `setState` called
2. Component re-runs
3. DOM updated
4. Cleanup runs
5. New effect runs

---

# Final Mental Model

On every dependency change:

✔ Component renders
✔ Virtual DOM created
✔ Real DOM updated
✔ Cleanup runs (if exists)
✔ Effect runs

---

# Golden Rules

* Never update state blindly inside an effect
* Always declare dependencies correctly
* Cleanup subscriptions and timers
* Effects run **after paint**
* Dependency array **controls execution**

---

# Master-Level Understanding

`useEffect` is:

* A **post-render lifecycle handler**
* Controlled by **dependency array**
* Cleaned via **return function**
* Compared using **Object.is**
* Runs after **DOM commit**

---

# Next Advanced Topics

If you want to go deeper into React effects:

* Advanced dependency edge cases
* Race conditions in data fetching
* `AbortController` pattern
* `useEffect` in concurrent rendering
* Real production patterns

---

If you want, I can also show you how to turn this into a **🔥 professional GitHub README (with diagrams, flow charts, and visual lifecycle graphs)** so it looks **senior-level documentation instead of notes.**
