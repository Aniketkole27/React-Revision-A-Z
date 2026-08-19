# React A→Z Revision

**Phase 11 — Advanced React ⭐**

> Advanced React patterns and APIs for building reusable, scalable, maintainable, and flexible components.

---

# Contents

1. [Advanced React Overview](#1-advanced-react-overview)
2. [Error Boundaries](#2-error-boundaries)
3. [Portals](#3-portals)
4. [Refs](#4-refs)
5. [Forwarding Refs](#5-forwarding-refs)
6. [Higher-Order Components (HOC)](#6-higher-order-components-hoc)
7. [Render Props](#7-render-props)
8. [Compound Components](#8-compound-components)
9. [Composition](#9-composition)
10. [Custom Hooks](#10-custom-hooks)
11. [Patterns Comparison](#11-patterns-comparison)
12. [Complete Real-World Example](#12-complete-real-world-example)
13. [Quick Revision](#13-quick-revision)
14. [Interview Questions](#14-interview-questions)

---

# 1. Advanced React Overview

React provides several advanced techniques for handling:

```text
Error Handling
DOM Access
Component Reusability
Logic Reusability
Flexible Components
Complex UI Components
Cross-Cutting Concerns
```

The major concepts in this phase are:

```text
Error Boundaries
Portals
Refs
Forwarding Refs
HOC
Render Props
Compound Components
Composition
Custom Hooks
```

---

# 2. Error Boundaries

## Definition

An **Error Boundary** is a React component that catches JavaScript errors in its child component tree and displays fallback UI instead of allowing the entire UI to crash.

---

## Real-Life Example

Imagine a shopping mall.

```text
Mall
├── Clothing Store
├── Food Court
├── Cinema
└── Electronics
```

If the electronics store has a problem, the entire mall should not shut down.

Similarly:

```text
React Application
├── Navbar
├── Dashboard
├── Profile
└── Payment
```

If the Dashboard crashes:

```text
Dashboard ❌
```

the rest of the application should ideally remain usable.

An Error Boundary isolates that failure.

---

## Basic Error Boundary

Error Boundaries are traditionally implemented using a **class component**.

```jsx
import React from "react";

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      hasError: false
    };
  }

  static getDerivedStateFromError(error) {
    return {
      hasError: true
    };
  }

  componentDidCatch(error, errorInfo) {
    console.error(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong.</h2>;
    }

    return this.props.children;
  }
}
```

Usage:

```jsx
<ErrorBoundary>
  <Dashboard />
</ErrorBoundary>
```

---

## How It Works

```text
Dashboard
    ↓
Runtime Error
    ↓
Error Boundary catches error
    ↓
Fallback UI
```

---

## What Error Boundaries Catch

They catch errors during:

```text
Rendering
Lifecycle methods
Constructors of child components
```

---

## What Error Boundaries Do NOT Catch

They do not automatically catch errors from:

```text
Event handlers
Async callbacks
setTimeout
Promise callbacks
Server-side rendering
Errors inside the boundary itself
```

For example:

```jsx
<button
  onClick={() => {
    throw new Error("Click error");
  }}
>
  Click
</button>
```

This is not automatically handled by an Error Boundary.

---

## Multiple Error Boundaries

You can isolate different parts of an application.

```jsx
<ErrorBoundary>
  <Navbar />
</ErrorBoundary>

<ErrorBoundary>
  <Dashboard />
</ErrorBoundary>

<ErrorBoundary>
  <Profile />
</ErrorBoundary>
```

This provides more granular error handling.

---

## When to Use

Useful for:

* Dashboards
* Large applications
* Third-party components
* Complex UI sections
* Production error handling

---

# 3. Portals

## Definition

A **Portal** allows a React component to render its UI into a different DOM node outside its normal parent DOM hierarchy.

```jsx
createPortal(children, domNode)
```

---

## Real-Life Example

Imagine a person belongs to one department:

```text
Engineering
```

but is temporarily working inside:

```text
Conference Room
```

Their logical organization has not changed.

Similarly, a React component can logically belong to one component tree while its DOM element is rendered somewhere else.

---

## HTML

```html
<div id="root"></div>

<div id="modal-root"></div>
```

---

## React Portal

```jsx
import { createPortal } from "react-dom";

function Modal() {
  return createPortal(
    <div className="modal">
      <h2>Hello</h2>
    </div>,
    document.getElementById("modal-root")
  );
}
```

Usage:

```jsx
function App() {
  return (
    <div>
      <h1>App</h1>

      <Modal />
    </div>
  );
}
```

The modal appears under:

```html
<div id="modal-root">
  ...
</div>
```

instead of inside the normal DOM position of `<Modal />`.

---

## Common Use Cases

Portals are commonly used for:

```text
Modal
Dialog
Tooltip
Dropdown
Toast
Notification
Popover
```

---

## Why Modal Needs a Portal

Suppose:

```css
.container {
  overflow: hidden;
}
```

A modal rendered inside this container can be clipped.

Using a portal allows the modal to render near the document root:

```text
body
├── #root
│   └── App
│       └── Modal component
│
└── #modal-root
    └── Actual modal DOM
```

---

## Important

Although the DOM node is different, the portal still belongs to the **React tree**.

Therefore React context still works, and events participate in React's event propagation model.

---

# 4. Refs

## Definition

A **ref** provides a way to access a DOM element or persist a mutable value between renders without causing a re-render when that value changes.

The most common hook is:

```jsx
useRef()
```

---

## Real-Life Example

Think of a labeled storage box.

You don't recreate the box every time you change what's inside.

You simply access the same box and modify its contents.

Similarly:

```js
const ref = useRef();
```

provides a stable object:

```js
ref.current
```

---

# DOM Reference

```jsx
import { useRef } from "react";

function Input() {
  const inputRef = useRef(null);

  function focusInput() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />

      <button onClick={focusInput}>
        Focus
      </button>
    </>
  );
}
```

Flow:

```text
Click button
    ↓
inputRef.current
    ↓
Input DOM element
    ↓
focus()
```

---

# useRef for Mutable Values

`useRef` can also store values that persist across renders.

```jsx
const countRef = useRef(0);

countRef.current += 1;
```

Changing:

```js
countRef.current
```

does **not** trigger a component re-render.

---

## useState vs useRef

| `useState`                  | `useRef`                                               |
| --------------------------- | ------------------------------------------------------ |
| Stores state                | Stores mutable reference                               |
| Updating causes re-render   | Updating does not cause re-render                      |
| Used for UI state           | Used for DOM references, timers, previous values, etc. |
| Value is used for rendering | Value can persist without rendering                    |

---

## Timer Example

```jsx
function Timer() {
  const timerRef = useRef(null);

  function startTimer() {
    timerRef.current = setInterval(() => {
      console.log("Running");
    }, 1000);
  }

  function stopTimer() {
    clearInterval(timerRef.current);
  }

  return (
    <>
      <button onClick={startTimer}>Start</button>
      <button onClick={stopTimer}>Stop</button>
    </>
  );
}
```

---

## Common Ref Use Cases

```text
DOM access
Focus input
Scroll element
Store timer ID
Store previous value
Integrate third-party libraries
Store mutable values
```

---

# 5. Forwarding Refs

## Definition

**Ref forwarding** allows a parent component to pass a ref through a custom component to a DOM element inside that component.

---

## Problem

This does not directly give the parent the internal input DOM node:

```jsx
function CustomInput() {
  return <input />;
}
```

Parent:

```jsx
const inputRef = useRef();

<CustomInput ref={inputRef} />
```

For a regular function component, the ref is not automatically forwarded to the internal DOM element.

---

## Forwarding the Ref

```jsx
import { forwardRef } from "react";

const CustomInput = forwardRef(function CustomInput(props, ref) {
  return (
    <input
      ref={ref}
      {...props}
    />
  );
});
```

Parent:

```jsx
function App() {
  const inputRef = useRef(null);

  function focusInput() {
    inputRef.current.focus();
  }

  return (
    <>
      <CustomInput ref={inputRef} />

      <button onClick={focusInput}>
        Focus
      </button>
    </>
  );
}
```

Flow:

```text
Parent
  ↓
ref
  ↓
CustomInput
  ↓
input
```

---

## Why Use Forwarding Refs?

Useful for reusable UI components:

```text
CustomInput
CustomButton
Modal
Select
DatePicker
UI libraries
```

---

## Modern React Note

In modern React, React 19 allows `ref` to be passed as a prop to function components, reducing the need for `forwardRef` in new code. However, `forwardRef` remains important when working with existing React codebases and APIs built around it.

---

# 6. Higher-Order Components (HOC)

## Definition

A **Higher-Order Component** is a function that takes a component and returns a new enhanced component.

```js
const EnhancedComponent = HOC(Component);
```

---

## Real-Life Example

Think of a basic car:

```text
Car
```

You add:

```text
GPS
Security System
Insurance
```

and get:

```text
Enhanced Car
```

A HOC similarly enhances a component with additional behavior.

---

## Basic HOC

```jsx
function withLogger(Component) {
  return function EnhancedComponent(props) {
    console.log("Component rendered");

    return <Component {...props} />;
  };
}
```

Usage:

```jsx
function User() {
  return <h2>User</h2>;
}

const UserWithLogger = withLogger(User);
```

---

## Authentication HOC

```jsx
function withAuth(Component) {
  return function ProtectedComponent(props) {
    const isLoggedIn = true;

    if (!isLoggedIn) {
      return <p>Please login.</p>;
    }

    return <Component {...props} />;
  };
}
```

Usage:

```jsx
const ProtectedDashboard = withAuth(Dashboard);
```

---

## HOC Flow

```text
Original Component
       ↓
      HOC
       ↓
Enhanced Component
```

---

## Common HOC Use Cases

Historically used for:

```text
Authentication
Authorization
Logging
Analytics
Permissions
Feature flags
Data fetching
```

---

## Important

HOCs should generally:

```text
Not mutate the original component
Pass unrelated props through
Have clear naming
```

Modern React applications often prefer:

```text
Custom Hooks
Composition
```

for many cases where HOCs were historically used.

---

# 7. Render Props

## Definition

**Render Props** is a pattern where a component receives a function as a prop and calls that function to determine what UI should be rendered.

---

## Real-Life Example

Imagine a service providing information:

```text
Service
 ↓
Provides data
 ↓
Customer decides how to display it
```

The component provides the logic while the consumer controls the UI.

---

## Example

```jsx
function MouseTracker({ render }) {
  const [position, setPosition] = React.useState({
    x: 0,
    y: 0
  });

  return (
    <div
      onMouseMove={event => {
        setPosition({
          x: event.clientX,
          y: event.clientY
        });
      }}
    >
      {render(position)}
    </div>
  );
}
```

Usage:

```jsx
<MouseTracker
  render={({ x, y }) => (
    <p>
      Mouse: {x}, {y}
    </p>
  )}
/>
```

---

## Flow

```text
MouseTracker
     ↓
Calculates position
     ↓
Calls render(position)
     ↓
Consumer decides UI
```

---

## Children as a Function

Render Props can also be implemented using `children`.

```jsx
function DataProvider({ children }) {
  const data = {
    name: "Aniket"
  };

  return children(data);
}
```

Usage:

```jsx
<DataProvider>
  {data => <h2>{data.name}</h2>}
</DataProvider>
```

---

## Modern Alternative

Many use cases that previously required Render Props can now be handled more simply with:

```text
Custom Hooks
Composition
Context
```

---

# 8. Compound Components

## Definition

**Compound Components** are a group of components that work together to create a flexible UI component while sharing implicit state or behavior.

---

## Real-Life Example

Think about a `<select>`:

```jsx
<select>
  <option>React</option>
  <option>JavaScript</option>
</select>
```

The components work together:

```text
Select
 └── Option
```

Each piece has a specific responsibility.

---

## Example API

A compound component might look like:

```jsx
<Tabs>
  <Tabs.List>
    <Tabs.Tab>Profile</Tabs.Tab>
    <Tabs.Tab>Settings</Tabs.Tab>
  </Tabs.List>

  <Tabs.Panel>
    Profile Content
  </Tabs.Panel>
</Tabs>
```

The parent controls shared state while child components provide the UI structure.

---

## Basic Example

```jsx
import { createContext, useContext, useState } from "react";

const TabsContext = createContext(null);

function Tabs({ children }) {
  const [activeTab, setActiveTab] = useState("profile");

  return (
    <TabsContext.Provider
      value={{ activeTab, setActiveTab }}
    >
      {children}
    </TabsContext.Provider>
  );
}

function Tab({ value, children }) {
  const { activeTab, setActiveTab } =
    useContext(TabsContext);

  return (
    <button
      onClick={() => setActiveTab(value)}
      style={{
        fontWeight:
          activeTab === value ? "bold" : "normal"
      }}
    >
      {children}
    </button>
  );
}

function Panel({ value, children }) {
  const { activeTab } = useContext(TabsContext);

  if (activeTab !== value) {
    return null;
  }

  return <div>{children}</div>;
}
```

Attach components:

```jsx
Tabs.Tab = Tab;
Tabs.Panel = Panel;
```

Usage:

```jsx
<Tabs>
  <Tabs.Tab value="profile">
    Profile
  </Tabs.Tab>

  <Tabs.Tab value="settings">
    Settings
  </Tabs.Tab>

  <Tabs.Panel value="profile">
    Profile Content
  </Tabs.Panel>

  <Tabs.Panel value="settings">
    Settings Content
  </Tabs.Panel>
</Tabs>
```

---

## Advantages

Compound components provide:

```text
Flexible API
Reusable behavior
Readable JSX
Shared state
Custom layouts
```

Common examples:

```text
Tabs
Accordion
Dropdown
Menu
Modal
Select
```

---

# 9. Composition

## Definition

**Composition** means building complex components by combining smaller components instead of relying heavily on inheritance or complicated configuration.

Composition is one of React's fundamental design principles.

---

## Real-Life Example

A computer is composed of:

```text
Computer
├── CPU
├── RAM
├── Storage
├── GPU
└── Power Supply
```

Each part has a specific responsibility.

Similarly:

```text
Page
├── Navbar
├── Sidebar
├── Content
└── Footer
```

---

## Component Composition

```jsx
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```

Usage:

```jsx
<Card>
  <h2>Profile</h2>
  <p>Welcome back!</p>
</Card>
```

The `Card` doesn't need to know exactly what content it contains.

---

## Children Prop

`children` is one of the most important composition mechanisms.

```jsx
function Layout({ children }) {
  return (
    <main>
      <Navbar />
      {children}
      <Footer />
    </main>
  );
}
```

Usage:

```jsx
<Layout>
  <Dashboard />
</Layout>
```

---

## Multiple Slots

Composition can use multiple props.

```jsx
function Layout({
  header,
  sidebar,
  content
}) {
  return (
    <>
      <header>{header}</header>

      <aside>{sidebar}</aside>

      <main>{content}</main>
    </>
  );
}
```

Usage:

```jsx
<Layout
  header={<Navbar />}
  sidebar={<Sidebar />}
  content={<Dashboard />}
/>
```

---

## Composition vs Inheritance

React generally favors:

```text
Composition
```

over:

```text
Inheritance
```

Instead of:

```text
ParentComponent
      ↓
ChildComponent
      ↓
GrandChildComponent
```

prefer combining small components:

```text
Navbar
Sidebar
Card
Button
Modal
```

---

# 10. Custom Hooks

## Definition

A **Custom Hook** is a JavaScript function whose name starts with `use` and that can reuse stateful React logic across components.

---

## Real-Life Example

Imagine a reusable machine:

```text
Machine
 ↓
Same internal process
 ↓
Different users can use it
```

A Custom Hook provides reusable logic:

```text
Custom Hook
 ↓
Component A
Component B
Component C
```

---

## Basic Custom Hook

```jsx
import { useState } from "react";

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);

  const increment = () => {
    setCount(prev => prev + 1);
  };

  const decrement = () => {
    setCount(prev => prev - 1);
  };

  return {
    count,
    increment,
    decrement
  };
}
```

Usage:

```jsx
function Counter() {
  const {
    count,
    increment,
    decrement
  } = useCounter();

  return (
    <>
      <p>{count}</p>

      <button onClick={increment}>
        +
      </button>

      <button onClick={decrement}>
        -
      </button>
    </>
  );
}
```

Another component can reuse the same logic:

```jsx
function AnotherCounter() {
  const counter = useCounter(10);

  return (
    <button onClick={counter.increment}>
      {counter.count}
    </button>
  );
}
```

---

## Custom Hook for Fetching Data

```jsx
import { useEffect, useState } from "react";

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchData() {
      try {
        setLoading(true);

        const response = await fetch(url);

        if (!response.ok) {
          throw new Error("Request failed");
        }

        const result = await response.json();

        setData(result);
      } catch (error) {
        setError(error.message);
      } finally {
        setLoading(false);
      }
    }

    fetchData();
  }, [url]);

  return {
    data,
    loading,
    error
  };
}
```

Usage:

```jsx
function Users() {
  const {
    data,
    loading,
    error
  } = useFetch("/api/users");

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>{error}</p>;
  }

  return (
    <>
      {data.map(user => (
        <p key={user.id}>
          {user.name}
        </p>
      ))}
    </>
  );
}
```

---

## Rules of Custom Hooks

A Custom Hook:

```text
Must start with "use"
Can use other Hooks
Should extract reusable logic
Does not necessarily return JSX
```

Examples:

```text
useFetch()
useAuth()
useDebounce()
useLocalStorage()
useOnlineStatus()
usePrevious()
useWindowSize()
```

---

## Custom Hook vs Component

### Component

Returns UI:

```jsx
function Button() {
  return <button>Click</button>;
}
```

### Custom Hook

Returns logic/data:

```js
function useCounter() {
  return {
    count,
    increment
  };
}
```

Think:

```text
Component → UI

Custom Hook → Logic
```

---

# 11. Patterns Comparison

| Pattern             | Main Purpose                          |
| ------------------- | ------------------------------------- |
| Error Boundary      | Handle rendering errors               |
| Portal              | Render UI outside normal DOM location |
| Ref                 | Access DOM or persist mutable value   |
| Forwarding Ref      | Pass ref through a component          |
| HOC                 | Enhance a component                   |
| Render Props        | Share logic through a function prop   |
| Compound Components | Build flexible related components     |
| Composition         | Combine components to build larger UI |
| Custom Hook         | Reuse stateful logic                  |

---

# When Should You Use Which?

```text
Component crashes?
        ↓
Error Boundary


Need Modal/Tooltip outside parent DOM?
        ↓
Portal


Need DOM access?
        ↓
useRef


Need parent ref to reach child DOM?
        ↓
Ref forwarding


Need to enhance existing components?
        ↓
HOC


Need consumer-controlled rendering?
        ↓
Render Props


Need multiple related components sharing state?
        ↓
Compound Components


Need flexible component structure?
        ↓
Composition


Need reusable stateful logic?
        ↓
Custom Hook
```

---

# 12. Complete Real-World Example

Imagine building a reusable Modal system.

We can combine multiple advanced React concepts.

## Modal

```jsx
import {
  createContext,
  useContext,
  useState
} from "react";

const ModalContext = createContext(null);

function Modal({ children }) {
  const [open, setOpen] = useState(false);

  return (
    <ModalContext.Provider
      value={{
        open,
        setOpen
      }}
    >
      {children}
    </ModalContext.Provider>
  );
}
```

---

## Modal Trigger

```jsx
function Trigger({ children }) {
  const { setOpen } = useContext(ModalContext);

  return (
    <button onClick={() => setOpen(true)}>
      {children}
    </button>
  );
}
```

---

## Modal Content

```jsx
import { createPortal } from "react-dom";

function Content({ children }) {
  const { open, setOpen } =
    useContext(ModalContext);

  if (!open) {
    return null;
  }

  return createPortal(
    <div className="modal">
      <div className="modal-content">
        {children}

        <button onClick={() => setOpen(false)}>
          Close
        </button>
      </div>
    </div>,
    document.getElementById("modal-root")
  );
}
```

---

## Attach Compound Components

```jsx
Modal.Trigger = Trigger;
Modal.Content = Content;
```

Usage:

```jsx
<Modal>
  <Modal.Trigger>
    Open Modal
  </Modal.Trigger>

  <Modal.Content>
    <h2>Hello!</h2>
    <p>This is a reusable modal.</p>
  </Modal.Content>
</Modal>
```

Here we used:

```text
Composition
      +
Compound Components
      +
Context
      +
Portal
```

This is a good example of how advanced React concepts work together.

---

# 13. Quick Revision

| Concept             | Remember                                                |
| ------------------- | ------------------------------------------------------- |
| Error Boundary      | Catches rendering errors and displays fallback UI       |
| Portal              | Renders React UI into another DOM node                  |
| `useRef`            | Accesses DOM or stores mutable values without re-render |
| Forwarding Ref      | Passes a ref through a custom component                 |
| HOC                 | Function that enhances a component                      |
| Render Props        | Function prop used to control rendering                 |
| Compound Components | Related components sharing state/behavior               |
| Composition         | Build complex UI by combining components                |
| Custom Hook         | Reusable stateful React logic                           |

---

# Advanced React Mental Map

```text
                     ADVANCED REACT
                           │
       ┌───────────────────┼───────────────────┐
       ↓                   ↓                   ↓
   Error Handling       DOM / UI          Reusability
       │                   │                   │
       ↓             ┌─────┼─────┐       ┌─────┼─────┐
 Error Boundary      ↓     ↓     ↓       ↓     ↓     ↓
                   Refs Portal Forward  HOC Render Compound
                          Ref           Props Components
                                                       │
                                                       ↓
                                                 Composition
                                                       │
                                                       ↓
                                                 Custom Hooks
```

---

# 14. Interview Questions

## Basic

1. What is an Error Boundary?
2. What errors can Error Boundaries catch?
3. What is a React Portal?
4. Why are Portals useful for modals?
5. What is `useRef()`?
6. What is the difference between `useRef()` and `useState()`?
7. What is ref forwarding?
8. What is a Higher-Order Component?
9. What is a Render Prop?
10. What are Compound Components?
11. What is component composition?
12. What is a Custom Hook?

---

## Intermediate

13. Why are Error Boundaries implemented using class components traditionally?
14. Do Error Boundaries catch errors inside event handlers?
15. How does event propagation work with React Portals?
16. When should you use a ref instead of state?
17. What problem does ref forwarding solve?
18. What is the difference between HOC and Custom Hooks?
19. What is the difference between Render Props and Custom Hooks?
20. How does Context help implement Compound Components?
21. Why does React favor composition over inheritance?
22. How do Custom Hooks share logic without sharing state?
23. Can two components using the same Custom Hook share the same state?

---

## Advanced

24. Design a reusable Modal using Compound Components.
25. How would you expose only specific methods through a ref?
26. How would you build a reusable form library using Custom Hooks?
27. How would you migrate an old HOC-based architecture to Custom Hooks?
28. What are the disadvantages of HOCs?
29. What are the disadvantages of Render Props?
30. How can deeply nested components communicate without prop drilling?
31. How would you build a reusable Tabs component?
32. How would you combine Context, Compound Components, and Custom Hooks?
33. How would you isolate failures in different sections of a large React application?
34. How would you design a reusable component library using composition?

---

# Final Memory Trick

```text
Error Boundary
→ Protect the UI

Portal
→ Change DOM location

Ref
→ Access DOM / mutable value

Forward Ref
→ Pass ref through component

HOC
→ Enhance component

Render Props
→ Share logic through function

Compound Components
→ Related components work together

Composition
→ Build big components from small components

Custom Hooks
→ Reuse stateful logic
```

> **Advanced React is mainly about separating concerns: isolate errors, control DOM access, reuse logic, and design flexible components that can be composed together.**
