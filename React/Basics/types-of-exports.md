# JavaScript Module System – Types of Exports

In modern JavaScript (ES6+), we use **modules** to split code into multiple files.

There are **two main types of exports**:

1. **Default Export**
2. **Named Export**

Understanding the difference is very important in React projects.

---

# 1️⃣ Default Export

A file can have **only ONE default export**.

## ✅ Syntax

```js
export default function Settings() {
  return "Settings Page";
}
```

OR

```js
const Settings = () => {
  return "Settings Page";
};

export default Settings;
```

---

## ✅ Importing Default Export

```js
import Settings from "./Settings";
```

⚠ No curly braces `{}` used.

---

## 🧠 Key Rules

* Only one default export per file
* Import name can be anything

Example:

```js
import MyPage from "./Settings";
```

This still works because it’s default export.

---

## 🎯 When to Use Default Export

Use default export when:

* The file represents **one main thing**
* React page components
* Layout components
* Single utility function files

### Industry Standard:

Most React components are exported as default.

---

# 2️⃣ Named Export

A file can have **multiple named exports**.

## ✅ Syntax

```js
export function Users() {
  return "Users Page";
}

export function Settings() {
  return "Settings Page";
}
```

OR

```js
export const name = "Ulka";
export const age = 21;
```

---

## ✅ Importing Named Export

```js
import { Users, Settings } from "./file";
```

⚠ Must use `{}` and exact names.

---

## 🧠 Key Rules

* Can export multiple things
* Import name must match exactly
* Curly braces required

---

## 🎯 When to Use Named Export

Use named export when:

* File contains multiple utilities
* Helper functions
* Constants
* Multiple reusable components
* Service files (API, auth, etc.)

Example:

```js
export const login = () => {};
export const logout = () => {};
export const register = () => {};
```

---

# 🔄 Mixing Both (Allowed)

You can combine both in one file:

```js
export const helper = () => {};
export default function MainComponent() {}
```

Import:

```js
import MainComponent, { helper } from "./file";
```

---

# ⚠ Common Errors

## ❌ Error:

```
does not provide an export named 'default'
```

Meaning:

You imported like this:

```js
import Settings from "./Settings";
```

But file had:

```js
export function Settings() {}
```

That is a named export, not default.

---

# 📊 Quick Comparison Table

| Feature                 | Default Export | Named Export     |
| ----------------------- | -------------- | ---------------- |
| How many per file?      | Only one       | Multiple allowed |
| Curly braces needed?    | ❌ No           | ✅ Yes            |
| Import name must match? | ❌ No           | ✅ Yes            |
| Common in React pages?  | ✅ Yes          | Sometimes        |
| Best for utilities?     | ❌ No           | ✅ Yes            |

---

# 🧠 Mental Model

Think like this:

### Default Export → "This file is about ONE main thing"

Example:

* Login.jsx
* Dashboard.jsx
* Layout.jsx

---

### Named Export → "This file contains MANY things"

Example:

* utils.js
* api.js
* constants.js

---

# 🎯 Final Simple Rule

* Use **default export** for main React components.
* Use **named export** for multiple helpers, utilities, or constants.
* Import must always match export type.

---

If you want next, I can also make notes on:

* Common import/export mistakes
* Barrel files (`index.js`)
* How large projects structure exports
* Tree shaking concept

Since you're preparing for industry-level projects, mastering this concept is very important.
