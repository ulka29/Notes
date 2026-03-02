# 📘 React Components & Props – Complete Detailed Notes

---

# React Components

## What are React Components?

- Components are the building blocks of a React application.
- They allow you to split the UI into reusable pieces.
- Components return HTML (JSX).
- Components can be reused multiple times.

There are two types of components:
1. Class Components
2. Function Components

> In modern React, Function Components are mostly used.

---

## Function Components

A function component is simply a JavaScript function that returns JSX.

### Example:

```jsx
function Welcome() {
  return <h1>Hello React!</h1>;
}
```

Arrow Function Version:

```jsx
const Welcome = () => {
  return <h1>Hello React!</h1>;
};
```

---

## React Components Return HTML (JSX)

- A component must return one parent element.
- JSX must follow JSX rules.

```jsx
function App() {
  return (
    <div>
      <h1>My App</h1>
    </div>
  );
}
```

---

## Create Your First Component

### Step 1: Create component

```jsx
function Car() {
  return <h2>I am a Car!</h2>;
}
```

### Step 2: Use it

```jsx
function App() {
  return (
    <div>
      <Car />
    </div>
  );
}
```

---

## Rendering a Component

- Use component like a custom HTML tag.
- Component names must start with a capital letter.

```jsx
<Car />
```

❌ Wrong:

```jsx
<car />
```

---

## Components in Components

```jsx
function Engine() {
  return <p>Engine is powerful</p>;
}

function Car() {
  return (
    <div>
      <h2>I am a Car</h2>
      <Engine />
    </div>
  );
}
```

---

## Rendering a Component Twice

```jsx
function App() {
  return (
    <div>
      <Car brand="Tesla" />
      <Car brand="BMW" />
    </div>
  );
}
```

---

## Components in Separate Files

### Car.js

```jsx
function Car() {
  return <h2>I am a Car</h2>;
}

export default Car;
```

### App.js

```jsx
import Car from "./Car";

function App() {
  return <Car />;
}
```

---

# React Props

## What are Props?

Props allow you to pass data from one component to another.

Props are like function parameters.

---

## Sending Props

```jsx
function Car(props) {
  return <h2>I am a {props.brand}</h2>;
}

function App() {
  return <Car brand="Tesla" />;
}
```

---

## Passing Multiple Properties

```jsx
<Car brand="Tesla" color="Red" year={2024} />
```

```jsx
function Car(props) {
  return (
    <h2>
      {props.brand} - {props.color} - {props.year}
    </h2>
  );
}
```

---

## Different Data Types in Props

### Strings

```jsx
<Car brand="Tesla" />
```

### Numbers

```jsx
<Car year={2024} />
```

### Variables

```jsx
const carName = "BMW";
<Car brand={carName} />
```

### Objects

```jsx
const carInfo = { brand: "Tesla", color: "Red" };
<Car info={carInfo} />
```

Access:

```jsx
props.info.brand
```

---

## Array Props

```jsx
const colors = ["Red", "Blue", "Black"];
<Car colors={colors} />
```

```jsx
function Car(props) {
  return <p>{props.colors[0]}</p>;
}
```

---

## Object Props

```jsx
const car = {
  brand: "Tesla",
  model: "Model S",
};

<Car data={car} />
```

```jsx
props.data.model
```

---

## Passing Props from Component to Component

```jsx
function Parent() {
  return <Child message="Hello" />;
}

function Child(props) {
  return <GrandChild msg={props.message} />;
}

function GrandChild(props) {
  return <h1>{props.msg}</h1>;
}
```

> Props flow downward only (Unidirectional Data Flow).

---

# Destructuring Props

## Basic Destructuring

```jsx
function Car({ brand }) {
  return <h2>{brand}</h2>;
}
```

---

## Multiple Props Destructuring

```jsx
function Car({ brand, color }) {
  return <h2>{brand} - {color}</h2>;
}
```

---

## Destructuring with ...rest

```jsx
function Car({ brand, ...rest }) {
  console.log(rest);
  return <h2>{brand}</h2>;
}
```

Example:

```jsx
<Car brand="Tesla" color="Red" year={2024} />
```

`rest` becomes:

```js
{ color: "Red", year: 2024 }
```

---

## Default Values in Props

```jsx
function Car({ brand = "Unknown" }) {
  return <h2>{brand}</h2>;
}
```

---

# React Props Children

## What is props.children?

When you pass content inside component tags:

```jsx
<Card>
  <h1>Hello</h1>
</Card>
```

It is stored in:

```js
props.children
```

---

## Example

```jsx
function Card(props) {
  return <div>{props.children}</div>;
}
```

Using destructuring:

```jsx
function Card({ children }) {
  return <div>{children}</div>;
}
```

---

## Multiple Children

```jsx
<Card>
  <h1>Title</h1>
  <p>Description</p>
</Card>
```

`children` can contain:
- JSX
- Strings
- Components
- Multiple elements

---

# Quick Revision Summary

- Components are reusable UI blocks.
- Modern React uses Function Components.
- Components return JSX.
- Props pass data from parent to child.
- Props support strings, numbers, arrays, objects.
- Destructuring makes props cleaner.
- Default values can be set.
- `props.children` allows nested content.
- Props follow unidirectional data flow.
