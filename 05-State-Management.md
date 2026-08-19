# React A→Z Revision

**Phase 5 — State Management 🧠**

> State management is the process of storing, updating, sharing, and synchronizing data required by a React application.

---

## 📚 Contents

1. [What is State Management?](#1-what-is-state-management)
2. [Local State](#2-local-state)
3. [Lifting State Up](#3-lifting-state-up)
4. [Context API](#4-context-api)
5. [useReducer](#5-usereducer)
6. [Redux Toolkit](#6-redux-toolkit)
7. [Zustand](#7-zustand)
8. [Server State](#8-server-state)
9. [Client State vs Server State](#9-client-state-vs-server-state)
10. [Choosing a State Management Solution](#10-choosing-a-state-management-solution)
11. [Common State Management Mistakes](#11-common-state-management-mistakes)
12. [Quick Revision](#12-quick-revision)
13. [Interview Revision](#13-interview-revision)

---

# 1. What is State Management?

## Definition

**State management** is the process of controlling application data that can change over time and determining how that data is stored, updated, and shared between components.

Examples of state:

```text
User information
Shopping cart
Theme
Authentication status
Form data
Modal visibility
Search filters
API data
```

---

## Simple Explanation

Imagine an e-commerce application:

```text
              Application State
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
     User           Cart         Products
       │             │             │
       ↓             ↓             ↓
   Logged in      3 items      API data
```

Different parts of the application may need access to different pieces of state.

The main question is:

> **Where should this state live?**

---

## Real-Life Example

Think of a company.

```text
Company
│
├── Employee's personal information
│      → Local
│
├── Department information
│      → Shared within department
│
├── Company-wide policies
│      → Shared globally
│
└── Data from external systems
       → Server state
```

React applications have similar levels of state.

---

# 2. Local State

## Definition

**Local state** is state owned and managed by a specific component.

The most common implementation uses `useState`.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <h1>{count}</h1>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </>
  );
}
```

Here:

```text
Counter
  │
  └── count
       ↓
   Local State
```

---

## Real-Life Example

Think about a **personal notebook**.

Only you need the information:

```text
My Notes
├── Today's tasks
├── Personal reminders
└── Shopping list
```

There is no reason to put this information on a company-wide notice board.

Similarly, if only one component needs some state, keep it local.

---

## Examples of Local State

```text
Input value
Modal open/closed
Dropdown open/closed
Counter
Selected tab
Temporary form state
```

Example:

```jsx
function Modal() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <button onClick={() => setIsOpen(true)}>
        Open
      </button>

      {isOpen && (
        <div>
          Modal Content
        </div>
      )}
    </>
  );
}
```

---

## Advantages

* Simple
* Easy to understand
* Easy to maintain
* Less unnecessary sharing
* Keeps state close to where it is used

---

## Rule of Thumb

> **Keep state as close as possible to the components that need it.**

Don't move state to a global store just because you can.

---

# 3. Lifting State Up

## Definition

**Lifting state up** means moving shared state from child components to their closest common parent so multiple children can access and update the same source of truth.

---

## Problem

Suppose:

```text
Parent
├── Input
└── Preview
```

Both components need the same data.

If the state exists inside `Input`:

```text
Parent
├── Input
│    └── state ❌
└── Preview
```

`Preview` cannot directly access that state.

---

## Solution

Move the state to the parent:

```text
Parent
└── state
    ├── Input
    └── Preview
```

---

## Real-Life Example

Imagine two employees need access to the same document.

Instead of keeping two different copies:

```text
Employee A → Document A
Employee B → Document B
```

create one shared document:

```text
             Shared Document
               /        \
              ↓          ↓
        Employee A    Employee B
```

Both work with the same source of truth.

---

## Example

```jsx
import { useState } from "react";

function Parent() {
  const [name, setName] = useState("");

  return (
    <>
      <Input
        name={name}
        setName={setName}
      />

      <Preview name={name} />
    </>
  );
}

function Input({ name, setName }) {
  return (
    <input
      value={name}
      onChange={e => setName(e.target.value)}
    />
  );
}

function Preview({ name }) {
  return <h2>Hello {name}</h2>;
}
```

Data flow:

```text
             Parent
               │
          name + setName
           /          \
          ↓            ↓
       Input         Preview
          │
          └── updates state
```

---

## When to Lift State Up

Lift state when:

* Multiple components need the same data.
* Components need to stay synchronized.
* Sibling components need to communicate.
* You need one source of truth.

---

# 4. Context API

## Definition

The **Context API** allows data to be made available to components deep within a component tree without manually passing props through every intermediate component.

---

## Prop Drilling Problem

Consider:

```text
App
 ↓
Navbar
 ↓
UserMenu
 ↓
Avatar
```

Suppose `Avatar` needs the user.

Without Context:

```text
App
 ↓ user
Navbar
 ↓ user
UserMenu
 ↓ user
Avatar
```

`Navbar` and `UserMenu` may not even use `user`.

This is called **prop drilling**.

---

## Context Solution

```text
             UserContext
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Navbar    UserMenu    Avatar
                            │
                            ↓
                       useContext()
```

---

## Create Context

```jsx
import { createContext } from "react";

const UserContext = createContext(null);
```

---

## Provide Context

Traditional form:

```jsx
<UserContext.Provider value={user}>
  <App />
</UserContext.Provider>
```

Modern React also supports:

```jsx
<UserContext value={user}>
  <App />
</UserContext>
```

---

## Consume Context

```jsx
import { useContext } from "react";

function Avatar() {
  const user = useContext(UserContext);

  return (
    <img
      src={user.avatar}
      alt={user.name}
    />
  );
}
```

---

## Real-Life Example

Think about a **school announcement system**.

Without a central announcement:

```text
Principal
 ↓
Teacher 1 → Student
Teacher 2 → Student
Teacher 3 → Student
```

With a central announcement system:

```text
             Announcement
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Class A    Class B    Class C
```

Context works similarly for values needed by many descendants.

---

## Good Context Use Cases

```text
Theme
Authentication information
Current user
Language
Permissions
Application configuration
```

---

## Context Is Not Always a Global Store

Context solves:

> **How can components access a shared value?**

It does not automatically solve:

> **How should complex application state be modeled, updated, persisted, cached, or synchronized?**

For complex state management, tools such as Redux Toolkit or Zustand may be more appropriate.

---

# 5. useReducer

## Definition

`useReducer` manages state using a **reducer function** and **actions**.

```jsx
const [state, dispatch] = useReducer(
  reducer,
  initialState
);
```

---

## Core Flow

```text
User Action
    ↓
dispatch(action)
    ↓
Reducer
    ↓
New State
    ↓
Component renders
```

---

## Real-Life Example

Think about an ATM.

You don't directly modify your bank balance.

You perform an action:

```text
Withdraw ₹500
```

The system processes the action:

```text
Action
  ↓
Bank Rules
  ↓
New Balance
```

Similarly:

```text
dispatch({
  type: "withdraw",
  payload: 500
})
```

goes through the reducer.

---

## Example

```jsx
import { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return {
        count: state.count + 1
      };

    case "decrement":
      return {
        count: state.count - 1
      };

    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(
    reducer,
    { count: 0 }
  );

  return (
    <>
      <h1>{state.count}</h1>

      <button
        onClick={() =>
          dispatch({ type: "increment" })
        }
      >
        +
      </button>

      <button
        onClick={() =>
          dispatch({ type: "decrement" })
        }
      >
        -
      </button>
    </>
  );
}
```

---

## When to Use `useReducer`

Use it when:

```text
Many state values
        +
Many possible actions
        +
Complex state transitions
```

For simple state:

```jsx
const [count, setCount] = useState(0);
```

is usually easier.

---

# 6. Redux Toolkit

## Definition

**Redux Toolkit (RTK)** is the official recommended way to write Redux logic. It provides utilities for creating stores, slices, reducers, and actions with less boilerplate.

Redux is useful when application state is:

* Shared across many parts of the application
* Complex
* Frequently updated
* Dependent on predictable state transitions
* Easier to manage from a centralized store

---

## Redux Architecture

```text
                Redux Store
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
      User         Cart        Products
       │            │            │
       └────────────┼────────────┘
                    ↓
               Components
                    │
                    ↓
                  UI
```

---

## Real-Life Example

Think of a **central company database**.

Instead of every department maintaining separate copies:

```text
HR → Employee Data
Sales → Employee Data
Admin → Employee Data
```

there is a centralized source:

```text
              Central Store
             /      |       \
            ↓       ↓        ↓
           HR     Sales     Admin
```

Redux provides a centralized state store.

---

# 6.1 Redux Toolkit Setup

Install:

```bash
npm install @reduxjs/toolkit react-redux
```

---

## Create a Slice

A **slice** represents one logical section of Redux state.

```jsx
import {
  createSlice
} from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",

  initialState: {
    value: 0
  },

  reducers: {
    increment: state => {
      state.value += 1;
    },

    decrement: state => {
      state.value -= 1;
    }
  }
});

export const {
  increment,
  decrement
} = counterSlice.actions;

export default counterSlice.reducer;
```

---

## Create Store

```jsx
import {
  configureStore
} from "@reduxjs/toolkit";

import counterReducer
  from "./counterSlice";

export const store = configureStore({
  reducer: {
    counter: counterReducer
  }
});
```

---

## Provide Store

```jsx
import {
  Provider
} from "react-redux";

import { store } from "./store";

function Root() {
  return (
    <Provider store={store}>
      <App />
    </Provider>
  );
}
```

---

## Read State

```jsx
import {
  useSelector
} from "react-redux";

function Counter() {
  const count = useSelector(
    state => state.counter.value
  );

  return <h1>{count}</h1>;
}
```

---

## Dispatch Action

```jsx
import {
  useDispatch
} from "react-redux";

import {
  increment
} from "./counterSlice";

function Counter() {
  const dispatch = useDispatch();

  return (
    <button
      onClick={() => dispatch(increment())}
    >
      +
    </button>
  );
}
```

---

## Redux Flow

```text
User clicks button
       ↓
dispatch(increment())
       ↓
Action
       ↓
Reducer
       ↓
Redux Store updates
       ↓
Subscribed component re-renders
       ↓
UI updates
```

---

## Why Can Redux Reducers "Mutate" State?

This code looks like direct mutation:

```jsx
state.value += 1;
```

But Redux Toolkit uses **Immer** internally.

Immer tracks the apparent mutations and produces an immutable next state.

So you get concise reducer code while preserving Redux's immutable state update model.

---

## Redux Toolkit Concepts

```text
Store
 ↓
Central state container

Slice
 ↓
State + reducers + actions for one feature

Action
 ↓
Describes what happened

Reducer
 ↓
Calculates next state

Dispatch
 ↓
Sends an action

Selector
 ↓
Reads state
```

---

# 7. Zustand

## Definition

**Zustand** is a lightweight state-management library that provides a simple store-based API, commonly using Hooks to read and update state.

---

## Real-Life Example

Imagine a small office.

Instead of creating a large administrative system:

```text
Central Government
   ↓
Multiple Departments
   ↓
Multiple Forms
   ↓
Multiple Processes
```

you have a small shared board:

```text
Shared Store
   ├── User
   ├── Cart
   └── Theme
```

Components can subscribe directly to the pieces they need.

---

## Basic Zustand Store

Install:

```bash
npm install zustand
```

Create store:

```jsx
import {
  create
} from "zustand";

const useCounterStore = create(
  set => ({
    count: 0,

    increment: () =>
      set(state => ({
        count: state.count + 1
      })),

    decrement: () =>
      set(state => ({
        count: state.count - 1
      }))
  })
);

export default useCounterStore;
```

---

## Use Store

```jsx
function Counter() {
  const count =
    useCounterStore(state => state.count);

  const increment =
    useCounterStore(
      state => state.increment
    );

  return (
    <>
      <h1>{count}</h1>

      <button onClick={increment}>
        +
      </button>
    </>
  );
}
```

---

## Zustand Mental Model

```text
              Zustand Store
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
      count      user       cart
        │          │          │
        ↓          ↓          ↓
   Component A Component B Component C
```

Components subscribe to the state they select.

---

## Redux Toolkit vs Zustand

| Feature              | Redux Toolkit     | Zustand     |
| -------------------- | ----------------- | ----------- |
| Setup                | More structured   | Very simple |
| Boilerplate          | Moderate          | Low         |
| Architecture         | Highly structured | Flexible    |
| DevTools             | Excellent         | Available   |
| Middleware ecosystem | Large             | Smaller     |
| Large applications   | Excellent         | Good        |
| Small/medium apps    | Good              | Excellent   |
| Learning curve       | Higher            | Lower       |
| Centralized store    | Yes               | Yes         |

---

## When to Choose Zustand

Good choice when you want:

* Simple global state
* Minimal boilerplate
* Lightweight API
* Easy component subscriptions
* Flexible architecture

---

# 8. Server State

## Definition

**Server state** is data that comes from an external server/API and whose authoritative source is the server.

Examples:

```text
Users from database
Products
Orders
Posts
Comments
Notifications
Analytics
```

---

## Client State vs Server State

Client state:

```text
Modal open
Selected tab
Theme
Input value
Sidebar open
```

Server state:

```text
Products from API
User profile from API
Orders from database
```

---

## Real-Life Example

Think about a restaurant.

### Client State

Your current table:

```text
Selected table
Number of guests
Selected dish
```

This belongs to your current interaction.

### Server State

The restaurant's database:

```text
Menu
Available tables
Orders
Customer records
```

The restaurant is the authoritative source.

Similarly:

```text
Browser
  ↓
Client State

Server
  ↓
Server State
```

---

# 8.1 Why Server State Is Different

Server data has additional concerns:

```text
Fetching
Caching
Refetching
Loading
Errors
Stale data
Retries
Pagination
Deduplication
Mutations
Synchronization
```

A normal `useState` doesn't automatically solve these.

---

## Basic Fetch Example

```jsx
function Users() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] =
    useState(true);
  const [error, setError] =
    useState(null);

  useEffect(() => {
    async function fetchUsers() {
      try {
        const response =
          await fetch("/api/users");

        const data =
          await response.json();

        setUsers(data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    }

    fetchUsers();
  }, []);

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error loading users.</p>;
  }

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>
          {user.name}
        </li>
      ))}
    </ul>
  );
}
```

This works, but as an application grows, server-state management becomes more complex.

---

# 8.2 Server-State Libraries

Libraries such as **TanStack Query** can manage concerns such as:

```text
Fetching
Caching
Refetching
Stale data
Retries
Mutations
Pagination
Query invalidation
```

Example:

```jsx
import {
  useQuery
} from "@tanstack/react-query";

function Users() {
  const {
    data,
    isPending,
    error
  } = useQuery({
    queryKey: ["users"],
    queryFn: fetchUsers
  });

  if (isPending) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error!</p>;
  }

  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>
          {user.name}
        </li>
      ))}
    </ul>
  );
}
```

---

## Server State Mental Model

```text
             Server
               │
               │ API
               ↓
        Server State Layer
               │
      ┌────────┼────────┐
      ↓        ↓        ↓
    Cache    Fetch    Mutation
      │
      ↓
  React Components
```

---

# 9. Client State vs Server State

This distinction is extremely important for modern React applications.

| Client State                        | Server State                        |
| ----------------------------------- | ----------------------------------- |
| Owned by client                     | Owned by server                     |
| UI/application state                | Remote data                         |
| Usually synchronous                 | Usually asynchronous                |
| Example: modal                      | Example: users                      |
| Example: theme                      | Example: products                   |
| Example: selected tab               | Example: orders                     |
| `useState` works well               | Query libraries often help          |
| Context/Redux/Zustand can manage it | TanStack Query / RTK Query can help |

---

## Example

Imagine an e-commerce app:

```text
Client State
├── Cart UI open?
├── Selected category
├── Dark mode
├── Search input
└── Selected product

Server State
├── Products
├── User profile
├── Orders
├── Reviews
└── Inventory
```

---

# 10. Choosing a State Management Solution

Don't immediately choose Redux or Zustand.

First ask:

> **Who needs this state?**

---

## Case 1 — Only One Component

Use:

```jsx
useState
```

Example:

```text
Modal visibility
```

```jsx
const [isOpen, setIsOpen] = useState(false);
```

---

## Case 2 — Parent + Children

Use:

```text
Local State
+
Props
```

or lift state up.

---

## Case 3 — Sibling Components

Lift state to the closest common parent.

```text
        Parent
        State
       /     \
      ↓       ↓
   Child A  Child B
```

---

## Case 4 — Many Descendants

Consider:

```text
Context API
```

Example:

```text
Theme
Current User
Language
```

---

## Case 5 — Complex Local State

Use:

```text
useReducer
```

---

## Case 6 — Complex Global Client State

Consider:

```text
Redux Toolkit
Zustand
```

Choose based on project requirements and team preferences.

---

## Case 7 — Server Data

Consider:

```text
TanStack Query
RTK Query
```

rather than automatically placing all API data into a client-state store.

---

# 11. Common State Management Mistakes

## Mistake 1 — Making Everything Global

❌

```text
Every input
Every modal
Every dropdown
Every temporary value
       ↓
Global Store
```

This makes applications harder to reason about.

Prefer:

```text
Local state → local concerns
Global state → genuinely shared concerns
```

---

## Mistake 2 — Using Context for Everything

Context is useful, but putting a large frequently changing state tree into one context can cause broad consumer updates and make the architecture harder to maintain.

Use separate contexts or a dedicated state library when appropriate.

---

## Mistake 3 — Storing All API Data in Redux

For server state, ask whether a query/cache solution is more appropriate.

Instead of:

```text
API
 ↓
Redux
 ↓
Manually manage
 ├── loading
 ├── errors
 ├── cache
 ├── stale data
 └── refetch
```

a server-state library can handle many of these concerns.

---

## Mistake 4 — Duplicating State

Avoid storing data that can be derived.

❌

```jsx
const [firstName, setFirstName] = useState("Aniket");
const [lastName, setLastName] = useState("Kole");
const [fullName, setFullName] = useState("Aniket Kole");
```

`fullName` can be derived:

```jsx
const fullName = `${firstName} ${lastName}`;
```

---

## Mistake 5 — Multiple Sources of Truth

Avoid:

```text
Component A → user
Component B → user
Redux → user
Context → user
```

Prefer one clear source of truth where possible.

```text
             User State
                 │
        ┌────────┼────────┐
        ↓        ↓        ↓
     Component Component Component
```

---

# 12. Quick Revision

## State Management Hierarchy

```text
                    STATE
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
     Local          Shared         Server
       │              │              │
       ↓              ↓              ↓
   useState       Context /       API Data
                  useReducer          │
                                     ↓
                              Query Libraries
```

---

## Local State

```text
One component needs it
        ↓
useState
```

---

## Lifting State

```text
Sibling components need same state
        ↓
Move state to common parent
```

---

## Context

```text
Many descendants need same value
        ↓
Context API
```

---

## useReducer

```text
Complex state transitions
        ↓
useReducer
```

---

## Redux Toolkit

```text
Complex shared client state
        ↓
Redux Toolkit
```

---

## Zustand

```text
Simple shared client state
        ↓
Zustand
```

---

## Server State

```text
Data belongs to server
        ↓
Query/cache solution
        ↓
TanStack Query / RTK Query
```

---

# 13. Interview Revision

### Q1. What is state management?

State management is the process of storing, updating, sharing, and synchronizing application data that changes over time.

---

### Q2. What is local state?

State owned by a specific component and primarily used by that component.

```jsx
const [count, setCount] = useState(0);
```

---

### Q3. What is lifting state up?

Moving state to the closest common parent so multiple child components can share the same source of truth.

---

### Q4. When should you lift state up?

When two or more components need to access or modify the same state.

---

### Q5. What problem does Context API solve?

It reduces the need to pass props through intermediate components when many descendants need access to the same value.

---

### Q6. Is Context a state-management library?

Context is a React mechanism for making values available through a component tree. It can be combined with state Hooks to implement shared state, but it isn't a complete state-management solution by itself.

---

### Q7. When should you use `useReducer` instead of `useState`?

Use `useReducer` when state transitions become complex or many actions modify related state.

---

### Q8. What is Redux Toolkit?

Redux Toolkit is the official recommended way to write Redux logic and provides utilities such as `configureStore` and `createSlice` to reduce boilerplate.

---

### Q9. What is a Redux store?

The Redux store is the centralized container that holds the application's Redux state.

---

### Q10. What is a Redux action?

An action is an object describing an event or state change.

```jsx
{
  type: "cart/addItem",
  payload: product
}
```

---

### Q11. What is a reducer?

A reducer is a function that receives the current state and an action and determines the next state.

```jsx
function reducer(state, action) {
  // return next state
}
```

---

### Q12. What is Zustand?

Zustand is a lightweight state-management library that provides a simple store-based API for shared client state.

---

### Q13. Redux Toolkit vs Zustand?

```text
Redux Toolkit
→ More structured
→ Larger ecosystem
→ Strong conventions
→ Excellent for complex applications

Zustand
→ Smaller API
→ Less boilerplate
→ Flexible
→ Convenient for lightweight shared state
```

Neither is universally "better"; the appropriate choice depends on application requirements.

---

### Q14. What is server state?

Server state is data whose authoritative source is an external server.

Examples:

```text
Users
Products
Orders
Posts
Comments
```

---

### Q15. Why is server state different from client state?

Because server state is:

```text
Asynchronous
Remote
Potentially stale
Shared between clients
Subject to refetching
Cacheable
```

Client state is usually controlled directly by the application running in the browser.

---

### Q16. Should API data always be stored in Redux?

No.

For server state, a query/cache solution such as TanStack Query or RTK Query may be more appropriate.

---

### Q17. What is the difference between Redux and Context?

| Context                             | Redux Toolkit                                     |
| ----------------------------------- | ------------------------------------------------- |
| Built into React                    | External library                                  |
| Provides values through tree        | Centralized store                                 |
| Good for shared context values      | Good for complex global client state              |
| Minimal API                         | More structured architecture                      |
| No built-in complete state workflow | Actions, reducers, middleware, DevTools ecosystem |

---

### Q18. What is the most important state-management principle?

> **Keep state at the lowest level that can correctly own it.**

Don't make state global unless multiple parts of the application genuinely need it.

---

# 🧠 Final Mental Model

```text
                         STATE MANAGEMENT
                                │
             ┌──────────────────┼──────────────────┐
             ↓                  ↓                  ↓
        CLIENT STATE       SHARED STATE       SERVER STATE
             │                  │                  │
             ↓                  ↓                  ↓
         useState          Lifting State        API
             │                  │                  │
             │               Context              ↓
             │                  │           Query Library
             │               useReducer            │
             │                  │                  │
             │          Redux Toolkit              │
             │             / Zustand               │
             │                  │                  │
             └──────────────────┼──────────────────┘
                                ↓
                         React Components
                                ↓
                                UI
```

---

# 🔥 State Management Decision Tree

```text
Where does the data come from?
            │
      ┌─────┴─────┐
      ↓           ↓
   Client       Server
      │           │
      │           ↓
      │      API / Database
      │           │
      │           ↓
      │    Query / Cache Layer
      │
      ↓
Who needs it?
      │
 ┌────┼───────────────┐
 ↓    ↓               ↓
One  Few          Many/Global
 │    │               │
 ↓    ↓               ↓
useState   Lift State   Context
                       │
                       ↓
                Complex Updates?
                  │         │
                 Yes        No
                  ↓          ↓
             useReducer   Context
                  │
                  ↓
           Larger Client App?
                  │
             ┌────┴────┐
             ↓         ↓
            Yes        No
             ↓          ↓
       Redux Toolkit  Zustand
```

---

# ⭐ One-Line Cheat Sheet

```text
Local State
→ State needed by one component.

Lifting State Up
→ Move shared state to the closest common parent.

Context API
→ Share values through a component tree without prop drilling.

useReducer
→ Manage complex state transitions with actions and a reducer.

Redux Toolkit
→ Structured centralized state management for complex client state.

Zustand
→ Lightweight store-based state management with minimal boilerplate.

Server State
→ Remote data owned by an external server.

TanStack Query / RTK Query
→ Tools for fetching, caching, synchronizing, and managing server state.
```

> **Core idea:** Don't ask "Which state-management library should I use?" first. Ask **"What kind of state is this, who needs it, and who owns the source of truth?"** Then choose the simplest solution that satisfies those requirements.
