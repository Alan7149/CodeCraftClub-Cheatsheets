# ⚛️ React Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · React (with Hooks) quick reference.

---

## Setup

```bash
npm create vite@latest my-app -- --template react
cd my-app && npm install && npm run dev
```

## Components & JSX

```jsx
function Welcome({ name }) {          // props via destructuring
  return <h1>Hello, {name}</h1>;
}

const Card = ({ title, children }) => (
  <div className="card">
    <h2>{title}</h2>
    {children}
  </div>
);

// Usage
<Welcome name="Alan" />
<Card title="Hi"><p>body</p></Card>
```

## JSX Rules

```jsx
// className not class, camelCase events, close all tags
<div className="box" onClick={handleClick}>
  {condition && <span>shown if true</span>}
  {condition ? <A /> : <B />}
  {items.map(item => <li key={item.id}>{item.name}</li>)}
</div>
```

## useState

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  const [user, setUser] = useState({ name: "", age: 0 });

  return (
    <button onClick={() => setCount(count + 1)}>{count}</button>
  );
}

// Update object/array immutably
setUser(prev => ({ ...prev, name: "Alan" }));
setItems(prev => [...prev, newItem]);
setCount(prev => prev + 1);          // functional update
```

## useEffect

```jsx
import { useEffect } from "react";

useEffect(() => {
  console.log("runs after every render");
});

useEffect(() => {
  console.log("runs once on mount");
}, []);

useEffect(() => {
  console.log("runs when count changes");
}, [count]);

useEffect(() => {
  const id = setInterval(tick, 1000);
  return () => clearInterval(id);     // cleanup on unmount
}, []);
```

## Fetching Data

```jsx
function Posts() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("/api/posts")
      .then(res => res.json())
      .then(data => { setPosts(data); setLoading(false); });
  }, []);

  if (loading) return <p>Loading...</p>;
  return <ul>{posts.map(p => <li key={p.id}>{p.title}</li>)}</ul>;
}
```

## Forms & Controlled Inputs

```jsx
function Form() {
  const [email, setEmail] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={email} onChange={e => setEmail(e.target.value)} />
      <button type="submit">Send</button>
    </form>
  );
}
```

## Other Core Hooks

```jsx
import { useRef, useContext, useMemo, useCallback, useReducer } from "react";

const inputRef = useRef(null);        // DOM ref / mutable value
inputRef.current.focus();

const value = useContext(MyContext);  // consume context

const expensive = useMemo(() => compute(a), [a]);     // memoize value
const memoFn = useCallback(() => doThing(a), [a]);    // memoize function

const [state, dispatch] = useReducer(reducer, initialState);
```

## Context API

```jsx
const ThemeContext = createContext("light");

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <div className={theme}>...</div>;
}
```

## Custom Hooks

```jsx
function useToggle(initial = false) {
  const [on, setOn] = useState(initial);
  const toggle = () => setOn(o => !o);
  return [on, toggle];
}

// usage
const [isOpen, toggleOpen] = useToggle();
```

## Conditional & List Rendering

```jsx
{loading && <Spinner />}
{error ? <Error /> : <Content />}
{users.length === 0 ? <Empty /> : (
  <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>
)}
```

> **Key prop:** always give list items a stable, unique `key` (not the array index when the list can change).

---

[🔝 Back to README](../README.md)
