# 📘 JSX Notes (React)

Clear, structured notes on JSX written in an exam-friendly format.

---

# 1️⃣ What is JSX?

- **JSX stands for JavaScript XML**
- It allows us to **write HTML inside JavaScript**
- Used in **React** to describe UI
- Browsers do NOT understand JSX directly
- JSX is converted to `React.createElement()` by Babel

### Example:

```jsx
const element = <h1>Hello World</h1>;
```

Equivalent JavaScript:

```js
React.createElement("h1", null, "Hello World");
```

---

# 2️⃣ Expressions in JSX

- JSX allows **JavaScript expressions** inside `{ }`
- Anything inside curly braces is treated as JavaScript

### Example:

```jsx
const name = "Ulka";
<h1>Hello {name}</h1>
```

You can use:

- Variables
- Arithmetic
- Function calls
- Ternary operators

```jsx
<h1>{5 + 5}</h1>
```

---

# 3️⃣ Inserting a Large Block of HTML

To write HTML on multiple lines:

- Wrap it inside **parentheses ( )**

### Example:

```jsx
const element = (
  <div>
    <h1>Title</h1>
    <p>This is a paragraph.</p>
  </div>
);
```

This improves readability.

---

# 4️⃣ One Top Level Element

JSX must return **only ONE parent element**.

❌ Wrong:

```jsx
return (
  <h1>Hello</h1>
  <p>World</p>
);
```

✅ Correct:

```jsx
return (
  <div>
    <h1>Hello</h1>
    <p>World</p>
  </div>
);
```

---

# 5️⃣ React Fragment (`<> </>`)

Instead of adding extra `<div>`, we use a **Fragment**.

A fragment looks like an empty HTML tag:

```jsx
<>
  <h1>Hello</h1>
  <p>World</p>
</>
```

- Does NOT create extra DOM node
- Cleaner structure

---

# 6️⃣ Elements Must Be Closed

All JSX elements must be properly closed.

### Self-closing tags:

```jsx
<img src="image.jpg" />
<input />
<br />
```

❌ Wrong:

```jsx
<img src="image.jpg">
```

---

# 7️⃣ Attribute: `class` → `className`

In JSX:

- `class` is replaced by `className`
- Because `class` is a reserved JavaScript keyword

```jsx
<h1 className="title">Hello</h1>
```

---

# 📘 React JSX Expressions

JSX supports:

## ✅ 1. Expressions

```jsx
<h1>{10 * 2}</h1>
```

## ✅ 2. Variables

```jsx
const user = "Ulka";
<h1>{user}</h1>
```

## ✅ 3. Object Properties

```jsx
const person = { name: "Ulka", age: 20 };
<h1>{person.name}</h1>
```

---

# 📘 React JSX Attributes

---

## 1️⃣ class → className

```jsx
<div className="container"></div>
```

---

## 2️⃣ Expressions as Attributes

Use `{}` inside attributes:

```jsx
const imgUrl = "photo.jpg";
<img src={imgUrl} />
```

---

## 3️⃣ camelCase Event Attributes

HTML:

```html
onclick
```

JSX:

```jsx
onClick
```

Example:

```jsx
<button onClick={handleClick}>Click Me</button>
```

Other examples:

- onChange
- onSubmit
- onMouseOver

---

## 4️⃣ The style Attribute

In JSX:

- Style is written as a JavaScript object
- CSS properties use camelCase

```jsx
<h1 style={{ color: "red", backgroundColor: "yellow" }}>
  Hello
</h1>
```

Notice:

- Double curly braces `{{ }}`
- Outer `{}` → JavaScript
- Inner `{}` → Object

---

# 📘 React JSX If Statements

---

## Important Rule:

React supports **if statements**, but NOT directly inside JSX.

❌ Wrong:

```jsx
<h1>
  if (isLoggedIn) { "Welcome" }
</h1>
```

---

## ✅ Method 1: Use if outside JSX

```jsx
let message;

if (isLoggedIn) {
  message = <h1>Welcome</h1>;
} else {
  message = <h1>Please Login</h1>;
}

return message;
```

---

## ✅ Method 2: Ternary Operator (Most Common)

```jsx
<h1>{isLoggedIn ? "Welcome" : "Please Login"}</h1>
```

---

## ✅ Method 3: Logical AND (&&)

Used when you only need to check one condition:

```jsx
{isLoggedIn && <h1>Welcome</h1>}
```

---

# 🎯 Quick Revision Summary

- JSX = JavaScript + HTML
- Use `{}` for expressions
- Only one parent element
- Use Fragment `<> </>`
- Use `className` instead of `class`
- Events use camelCase
- style is an object
- if statements must be outside JSX
- Use ternary for conditional rendering
