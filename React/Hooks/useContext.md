# ⚛️ React `useContext` – Deep System Notes

*(Extension of your notes — no content removed)*

---

# 1️⃣ Context Is NOT a State Manager

Many developers misunderstand Context.

Context is **not designed for complex state management**.

It is primarily a **value propagation system**.

### What Context Does

Context:

* Shares values across component tree
* Avoids prop drilling
* Notifies subscribed consumers when value changes

### What Context Does NOT Do

Context does **not**:

* Handle complex update logic
* Prevent unnecessary renders automatically
* Provide selective subscriptions
* Replace state libraries like Redux, Zustand, or Jotai

Think of it as:

```
Dependency Injection for React
```

---

# 2️⃣ Context Update vs Prop Update

Important conceptual difference.

### Prop Update

When parent re-renders:

```
Parent
  ↓
Child
  ↓
Grandchild
```

All children re-render **by default**.

Even if props are unchanged.

---

### Context Update

When context value changes:

```
Provider
   ↓
Subscribed Consumers
```

Only components **calling `useContext`** re-render.

Intermediate components are skipped.

---

# 3️⃣ Default Value in `createContext()`

```js
const UserContext = createContext(defaultValue);
```

This **default value is used only if no Provider exists**.

Example:

```
Component
   ↓
useContext(UserContext)
```

If no provider exists above:

```
defaultValue is returned
```

---

## Important Trap

If provider exists but value is `undefined`:

```jsx
<UserContext.Provider value={undefined}>
```

Consumer receives:

```
undefined
```

NOT the default value.

---

# 4️⃣ Context Scope (Per Provider Instance)

Each Provider instance creates a **separate context scope**.

Example:

```jsx
<UserContext.Provider value="A">
  <Component />
</UserContext.Provider>

<UserContext.Provider value="B">
  <Component />
</UserContext.Provider>
```

Result:

```
First Component → "A"
Second Component → "B"
```

Each subtree receives its **own context value**.

---

# 5️⃣ Nested Providers Override Parents

React always finds the **closest provider upward**.

Example:

```jsx
<UserContext.Provider value="Parent">
  <UserContext.Provider value="Child">
    <Avatar />
  </UserContext.Provider>
</UserContext.Provider>
```

Avatar receives:

```
"Child"
```

Because nearest provider overrides outer provider.

---

# 6️⃣ Context + `React.memo`

Important performance behavior.

Example:

```js
export default React.memo(MyComponent)
```

`React.memo` prevents **prop-based re-renders**.

But **context updates bypass memo**.

Example:

```
Context value changes
↓
Consumer component re-renders
↓
Even if wrapped in React.memo
```

Reason:

Context subscription is handled by **React Fiber**, not props comparison.

---

# 7️⃣ Context Value Must Be Stable

Bad example:

```jsx
<UserContext.Provider value={{ user }}>
```

Why bad?

Every render creates a **new object reference**.

```
{} !== {}
```

So consumers re-render every time.

---

### Correct Version

```js
const value = useMemo(() => ({ user }), [user]);

<UserContext.Provider value={value}>
```

Now reference changes **only when user changes**.

---

# 8️⃣ Context Re-render Behavior

If 20 components consume the same context:

```
Provider update
↓
20 components re-render
```

Even if they use **only a part of the value**.

Example:

```
value = { user, theme }
```

Component A uses `user`.

Component B uses `theme`.

If theme changes:

```
A re-renders
B re-renders
```

Context **does not support partial subscriptions**.

---

# 9️⃣ Selective Subscription Problem

Large apps need **granular updates**.

Solutions:

### 1. Split contexts

```
UserContext
ThemeContext
AuthContext
```

---

### 2. Context Selector Libraries

Example:

```
use-context-selector
```

Allows selecting only part of context.

---

### 3. External Store Pattern

React 18 introduced:

```
useSyncExternalStore
```

Used by libraries like:

* Redux
* Zustand
* Jotai

---

# 🔟 `useContext` Is Synchronous

Context updates happen **in same render cycle**.

Example:

```
setUser()
↓
Provider updates
↓
Consumers re-render immediately
```

No async delay.

---

# 1️⃣1️⃣ Derived Values in Consumer

Bad example:

```js
const user = useContext(UserContext);
const permissions = expensiveCalculation(user);
```

Runs every render.

Better:

```
Compute before provider
```

Or:

```js
const permissions = useMemo(
  () => expensiveCalculation(user),
  [user]
);
```

---

# 1️⃣2️⃣ Context + Server Components (React 18+)

Modern React supports context in:

```
Server Components
Client Components
```

But:

```
Stateful logic must stay in Client Components
```

Important for **Next.js architecture**.

---

# 1️⃣3️⃣ Context and Testing

When testing components using context:

You must wrap with provider.

Example:

```js
render(
  <UserContext.Provider value={mockUser}>
    <Avatar />
  </UserContext.Provider>
);
```

Otherwise consumer receives:

```
defaultValue
```

---

# 1️⃣4️⃣ Debugging Context

Use **React DevTools**.

Steps:

```
Open DevTools
↓
Select Provider component
↓
Inspect value
```

Shows current context value.

Very helpful for debugging.

---

# 1️⃣5️⃣ Professional Architecture Pattern

Large apps rarely export raw context.

Instead they use:

```
Context
 + useReducer
 + Custom Hook
```

Example:

```js
const StoreContext = createContext();

export function useStore() {
  return useContext(StoreContext);
}
```

Components use:

```
useStore()
```

Not `useContext`.

Benefits:

* Cleaner API
* Encapsulation
* Easier refactoring

---

# 1️⃣6️⃣ React Fiber and Context Tracking

Internally React tracks context dependencies in **Fiber nodes**.

Process:

```
Component calls useContext
↓
React registers dependency in Fiber
↓
Fiber remembers which context was read
↓
Provider updates value
↓
React schedules updates for dependent fibers
```

This avoids re-rendering entire subtree.

---

# 1️⃣7️⃣ Context Update Algorithm

When provider updates:

```
Old value stored
New value compared
↓
Object.is(oldValue, newValue)
↓
If different → notify consumers
```

Consumers are marked as **dirty** in Fiber tree.

---

# 1️⃣8️⃣ Context vs Redux (Conceptual Difference)

Context:

```
Propagation mechanism
```

Redux:

```
State management system
```

Redux provides:

* selective subscriptions
* middleware
* devtools
* predictable updates

Context does not.

---

# 1️⃣9️⃣ Context Performance in Large Apps

Large apps often follow this structure:

```
AuthContext
ThemeContext
SettingsContext
FeatureFlagsContext
```

Instead of one giant context.

Why?

```
Granular updates
Better performance
```

---

# 2️⃣0️⃣ Golden Rules

✔ Use context for **global stable data**

Examples:

```
auth user
theme
language
settings
feature flags
```

Avoid using context for:

```
form inputs
animations
frequent updates
local component state
```

---

# 🧠 Final Deep Mental Model

Context works like:

```
Provider (broadcasts value)
        ↓
Consumers (subscribed components)
```

React stores subscription in **Fiber dependency list**.

When provider value reference changes:

```
React marks dependent fibers
↓
Only those components re-render
```

Context is therefore:

```
A subscription-based global variable scoped to a subtree
```

---

# 🏁 Final Rendering Flow

### Without Context

```
setUser
↓
App
↓
Navbar
↓
UserMenu
↓
Avatar
```

All re-render.

---

### With Context

```
setUser
↓
Provider
↓
Avatar (consumer)
```

Only consumer re-renders.

---

💡 If you want, I can also give you the **next deep system note**:

### 🔥 **React Rendering System (Fiber, Scheduler, Concurrent Rendering)**

This is **one of the most advanced React topics** and explains:

* why React sometimes renders twice
* how concurrent rendering works
* how React interrupts renders
* why StrictMode double invokes functions
* how updates get priority

This is the **knowledge gap between intermediate and senior React developers**.
