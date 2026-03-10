# 📘 React Unmounting — Complete Notes

---

# 1️⃣ What is Unmounting?

**Unmounting** means:

> A component is removed from the UI (DOM) because it is no longer rendered in the component tree.

It happens when:

* A conditional render becomes false
* A route changes
* A parent component stops rendering it

Example:

```jsx
{show && <Child />}
```

If `show` becomes `false`, `<Child />` is removed → **unmounted**.

---

# 2️⃣ React Component Lifecycle (Function Components)

In modern React (with hooks), lifecycle is:

1. Mount
2. Update
3. Unmount

### Mount

Component appears in the DOM.

### Update

Component re-renders due to:

* State change
* Props change
* Parent re-render

### Unmount

Component is removed from DOM.

---

# 3️⃣ What Happens During Unmount?

When React decides to remove a component:

1. React builds new Virtual DOM
2. Compares with previous Virtual DOM (Reconciliation)
3. Detects component is missing
4. Marks it for deletion
5. Runs all cleanup functions from `useEffect`
6. Removes it from real DOM
7. Releases memory

---

# 4️⃣ Relationship Between Unmounting and `useEffect`

Inside `useEffect`, you can return a cleanup function:

```js
useEffect(() => {
  console.log("Effect started");

  return () => {
    console.log("Cleanup runs");
  };
}, []);
```

The function returned from `useEffect` is called:

> **Cleanup function**

---

# 5️⃣ When Does Cleanup Run?

Cleanup runs in two situations:

### 1️⃣ Before the effect runs again

(When dependencies change)

### 2️⃣ Before component unmounts

Even if dependency array is empty `[]`, cleanup still runs during unmount.

---

# 6️⃣ What Does `[]` Mean?

```js
useEffect(() => {}, []);
```

The empty dependency array means:

> Run the effect only once after mount.

It does NOT mean:

> Ignore unmounting.

Unmount cleanup still runs.

---

# 7️⃣ Internal Mechanism (How React Knows It’s Unmounting)

React uses a process called **Reconciliation**.

When state or props change:

* React creates a new Virtual DOM
* Compares it with old Virtual DOM
* If a component is missing → React detects unmount
* Before deleting it → React executes stored cleanup functions

React stores the cleanup function internally like this:

```
Component Instance → Cleanup Function Reference
```

When component is removed, React calls that stored function.

Dependencies are NOT used to detect unmount.

---

# 8️⃣ Example Timeline (With `[]`)

```js
useEffect(() => {
  console.log("Effect ran");

  return () => {
    console.log("Cleanup ran");
  };
}, []);
```

Lifecycle:

```
Mount
↓
Effect runs once
↓
(Component exists normally)
↓
Component removed
↓
Cleanup runs
```

---

# 9️⃣ Why Cleanup is Important

Cleanup prevents:

* Memory leaks
* Running intervals after component removal
* Event listeners staying active
* State updates on removed components
* WebSocket leaks
* API subscription leaks

Example:

```js
useEffect(() => {
  const id = setInterval(() => {
    setCount(c => c + 1);
  }, 1000);

  return () => clearInterval(id);
}, []);
```

Without cleanup:

* Interval continues running
* React may warn:

  > Can't perform a React state update on an unmounted component

---

# 🔟 What If No Cleanup is Returned?

```js
useEffect(() => {
  console.log("Effect started");
}, []);
```

During unmount:

* Nothing runs
* No cleanup
* Safe only if no side effects exist

---

# 1️⃣1️⃣ Important Clarification

There are two different `return`s in a component:

### Component Return

```js
return <h1>Hello</h1>;
```

* Renders UI
* Not related to unmount

### useEffect Return

```js
return () => {};
```

* Cleanup function
* Runs during unmount
* Related to unmount

---

# 1️⃣2️⃣ Practical Situations Where Unmount Happens

* Toggling component visibility
* Changing routes
* Conditional rendering
* Parent component unmounting
* Key prop change in list

---

# 1️⃣3️⃣ Short Exam Definition

> Unmounting is the process where React removes a component from the DOM. Before removal, React executes cleanup functions returned from useEffect to prevent memory leaks and unwanted side effects.

---

# 1️⃣4️⃣ Interview-Level Insight

Unmounting is detected during React’s reconciliation process when a component no longer exists in the new Virtual DOM tree. React then executes stored cleanup functions before removing the component from the real DOM.

---

# 🔥 Final Core Concept

Dependencies control:

> When effect re-runs.

Unmount detection is handled by:

> React's reconciliation algorithm.

Cleanup runs:

> Before re-run and before unmount.

---

If you want next, I can give:

* Advanced notes on reconciliation
* StrictMode behavior explanation
* Common unmount-related interview traps
* Visual diagram of lifecycle

You’re now understanding React at architecture level.
