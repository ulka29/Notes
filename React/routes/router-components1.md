# React Router – `Link`, `Outlet`, and `useNavigate`

This document explains how `Link`, `Outlet`, and `useNavigate` work in real-world React Router projects.

---

# 1️⃣ `Link` in React Router

`Link` is a **client-side navigation component**.

Instead of using:

```html
<a href="/dashboard">Dashboard</a>
```

You should use:

```jsx
<Link to="/dashboard">Dashboard</Link>
```

---

## 🔎 Why Not Use `<a>`?

Using `<a>`:

* Reloads the entire page
* Breaks SPA (Single Page Application) behavior
* Resets React state

Using `Link`:

* Changes the URL without reloading
* Preserves application state
* Uses React Router internally for navigation

---

## 🔄 How `Link` Works

When you click:

```jsx
<Link to="/dashboard">Dashboard</Link>
```

React Router:

1. Updates the URL
2. Matches the corresponding route
3. Renders the correct component
4. Without refreshing the page

This is called **client-side routing**.

---

# 2️⃣ `Outlet` in React Router

`Outlet` is used for **nested routes**.

It acts as a placeholder where child route components will render.

Think of it as:

> “Render whatever child route belongs here.”

---

## 📌 Example Route Structure

```jsx
createBrowserRouter([
  {
    path: "/",
    element: <MainLayout />,
    children: [
      { path: "/", element: <Home /> },
      { path: "/dashboard", element: <Dashboard /> }
    ]
  }
])
```

Inside `MainLayout`:

```jsx
<Outlet />
```

### What This Means:

* When URL is `/` → `<Home />` renders inside `<Outlet />`
* When URL is `/dashboard` → `<Dashboard />` renders inside `<Outlet />`

---

## 🔵 Visual Understanding

### Without `Outlet`

```
MainLayout
  └── nav
  (nothing renders here)
```

### With `Outlet`

```
MainLayout
  ├── nav
  └── Home or Dashboard renders here
```

---

## 🧠 Industry Mental Model

```
MainLayout (Common Layout)
    ├── Navbar
    ├── Sidebar
    ├── Footer
    └── Outlet (dynamic page content)
```

The layout stays constant.
Only the content inside `<Outlet />` changes.

---

# 3️⃣ `useNavigate`

`useNavigate` is a React Router hook that allows navigation using JavaScript instead of clicking a `Link`.

```jsx
const navigate = useNavigate();
```

This gives you a `navigate()` function to change routes programmatically.

---

## 🔎 Why Do We Need `useNavigate`?

`Link` is used when users manually click something:

```jsx
<Link to="/users">Users</Link>
```

But sometimes navigation must happen after logic runs, such as:

* Login success
* Logout
* Form submission
* API response
* Permission check
* Timer expiry

In those cases, we use:

```jsx
navigate("/path");
```

---

# 📌 Example Code

```jsx
import { Outlet, Link, useNavigate } from "react-router-dom";
import { fakeAuth } from "../utils/fakeAuth";

export default function DashboardLayout() {
  const navigate = useNavigate();

  const handleLogout = () => {
    fakeAuth.logout();
    navigate("/login");
  };

  return (
    <>
      <h2>Dashboard</h2>

      <nav>
        <Link to="users">Users</Link> | 
        <Link to="settings">Settings</Link> |
        <button onClick={handleLogout}>Logout</button>
      </nav>

      <Outlet />
    </>
  );
}
```

---

## 🔄 What Happens on Logout?

1. User clicks **Logout**
2. `fakeAuth.logout()` runs
3. `navigate("/login")` redirects to `/login`
4. No page reload occurs (SPA behavior)

---

## 🔧 `navigate()` Usage

### Basic Navigation

```jsx
navigate("/login");
```

### Replace History (Important for Authentication)

```jsx
navigate("/login", { replace: true });
```

This prevents users from pressing the **Back button** and returning to a protected page after logout.

---

# 🧠 Mental Model Summary

| Method       | When to Use                            |
| ------------ | -------------------------------------- |
| `Link`       | When user clicks to navigate           |
| `navigate()` | When redirecting after logic or events |
| `Outlet`     | To render nested child routes          |

---

# 🎯 Final Simple Definitions

* **Link** → Navigates between routes without reloading the page.
* **Outlet** → Placeholder inside a parent route where child routes render.
* **useNavigate** → Allows programmatic navigation using JavaScript.

---

This pattern is fundamental in real-world React applications and industry-level projects.
