# React A→Z Revision

**Phase 10 — Performance ⭐**

> Complete revision notes for optimizing React applications, reducing unnecessary renders, improving initial load time, and efficiently handling large amounts of data.

---

# Contents

1. [Performance Overview](#1-performance-overview)
2. [React.memo](#2-reactmemo)
3. [useMemo](#3-usememo)
4. [useCallback](#4-usecallback)
5. [Lazy Loading](#5-lazy-loading)
6. [Code Splitting](#6-code-splitting)
7. [Suspense](#7-suspense)
8. [Virtualization](#8-virtualization)
9. [Debouncing](#9-debouncing)
10. [Throttling](#10-throttling)
11. [React.memo vs useMemo vs useCallback](#11-reactmemo-vs-usememo-vs-usecallback)
12. [Complete Performance Strategy](#12-complete-performance-strategy)
13. [Quick Revision](#13-quick-revision)
14. [Interview Questions](#14-interview-questions)

---

# 1. Performance Overview

## Definition

**React performance optimization** means improving an application's speed, responsiveness, and resource usage by reducing unnecessary rendering, calculations, network requests, and JavaScript that must be loaded initially.

---

## Real-Life Example

Imagine a restaurant with 100 customers.

A poorly optimized system might:

```text
Customer 1 changes order
        ↓
Recalculate everything for all 100 customers
```

An optimized system updates only what actually changed:

```text
Customer 1 changes order
        ↓
Update Customer 1
```

React performance optimization follows the same idea:

```text
State Change
    ↓
React Render
    ↓
Avoid unnecessary work
    ↓
Update only what is required
```

---

## Common Performance Problems

```text
Unnecessary re-renders
        ↓
Expensive calculations
        ↓
Large JavaScript bundles
        ↓
Large lists
        ↓
Too many API requests
        ↓
Poor user experience
```

React provides several techniques to solve these problems.

---

# 2. React.memo

## Definition

`React.memo()` prevents a functional component from re-rendering when its **props have not changed**.

```jsx
const MyComponent = React.memo(Component);
```

---

## Real-Life Example

Imagine a teacher has 50 students.

If Student A changes their assignment:

```text
Student A changes
        ↓
Only Student A's information needs updating
```

There is no reason to recalculate all 50 students.

`React.memo` follows a similar idea.

---

## Without React.memo

```jsx
function User({ name }) {
  console.log("User rendered");

  return <h2>{name}</h2>;
}
```

If the parent re-renders:

```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        {count}
      </button>

      <User name="Aniket" />
    </>
  );
}
```

`User` may render again even though:

```text
name = "Aniket"
```

has not changed.

---

## With React.memo

```jsx
const User = React.memo(function User({ name }) {
  console.log("User rendered");

  return <h2>{name}</h2>;
});
```

Now React can skip rendering `User` when its props remain unchanged.

---

## Important

`React.memo` performs a **shallow comparison of props** by default.

Primitive values:

```js
"Aniket"
10
true
```

are straightforward.

But objects and functions can cause problems:

```jsx
<User user={{ name: "Aniket" }} />
```

A new object is created on every render.

Therefore:

```js
{} !== {}
```

React sees the prop as changed.

---

## When to Use

Use `React.memo` when:

* Component renders frequently
* Component is expensive to render
* Parent re-renders frequently
* Props usually remain unchanged

Do **not** blindly wrap every component with `React.memo`.

---

# 3. useMemo

## Definition

`useMemo()` memoizes the **result of an expensive calculation** and recomputes it only when its dependencies change.

```jsx
const value = useMemo(() => {
  return expensiveCalculation();
}, [dependency]);
```

---

## Real-Life Example

Suppose you calculate a student's final result.

```text
100 marks
↓
Calculate percentage
↓
Calculate grade
↓
Calculate rank
```

If unrelated information changes, you don't need to calculate everything again.

You can reuse the previous result.

That is the idea behind `useMemo`.

---

## Example

```jsx
import { useMemo, useState } from "react";

function ProductList({ products }) {
  const [count, setCount] = useState(0);

  const expensiveProducts = useMemo(() => {
    return products.filter(product => product.price > 1000);
  }, [products]);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        {count}
      </button>

      {expensiveProducts.map(product => (
        <p key={product.id}>{product.name}</p>
      ))}
    </>
  );
}
```

The filtering operation runs again only when:

```js
products
```

changes.

---

## Without useMemo

```js
const expensiveProducts =
  products.filter(product => product.price > 1000);
```

This calculation executes on every render.

---

## With useMemo

```js
const expensiveProducts = useMemo(
  () => products.filter(product => product.price > 1000),
  [products]
);
```

React can reuse the previous result when `products` has not changed.

---

## Important

Do not use `useMemo` for every calculation.

Bad:

```js
const fullName = useMemo(
  () => `${firstName} ${lastName}`,
  [firstName, lastName]
);
```

This calculation is already extremely cheap.

Use `useMemo` primarily when the calculation is expensive or when referential stability is useful.

---

# 4. useCallback

## Definition

`useCallback()` memoizes a **function reference** so that the same function object can be reused between renders until its dependencies change.

```jsx
const handleClick = useCallback(() => {
  // logic
}, [dependencies]);
```

---

## Real-Life Example

Imagine giving a worker the same instruction sheet.

Instead of printing a new copy every time:

```text
Same instruction
      ↓
Reuse existing copy
```

`useCallback` similarly preserves a function reference.

---

## Example

```jsx
import { useCallback, useState } from "react";

const Button = React.memo(function Button({ onClick }) {
  console.log("Button rendered");

  return (
    <button onClick={onClick}>
      Click
    </button>
  );
});

function App() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>

      <Button onClick={handleClick} />
    </>
  );
}
```

Because `handleClick` maintains the same reference, `React.memo` can prevent unnecessary rendering of `Button`.

---

## Why Function References Matter

Every render can create a new function:

```js
const handleClick = () => {
  console.log("Clicked");
};
```

Even if the function looks identical:

```js
oldFunction !== newFunction
```

Therefore a memoized child may consider the function prop changed.

`useCallback` helps maintain the reference.

---

## useCallback vs useMemo

```js
useMemo(() => calculateValue(), [dependency]);
```

Memoizes:

```text
VALUE
```

While:

```js
useCallback(() => handleSomething(), [dependency]);
```

Memoizes:

```text
FUNCTION
```

Conceptually:

```js
useCallback(fn, deps)
```

is similar to:

```js
useMemo(() => fn, deps)
```

---

# 5. Lazy Loading

## Definition

**Lazy loading** means loading a component or resource only when it is actually needed instead of loading everything immediately.

---

## Real-Life Example

Imagine a university website with:

```text
Home
About
Courses
Admin
Reports
Settings
```

A normal user visiting Home doesn't need the Admin dashboard immediately.

Instead of downloading:

```text
Home + About + Courses + Admin + Reports + Settings
```

load:

```text
Home
```

and load Admin later when needed.

---

## React.lazy

```jsx
import { lazy } from "react";

const Admin = lazy(() => import("./Admin"));
```

The Admin component is downloaded when React needs to render it.

---

# 6. Code Splitting

## Definition

**Code splitting** divides a large JavaScript bundle into smaller chunks that can be loaded independently.

---

## Without Code Splitting

```text
main.js
├── Home
├── About
├── Dashboard
├── Admin
├── Reports
└── Settings
```

The browser may need to download a large bundle.

---

## With Code Splitting

```text
main.js
├── Home
└── common code

admin.chunk.js
reports.chunk.js
settings.chunk.js
```

Only required chunks are loaded.

---

## Real-Life Example

Instead of carrying your entire house when going to college:

```text
Everything → Carry everything
```

You carry only what you need:

```text
College → College books
Gym → Gym clothes
Travel → Travel items
```

Code splitting does something similar with JavaScript.

---

## Route-Based Code Splitting

This is one of the most common uses.

```jsx
import { lazy } from "react";

const Home = lazy(() => import("./pages/Home"));
const Dashboard = lazy(() => import("./pages/Dashboard"));
const Settings = lazy(() => import("./pages/Settings"));
```

---

## Lazy Loading vs Code Splitting

They are related but not exactly the same.

### Code Splitting

```text
Break one large bundle
        ↓
Multiple smaller chunks
```

### Lazy Loading

```text
Load a chunk
        ↓
Only when needed
```

A common React implementation is:

```js
lazy(() => import("./Component"));
```

which uses dynamic imports and enables chunk-based loading.

---

# 7. Suspense

## Definition

`Suspense` allows React to display a fallback UI while a child component is waiting to become ready.

---

## Real-Life Example

When an elevator is coming:

```text
Press button
    ↓
Waiting...
    ↓
Elevator arrives
```

The waiting state is similar to a Suspense fallback.

---

## Example

```jsx
import { lazy, Suspense } from "react";

const Dashboard = lazy(() => import("./Dashboard"));

function App() {
  return (
    <Suspense fallback={<p>Loading Dashboard...</p>}>
      <Dashboard />
    </Suspense>
  );
}
```

Flow:

```text
Render Dashboard
      ↓
Chunk not loaded
      ↓
Show fallback
      ↓
Chunk loads
      ↓
Render Dashboard
```

---

## Suspense Boundary

A Suspense boundary defines which part of the UI should show the fallback.

```jsx
<Suspense fallback={<Loader />}>
  <Dashboard />
</Suspense>
```

You can also have multiple boundaries:

```jsx
<Suspense fallback={<DashboardLoader />}>
  <Dashboard />
</Suspense>

<Suspense fallback={<ProfileLoader />}>
  <Profile />
</Suspense>
```

This allows different parts of the UI to load independently.

---

# 8. Virtualization

## Definition

**Virtualization** means rendering only the items currently visible in the viewport instead of rendering the entire large dataset.

---

## Real-Life Example

Imagine a warehouse containing:

```text
100,000 products
```

You don't place all 100,000 products on the shop floor.

You display only the products customers currently need.

Virtualization does something similar.

---

## Problem

Suppose:

```js
const users = 100000;
```

Rendering:

```jsx
users.map(user => (
  <User key={user.id} user={user} />
))
```

creates a huge number of DOM elements.

This can cause:

```text
High memory usage
Slow rendering
Slow scrolling
Poor performance
```

---

## Virtualization

Instead of:

```text
100,000 DOM nodes
```

the application might maintain only:

```text
20–50 visible DOM nodes
```

and reuse them as the user scrolls.

---

## Example with a Virtualization Library

A common library is `react-window`.

Conceptually:

```jsx
import { FixedSizeList } from "react-window";

function UserList({ users }) {
  return (
    <FixedSizeList
      height={500}
      width={400}
      itemCount={users.length}
      itemSize={50}
    >
      {({ index, style }) => (
        <div style={style}>
          {users[index].name}
        </div>
      )}
    </FixedSizeList>
  );
}
```

The important idea:

```text
100,000 items
      ↓
Only visible items rendered
```

---

## Virtualization vs Pagination

### Pagination

```text
Page 1 → 20 items
Page 2 → 20 items
```

Only one page is fetched/displayed.

### Virtualization

```text
100,000 loaded items
        ↓
Only visible items rendered
```

They solve different problems.

They can also be combined:

```text
Pagination
    +
Virtualization
```

---

# 9. Debouncing

## Definition

**Debouncing** delays function execution until the user stops triggering an event for a specified period.

---

## Real-Life Example

Search:

```text
R
Re
Rea
Reac
React
```

Instead of making five API requests:

```text
Wait 500ms
      ↓
Make one request
```

---

## React Example

```jsx
import { useEffect, useState } from "react";

function Search() {
  const [query, setQuery] = useState("");

  useEffect(() => {
    const timer = setTimeout(() => {
      if (query.trim()) {
        console.log("Search API:", query);
      }
    }, 500);

    return () => clearTimeout(timer);
  }, [query]);

  return (
    <input
      value={query}
      onChange={event => setQuery(event.target.value)}
    />
  );
}
```

---

## Common Use Cases

```text
Search
Autocomplete
API requests
Form validation
Filtering
```

---

# 10. Throttling

## Definition

**Throttling** limits a function so it executes at most once during a specified time interval.

---

## Real-Life Example

Imagine someone pressing a doorbell continuously:

```text
Press
Press
Press
Press
Press
```

Instead of processing every press, the system accepts one press per second.

---

## Example

```js
function throttle(callback, delay) {
  let lastCall = 0;

  return (...args) => {
    const now = Date.now();

    if (now - lastCall >= delay) {
      lastCall = now;
      callback(...args);
    }
  };
}
```

---

## React Example

```jsx
import { useEffect } from "react";

function App() {
  useEffect(() => {
    const handleScroll = throttle(() => {
      console.log("Scroll position:", window.scrollY);
    }, 200);

    window.addEventListener("scroll", handleScroll);

    return () => {
      window.removeEventListener("scroll", handleScroll);
    };
  }, []);

  return <div>Long page...</div>;
}
```

---

## Debouncing vs Throttling

| Debouncing                | Throttling                   |
| ------------------------- | ---------------------------- |
| Runs after activity stops | Runs at controlled intervals |
| Waits for inactivity      | Limits execution frequency   |
| Search is common          | Scroll is common             |
| Autocomplete              | Mouse movement               |
| Input validation          | Resize events                |

### Easy Memory Trick

```text
Debounce → "Wait until user stops"

Throttle → "Run at a limited rate"
```

---

# 11. React.memo vs useMemo vs useCallback

This is one of the most important React interview concepts.

| Feature       | Purpose                               | Memoizes  |
| ------------- | ------------------------------------- | --------- |
| `React.memo`  | Prevent unnecessary component renders | Component |
| `useMemo`     | Avoid expensive recalculations        | Value     |
| `useCallback` | Preserve function reference           | Function  |

---

## React.memo

```jsx
const User = React.memo(UserComponent);
```

Think:

```text
Component optimization
```

---

## useMemo

```jsx
const result = useMemo(
  () => expensiveCalculation(data),
  [data]
);
```

Think:

```text
Value optimization
```

---

## useCallback

```jsx
const handleClick = useCallback(
  () => doSomething(id),
  [id]
);
```

Think:

```text
Function optimization
```

---

# 12. Complete Performance Strategy

Performance optimization should not mean:

> "Use every optimization everywhere."

Instead:

```text
Measure
  ↓
Find bottleneck
  ↓
Choose optimization
  ↓
Measure again
```

---

## Optimization Decision Map

```text
Component re-renders unnecessarily?
            ↓
       React.memo


Expensive calculation?
            ↓
         useMemo


Function reference causes re-render?
            ↓
       useCallback


Large JavaScript bundle?
            ↓
      Code Splitting


Component not needed immediately?
            ↓
       Lazy Loading


Component is loading?
            ↓
         Suspense


Huge list?
            ↓
      Virtualization


Too many requests while typing?
            ↓
       Debouncing


Too many frequent events?
            ↓
       Throttling
```

---

# Important Performance Principles

## 1. Avoid Unnecessary Re-renders

```text
State change
   ↓
Parent renders
   ↓
Children may render
```

Use techniques such as:

```text
React.memo
useMemo
useCallback
```

when they actually address a measured problem.

---

## 2. Reduce Initial JavaScript

Use:

```text
Lazy Loading
Code Splitting
```

Instead of downloading the entire application immediately.

---

## 3. Optimize Large Lists

Use:

```text
Virtualization
Pagination
Infinite Scroll
```

depending on the use case.

---

## 4. Reduce Event Frequency

Use:

```text
Debouncing
Throttling
```

for high-frequency user events.

---

# Performance Example

Imagine an e-commerce application:

```text
Product List → 50,000 products
Search → API request
Dashboard → Large component
Admin → Rarely accessed
Scroll → Continuous event
```

Possible optimization:

```text
50,000 products
       ↓
Virtualization

Search
       ↓
Debouncing

Scroll
       ↓
Throttling

Admin
       ↓
Lazy Loading

Large calculations
       ↓
useMemo

Stable callbacks
       ↓
useCallback

Expensive child components
       ↓
React.memo

Large application
       ↓
Code Splitting
```

---

# 13. Quick Revision

| Concept        | One-Line Definition                                            |
| -------------- | -------------------------------------------------------------- |
| `React.memo`   | Prevents unnecessary child re-renders when props are unchanged |
| `useMemo`      | Memoizes the result of a calculation                           |
| `useCallback`  | Memoizes a function reference                                  |
| Lazy Loading   | Loads resources only when they are needed                      |
| Code Splitting | Splits a large bundle into smaller chunks                      |
| `Suspense`     | Displays fallback UI while content is loading                  |
| Virtualization | Renders only visible items from a large list                   |
| Debouncing     | Executes after triggering activity stops                       |
| Throttling     | Limits execution to a fixed frequency                          |

---

# Performance Optimization Map

```text
                  REACT PERFORMANCE
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
     Rendering         Loading        Events
          │              │              │
     ┌────┼────┐      ┌──┴───┐      ┌───┴────┐
     ↓    ↓    ↓      ↓      ↓      ↓        ↓
   memo useMemo      Lazy   Code   Debounce Throttle
       useCallback   Load  Split
                         │
                         ↓
                     Suspense
                         │
                         ↓
                    Virtualization
```

---

# 14. Interview Questions

## Basic

1. What is React performance optimization?
2. What causes unnecessary re-renders?
3. What is `React.memo()`?
4. What is `useMemo()`?
5. What is `useCallback()`?
6. What is lazy loading?
7. What is code splitting?
8. What is `Suspense`?
9. What is virtualization?
10. What is debouncing?
11. What is throttling?

---

## Intermediate

12. What is the difference between `React.memo` and `useMemo`?
13. What is the difference between `useMemo` and `useCallback`?
14. When should you use `React.memo`?
15. Why can objects and functions break memoization?
16. How does `React.lazy()` work?
17. How does code splitting improve initial load performance?
18. What is a Suspense boundary?
19. Why is virtualization useful for large lists?
20. Debouncing vs throttling?
21. How would you optimize a search input?
22. How would you optimize a scroll event?

---

## Advanced

23. Does `React.memo()` guarantee that a component will never re-render?
24. Why can excessive memoization hurt performance?
25. How does referential equality affect `useMemo`, `useCallback`, and `React.memo`?
26. How would you identify unnecessary re-renders in a React application?
27. How would you optimize a React page containing 100,000 records?
28. How would you implement route-based code splitting?
29. How would you combine pagination and virtualization?
30. How would you optimize a real-time dashboard?
31. When should you avoid `useMemo()`?
32. When should you avoid `useCallback()`?
33. What is the difference between lazy loading and code splitting?
34. How would you reduce the initial bundle size of a React application?

---

# Final Mental Model

Remember these five categories:

```text
1. RENDERING
   ├── React.memo
   ├── useMemo
   └── useCallback

2. LOADING
   ├── Lazy Loading
   ├── Code Splitting
   └── Suspense

3. LARGE DATA
   └── Virtualization

4. USER INPUT
   └── Debouncing

5. HIGH-FREQUENCY EVENTS
   └── Throttling
```

## One-Line Memory Trick

> **Memoize expensive work, stabilize important references, load code only when needed, render only visible data, and control high-frequency events.**
