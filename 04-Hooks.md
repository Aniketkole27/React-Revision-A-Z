# React A→Z Revision

**Phase 4 — Hooks ⭐**

> Hooks allow function components to use React features such as state, context, refs, effects, and performance optimizations.

---

## 📚 Contents

1. [What are Hooks?](#1-what-are-hooks)
2. [Rules of Hooks](#2-rules-of-hooks)
3. [useState](#3-usestate)
4. [useEffect](#4-useeffect)
5. [useContext](#5-usecontext)
6. [useReducer](#6-usereducer)
7. [useRef](#7-useref)
8. [useMemo](#8-usememo)
9. [useCallback](#9-usecallback)
10. [useLayoutEffect](#10-uselayouteffect)
11. [useImperativeHandle](#11-useimperativehandle)
12. [useId](#12-useid)
13. [useTransition](#13-usetransition)
14. [useDeferredValue](#14-usedeferredvalue)
15. [useSyncExternalStore](#15-usesyncexternalstore)
16. [useInsertionEffect](#16-useinsertioneffect)
17. [Custom Hooks](#17-custom-hooks)
18. [Hooks Comparison](#18-hooks-comparison)
19. [Common Hook Mistakes](#19-common-hook-mistakes)
20. [Quick Revision](#20-quick-revision)
21. [Interview Revision](#21-interview-revision)

---

# 1. What are Hooks?

## Definition

**Hooks** are special React functions that allow function components to use React features such as:

* State
* Effects
* Context
* Refs
* Performance optimizations
* Concurrent rendering features

Examples:

```jsx
useState()
useEffect()
useContext()
useRef()
useMemo()
```

---

## Why Were Hooks Introduced?

Before Hooks, stateful logic was commonly associated with **class components**.

Hooks allow function components to use state and other React features without needing classes.

Instead of:

```jsx
class Counter extends React.Component {
  // ...
}
```

we can write:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return <h1>{count}</h1>;
}
```

---

## Real-Life Example

Think of Hooks as **tools in a toolbox**.

```text
React Component
      │
      ├── useState
      │      → Store changing data
      │
      ├── useEffect
      │      → Synchronize with external systems
      │
      ├── useRef
      │      → Hold values / DOM references
      │
      ├── useContext
      │      → Access shared context
      │
      └── useMemo
             → Cache calculations
```

You choose the appropriate tool depending on the problem.

---

# 2. Rules of Hooks

Hooks have strict rules.

React's official Rules of Hooks are:

1. **Only call Hooks at the top level.**
2. **Only call Hooks from React functions.**

---

## Rule 1: Call Hooks at the Top Level

✅ Correct:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return <h1>{count}</h1>;
}
```

❌ Don't put Hooks inside conditions:

```jsx
function Counter({ enabled }) {
  if (enabled) {
    const [count, setCount] = useState(0);
  }

  return <h1>Hello</h1>;
}
```

---

## Don't Use Hooks Inside Loops

❌ Incorrect:

```jsx
function App() {
  for (let i = 0; i < 5; i++) {
    useState(0);
  }

  return <div />;
}
```

---

## Don't Use Hooks Inside Event Handlers

❌ Incorrect:

```jsx
function App() {
  function handleClick() {
    const [count, setCount] = useState(0);
  }

  return <button onClick={handleClick}>Click</button>;
}
```

---

## Rule 2: Only Call Hooks from React Functions

Hooks can be called from:

### Function Components

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return <h1>{count}</h1>;
}
```

### Custom Hooks

```jsx
function useCounter() {
  const [count, setCount] = useState(0);

  return { count, setCount };
}
```

Don't call Hooks from ordinary JavaScript functions.

---

## Why Do These Rules Exist?

React needs Hooks to be called in a **consistent order between renders**.

Conceptually:

```text
Render 1:
Hook 1 → useState
Hook 2 → useEffect
Hook 3 → useRef

Render 2:
Hook 1 → useState
Hook 2 → useEffect
Hook 3 → useRef
```

If Hooks were conditionally called:

```text
Render 1:
Hook 1 → useState
Hook 2 → useEffect

Render 2:
Hook 1 → useState
Hook 2 → useRef
```

React could no longer reliably associate each Hook call with its stored data.

---

# 3. useState

## Definition

`useState` is a Hook used to add **state** to a function component.

```jsx
const [state, setState] = useState(initialValue);
```

---

## Syntax

```jsx
const [count, setCount] = useState(0);
```

Here:

```text
count       → current state
setCount    → state updater
0           → initial state
```

---

## Real-Life Example

Think of a **digital score counter**.

Initially:

```text
Score = 0
```

Player scores:

```text
Score = 1
```

Another point:

```text
Score = 2
```

The score is changing data, so it belongs in state.

---

## Basic Example

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>{count}</h1>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

---

## Functional State Update

When the next state depends on the previous state:

```jsx
setCount(prev => prev + 1);
```

Example:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  function incrementTwice() {
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
  }

  return (
    <>
      <h1>{count}</h1>
      <button onClick={incrementTwice}>
        +2
      </button>
    </>
  );
}
```

---

## Object State

```jsx
function User() {
  const [user, setUser] = useState({
    name: "Aniket",
    age: 21
  });

  function increaseAge() {
    setUser(prev => ({
      ...prev,
      age: prev.age + 1
    }));
  }

  return (
    <>
      <h2>{user.name}</h2>
      <p>{user.age}</p>

      <button onClick={increaseAge}>
        Increase Age
      </button>
    </>
  );
}
```

---

## Array State

```jsx
function Todo() {
  const [todos, setTodos] = useState([]);

  function addTodo() {
    setTodos(prev => [
      ...prev,
      "Learn React"
    ]);
  }

  return (
    <>
      <button onClick={addTodo}>
        Add Todo
      </button>

      {todos.map((todo, index) => (
        <p key={index}>{todo}</p>
      ))}
    </>
  );
}
```

For dynamic lists, prefer a stable unique ID as the key.

---

## Important Points

* `useState` stores component state.
* Updating state schedules a render.
* Don't mutate state directly.
* Use the setter function.
* Use functional updates when the new state depends on previous state.

---

# 4. useEffect

## Definition

`useEffect` lets a component **synchronize with an external system**.

External systems can include:

* Browser APIs
* Timers
* Event subscriptions
* Network connections
* Third-party libraries
* Non-React widgets
* DOM integrations

React's current documentation emphasizes that Effects are for synchronization with external systems, not for ordinary application data flow.

---

## Syntax

```jsx
useEffect(() => {
  // setup

  return () => {
    // cleanup
  };
}, [dependencies]);
```

---

## Real-Life Example

Imagine a **phone connecting to Bluetooth headphones**.

```text
Phone
 ↓
Connect
 ↓
Headphones
```

When the headphones are no longer needed:

```text
Disconnect
```

Similarly:

```text
Component appears
      ↓
Effect setup
      ↓
External system connected
      ↓
Dependencies change / component removed
      ↓
Cleanup
```

---

## Basic Example

```jsx
import { useEffect } from "react";

function App() {
  useEffect(() => {
    console.log("Effect executed");

    return () => {
      console.log("Cleanup executed");
    };
  }, []);

  return <h1>Hello</h1>;
}
```

---

## Dependency Array

### No dependency array

```jsx
useEffect(() => {
  console.log("Runs after every commit");
});
```

---

### Empty dependency array

```jsx
useEffect(() => {
  console.log("Runs after initial mount");
}, []);
```

The setup is not re-run because of later prop/state changes.

In development Strict Mode, React may perform an extra setup → cleanup → setup cycle to verify cleanup logic.

---

### With dependencies

```jsx
useEffect(() => {
  console.log("User changed");
}, [userId]);
```

The Effect re-runs after a commit when `userId` has changed.

React compares dependencies using `Object.is`.

---

## Cleanup Function

Example with an event listener:

```jsx
useEffect(() => {
  function handleResize() {
    console.log(window.innerWidth);
  }

  window.addEventListener(
    "resize",
    handleResize
  );

  return () => {
    window.removeEventListener(
      "resize",
      handleResize
    );
  };
}, []);
```

Flow:

```text
Mount
 ↓
Add event listener
 ↓
Component exists
 ↓
Unmount
 ↓
Remove event listener
```

---

## Timer Example

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Running...");
  }, 1000);

  return () => {
    clearInterval(timer);
  };
}, []);
```

---

## API Example

```jsx
function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function fetchUsers() {
      const response = await fetch("/api/users");
      const data = await response.json();

      setUsers(data);
    }

    fetchUsers();
  }, []);

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

For production applications, dedicated data-fetching libraries can provide caching, deduplication, and other capabilities that raw Effects don't provide automatically.

---

## Important Rule

Don't use `useEffect` just because you need to calculate something.

❌ Unnecessary:

```jsx
useEffect(() => {
  setFullName(firstName + " " + lastName);
}, [firstName, lastName]);
```

Better:

```jsx
const fullName = `${firstName} ${lastName}`;
```

If something can be calculated during rendering, an Effect is usually unnecessary.

---

# 5. useContext

## Definition

`useContext` lets a component read and subscribe to a **Context value** without manually passing props through every intermediate component.

---

## Problem: Prop Drilling

Imagine:

```text
App
 ↓
Navbar
 ↓
UserMenu
 ↓
Avatar
```

If `Avatar` needs the current user:

```text
App
 ↓ user
Navbar
 ↓ user
UserMenu
 ↓ user
Avatar
```

Even if `Navbar` and `UserMenu` don't need the user themselves, they must pass it through.

This is called **prop drilling**.

---

## Context Solution

```text
       Context Provider
             │
       ┌─────┼─────┐
       ↓     ↓     ↓
     Navbar Menu Avatar
```

Components can read the context directly.

---

## Create Context

```jsx
import {
  createContext,
  useContext
} from "react";

const ThemeContext = createContext("light");
```

---

## Provide Context

Modern React supports using the context object itself as a provider:

```jsx
<ThemeContext value="dark">
  <App />
</ThemeContext>
```

The traditional form remains:

```jsx
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>
```

Both concepts provide a context value to descendants.

---

## Consume Context

```jsx
function Button() {
  const theme = useContext(ThemeContext);

  return (
    <button className={theme}>
      Click Me
    </button>
  );
}
```

---

## Real-Life Example

Think of a **company notice board**.

Instead of personally telling every employee:

```text
Employee 1 → company announcement
Employee 2 → company announcement
Employee 3 → company announcement
```

the company puts the information in one central place:

```text
Company Context
      ↓
Employees can read it
```

Context works similarly.

---

## Common Context Examples

* Theme
* Current authenticated user
* Language
* App configuration
* Permissions

---

## Important Point

Context is not automatically a replacement for all state-management libraries.

Use it when a value needs to be available to many components in a subtree.

---

# 6. useReducer

## Definition

`useReducer` is a Hook for managing state using a **reducer function** and dispatched actions.

Syntax:

```jsx
const [state, dispatch] = useReducer(
  reducer,
  initialState
);
```

---

## Basic Concept

```text
Action
  ↓
dispatch()
  ↓
Reducer
  ↓
New State
  ↓
UI
```

---

## Real-Life Example

Think of a **bank transaction system**.

You don't directly modify the account balance.

You submit an action:

```text
DEPOSIT ₹500
```

The system processes it:

```text
Action
 ↓
Bank Rules
 ↓
New Balance
```

Similarly:

```text
dispatch(action)
       ↓
    reducer
       ↓
   new state
```

---

## Basic Example

```jsx
import {
  useReducer
} from "react";

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

## Reducer

A reducer is a function:

```jsx
function reducer(state, action) {
  // calculate next state
}
```

It receives:

```text
state
action
```

and returns:

```text
new state
```

---

## Action

An action describes **what happened**.

```jsx
dispatch({
  type: "increment"
});
```

With data:

```jsx
dispatch({
  type: "addTodo",
  payload: {
    id: 1,
    title: "Learn React"
  }
});
```

---

## When Should You Use `useReducer`?

Use it when:

* State has multiple related values.
* Many actions can change the state.
* State transitions are complex.
* You want update logic centralized in one reducer.

For simple state:

```jsx
const [count, setCount] = useState(0);
```

is usually easier.

---

# 7. useRef

## Definition

`useRef` lets a component hold a mutable value that **persists between renders without causing a re-render when it changes**.

It is commonly used for:

* DOM references
* Timer IDs
* Previous values
* Instance-like mutable values

---

## Syntax

```jsx
const ref = useRef(initialValue);
```

Access the value:

```jsx
ref.current
```

---

## Real-Life Example

Think of a **locker**.

The locker remains available even if the person leaves and comes back.

Similarly:

```text
Render 1
 ↓
ref.current
 ↓
Render 2
 ↓
same ref object
```

Updating:

```jsx
ref.current = value;
```

does not itself cause the component to render again.

---

## DOM Reference Example

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
<input>
   ↓
inputRef
   ↓
inputRef.current
   ↓
DOM element
```

---

## Store a Value Without Re-rendering

```jsx
function Timer() {
  const timerRef = useRef(null);

  function start() {
    timerRef.current = setInterval(() => {
      console.log("Running");
    }, 1000);
  }

  function stop() {
    clearInterval(timerRef.current);
  }

  return (
    <>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </>
  );
}
```

---

## `useRef` vs `useState`

| `useState`                     | `useRef`                                                 |
| ------------------------------ | -------------------------------------------------------- |
| Stores state used by rendering | Stores mutable values not necessarily used for rendering |
| Updating causes a render       | Updating `.current` does not cause a render              |
| Use setter                     | Modify `.current`                                        |
| UI data                        | DOM refs / timers / mutable values                       |

---

# 8. useMemo

## Definition

`useMemo` caches the result of a calculation between renders until its dependencies change.

```jsx
const value = useMemo(
  () => expensiveCalculation(data),
  [data]
);
```

---

## Real-Life Example

Imagine calculating a complicated report.

Instead of recalculating it every time someone opens the dashboard:

```text
First calculation
       ↓
Save result
       ↓
Same inputs?
       ↓
YES → Reuse result
NO  → Calculate again
```

`useMemo` provides this kind of memoization.

---

## Example

```jsx
import { useMemo } from "react";

function ProductList({ products, search }) {
  const filteredProducts = useMemo(() => {
    return products.filter(product =>
      product.name
        .toLowerCase()
        .includes(search.toLowerCase())
    );
  }, [products, search]);

  return (
    <ul>
      {filteredProducts.map(product => (
        <li key={product.id}>
          {product.name}
        </li>
      ))}
    </ul>
  );
}
```

---

## Important

`useMemo` is a **performance optimization**, not something you should use by default for every value.

Don't do:

```jsx
const name = useMemo(
  () => firstName + " " + lastName,
  [firstName, lastName]
);
```

for a trivial calculation.

Prefer:

```jsx
const name = `${firstName} ${lastName}`;
```

---

## Mental Model

```text
Dependencies
     ↓
Changed?
 ┌───┴───┐
Yes     No
 ↓       ↓
Recalc   Reuse
```

---

# 9. useCallback

## Definition

`useCallback` caches a **function definition** between renders until its dependencies change.

```jsx
const handleClick = useCallback(() => {
  // logic
}, [dependencies]);
```

---

## Real-Life Example

Imagine giving someone the same phone number repeatedly.

Instead of generating a completely new contact every time:

```text
Render 1 → Contact A
Render 2 → Contact A
Render 3 → Contact A
```

you preserve the same function reference until something it depends on changes.

---

## Example

```jsx
import {
  useCallback
} from "react";

const handleDelete = useCallback(
  (id) => {
    deleteUser(id);
  },
  []
);
```

---

## Why Does Function Identity Matter?

In JavaScript:

```javascript
function handleClick() {}
```

A new function object is created when the component renders.

For example:

```jsx
function Parent() {
  const handleClick = () => {
    console.log("clicked");
  };

  return <Child onClick={handleClick} />;
}
```

The function reference can be different on subsequent renders.

This matters when passing callbacks to memoized children.

---

## With `memo`

```jsx
const Child = memo(function Child({
  onClick
}) {
  return (
    <button onClick={onClick}>
      Click
    </button>
  );
});
```

Parent:

```jsx
function Parent() {
  const handleClick = useCallback(() => {
    console.log("clicked");
  }, []);

  return <Child onClick={handleClick} />;
}
```

Now the callback reference remains stable unless its dependencies change.

---

## `useMemo` vs `useCallback`

```text
useMemo
   ↓
Caches a VALUE

useCallback
   ↓
Caches a FUNCTION
```

Example:

```jsx
const result = useMemo(
  () => calculate(data),
  [data]
);
```

```jsx
const handleClick = useCallback(
  () => doSomething(id),
  [id]
);
```

Conceptually:

```text
useCallback(fn, deps)

≈

useMemo(() => fn, deps)
```

The distinction is semantic: `useCallback` is specifically for caching a function definition.

---

# 10. useLayoutEffect

## Definition

`useLayoutEffect` is similar to `useEffect`, but React runs it **before the browser repaints the screen after the DOM has been committed**.

It is useful when you need to:

* Measure DOM layout
* Read element dimensions
* Position elements
* Make visual DOM adjustments before the user sees the result

React recommends preferring `useEffect` unless you specifically need to perform work before paint.

---

## Real-Life Example

Imagine hanging a picture on a wall.

You first measure:

```text
Wall
 ↓
Measure position
 ↓
Place picture
 ↓
Show final result
```

You don't want the user to see:

```text
Wrong position
   ↓
Move
   ↓
Correct position
```

Similarly, `useLayoutEffect` can perform measurements or synchronous visual adjustments before the browser paints.

---

## Example

```jsx
import {
  useLayoutEffect,
  useRef,
  useState
} from "react";

function Box() {
  const boxRef = useRef(null);
  const [width, setWidth] = useState(0);

  useLayoutEffect(() => {
    const width =
      boxRef.current.getBoundingClientRect().width;

    setWidth(width);
  }, []);

  return (
    <div>
      <div ref={boxRef}>
        Hello
      </div>

      <p>Width: {width}</p>
    </div>
  );
}
```

---

## `useEffect` vs `useLayoutEffect`

| `useEffect`                          | `useLayoutEffect`                     |
| ------------------------------------ | ------------------------------------- |
| Usually runs after browser paint     | Runs before browser repaint           |
| Preferred for most Effects           | Use for layout/visual synchronization |
| Doesn't block paint in typical cases | Can block paint                       |
| Network, subscriptions, timers       | DOM measurement/visual positioning    |

---

# 11. useImperativeHandle

## Definition

`useImperativeHandle` lets a component **customize the value exposed through a ref**.

It is an advanced Hook and is rarely needed in ordinary application code.

---

## Real-Life Example

Imagine a remote control.

The user doesn't directly control the internal electronics.

Instead, the remote exposes a few operations:

```text
Remote
├── Power
├── Volume Up
└── Volume Down
```

Similarly, a component can expose a small imperative API through a ref:

```text
Parent
  ↓
Child ref
  ↓
focus()
clear()
```

---

## Example

```jsx
import {
  forwardRef,
  useImperativeHandle,
  useRef
} from "react";

const CustomInput = forwardRef(
  function CustomInput(props, ref) {
    const inputRef = useRef(null);

    useImperativeHandle(ref, () => ({
      focus() {
        inputRef.current.focus();
      },

      clear() {
        inputRef.current.value = "";
      }
    }));

    return (
      <input ref={inputRef} />
    );
  }
);
```

Parent:

```jsx
function App() {
  const inputRef = useRef(null);

  return (
    <>
      <CustomInput ref={inputRef} />

      <button onClick={() => inputRef.current.focus()}>
        Focus
      </button>

      <button onClick={() => inputRef.current.clear()}>
        Clear
      </button>
    </>
  );
}
```

---

## Important React Version Note

In modern React, `ref` can be received as a prop by function components, so `forwardRef` is no longer required for this purpose in React 19. `forwardRef` remains relevant for existing code and older React versions.

---

## When to Use

Use `useImperativeHandle` sparingly.

Good use cases:

* Custom input APIs
* Focus management
* Exposing imperative animations
* Integrating with third-party DOM APIs

Avoid using it for normal data flow.

Prefer:

```text
Props
State
Callbacks
```

for normal React communication.

---

# 12. useId

## Definition

`useId` generates a unique ID that can be used to associate related elements, especially for **accessibility**.

---

## Real-Life Example

Imagine every student in a school having a unique roll number.

```text
Student → Roll Number
```

Similarly:

```text
Label → Unique ID
Input → Same ID
```

---

## Example

```jsx
import { useId } from "react";

function EmailField() {
  const id = useId();

  return (
    <>
      <label htmlFor={id}>
        Email
      </label>

      <input
        id={id}
        type="email"
      />
    </>
  );
}
```

Relationship:

```text
<label htmlFor="...">
       ↓
<input id="..." />
```

---

## Multiple IDs

You can derive related IDs:

```jsx
function PasswordField() {
  const id = useId();

  const inputId = `${id}-input`;
  const hintId = `${id}-hint`;

  return (
    <>
      <label htmlFor={inputId}>
        Password
      </label>

      <input
        id={inputId}
        aria-describedby={hintId}
      />

      <p id={hintId}>
        Password must contain 8 characters.
      </p>
    </>
  );
}
```

---

## Don't Use `useId` for List Keys

❌ Don't do:

```jsx
items.map(item => (
  <li key={useId()}>
    {item.name}
  </li>
))
```

Hooks cannot be called inside loops, and `useId` is not intended to generate list keys.

Use:

```jsx
<li key={item.id}>
```

instead.

---

# 13. useTransition

## Definition

`useTransition` lets you mark a state update as a **non-blocking transition**.

Syntax:

```jsx
const [
  isPending,
  startTransition
] = useTransition();
```

It is useful when an update is expensive and you want urgent interactions, such as typing, to remain responsive.

React describes transitions as non-blocking updates that can be interrupted by more urgent updates.

---

## Real-Life Example

Imagine an airport.

There are:

```text
Priority passengers
        ↓
Processed immediately
```

and:

```text
Non-urgent baggage
        ↓
Processed after priority work
```

Similarly:

```text
Urgent update
   ↓
Typing / clicking

Transition
   ↓
Expensive UI update
```

---

## Example

```jsx
import {
  useState,
  useTransition
} from "react";

function Search() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);

  const [
    isPending,
    startTransition
  ] = useTransition();

  function handleChange(e) {
    const value = e.target.value;

    setQuery(value);

    startTransition(() => {
      const filtered = expensiveSearch(value);

      setResults(filtered);
    });
  }

  return (
    <>
      <input
        value={query}
        onChange={handleChange}
      />

      {isPending && (
        <p>Updating results...</p>
      )}

      <Results data={results} />
    </>
  );
}
```

---

## Important

`startTransition` does **not make the expensive calculation itself faster**.

It tells React:

> "This update is less urgent than the other updates."

---

# 14. useDeferredValue

## Definition

`useDeferredValue` lets you use a deferred version of a value so that a non-critical part of the UI can lag behind the latest value while more urgent UI remains responsive.

---

## Real-Life Example

Imagine typing into a search box.

```text
Typing
 ↓
Must be immediate
```

But:

```text
Displaying 10,000 filtered results
 ↓
Can happen slightly later
```

You can defer the value used by the expensive UI.

---

## Example

```jsx
import {
  useDeferredValue,
  useState
} from "react";

function Search({ items }) {
  const [query, setQuery] = useState("");

  const deferredQuery =
    useDeferredValue(query);

  const results = expensiveFilter(
    items,
    deferredQuery
  );

  return (
    <>
      <input
        value={query}
        onChange={e =>
          setQuery(e.target.value)
        }
      />

      <Results data={results} />
    </>
  );
}
```

---

## `useTransition` vs `useDeferredValue`

### `useTransition`

You control the **state update**:

```jsx
startTransition(() => {
  setResults(newResults);
});
```

### `useDeferredValue`

You control the **value used by downstream rendering**:

```jsx
const deferredQuery =
  useDeferredValue(query);
```

Mental model:

```text
useTransition
→ "This update is non-urgent."

useDeferredValue
→ "This value can lag behind."
```

---

# 15. useSyncExternalStore

## Definition

`useSyncExternalStore` lets a component subscribe to a **store or data source outside React** in a way that integrates correctly with React rendering.

It is primarily useful for **library authors and external state-store integrations**.

---

## Real-Life Example

Imagine a **central stock market display**.

The stock price is maintained outside your React component:

```text
External Store
      ↓
Stock Price
      ↓
Multiple React Components
```

React components need to subscribe to the store and update when its data changes.

---

## Basic Structure

```jsx
import {
  useSyncExternalStore
} from "react";

function Component() {
  const value = useSyncExternalStore(
    subscribe,
    getSnapshot
  );

  return <p>{value}</p>;
}
```

---

## Example Store

```jsx
let count = 0;

const listeners = new Set();

function subscribe(listener) {
  listeners.add(listener);

  return () => {
    listeners.delete(listener);
  };
}

function getSnapshot() {
  return count;
}

function increment() {
  count++;

  listeners.forEach(listener => {
    listener();
  });
}
```

Component:

```jsx
function Counter() {
  const count =
    useSyncExternalStore(
      subscribe,
      getSnapshot
    );

  return (
    <>
      <h1>{count}</h1>

      <button onClick={increment}>
        Increment
      </button>
    </>
  );
}
```

---

## Important

For normal component-local state, use:

```jsx
useState()
```

or:

```jsx
useReducer()
```

`useSyncExternalStore` is mainly for integrating React with state that lives outside React.

---

# 16. useInsertionEffect

## Definition

`useInsertionEffect` is a specialized Effect Hook intended primarily for **CSS-in-JS library authors** to insert styles before React makes DOM changes.

React's documentation describes it as a rarely used Hook for dynamic CSS insertion.

---

## Simplified Timing

```text
React rendering
      ↓
useInsertionEffect
      ↓
DOM mutations
      ↓
useLayoutEffect
      ↓
Browser paint
      ↓
useEffect
```

This is a simplified mental model; exact scheduling can vary depending on the update.

---

## Example

```jsx
import { useInsertionEffect } from "react";

function StyledComponent() {
  useInsertionEffect(() => {
    const style =
      document.createElement("style");

    style.textContent = `
      .box {
        color: red;
      }
    `;

    document.head.appendChild(style);

    return () => {
      document.head.removeChild(style);
    };
  }, []);

  return (
    <div className="box">
      Hello
    </div>
  );
}
```

---

## Should Normal Application Developers Use It?

Usually **no**.

For most applications:

```text
useEffect
useLayoutEffect
```

are much more relevant.

`useInsertionEffect` is mainly designed for CSS-in-JS libraries and other specialized infrastructure.

---

# 17. Custom Hooks

## Definition

A **Custom Hook** is a JavaScript function whose name starts with `use` and that can call other Hooks to reuse stateful React logic.

Examples:

```text
useAuth()
useFetch()
useLocalStorage()
useDebounce()
useWindowWidth()
```

---

## Real-Life Example

Think of a **reusable machine**.

Instead of rebuilding the same machine every time:

```text
Machine Design
      ↓
Reusable
      ↓
Factory 1
Factory 2
Factory 3
```

A custom Hook extracts reusable React logic:

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
import {
  useState
} from "react";

function useCounter(initialValue = 0) {
  const [count, setCount] =
    useState(initialValue);

  function increment() {
    setCount(prev => prev + 1);
  }

  function decrement() {
    setCount(prev => prev - 1);
  }

  return {
    count,
    increment,
    decrement
  };
}
```

Use it:

```jsx
function Counter() {
  const {
    count,
    increment,
    decrement
  } = useCounter(0);

  return (
    <>
      <h1>{count}</h1>

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

---

## Custom Hook with `useEffect`

Example: Window width.

```jsx
import {
  useEffect,
  useState
} from "react";

function useWindowWidth() {
  const [width, setWidth] =
    useState(window.innerWidth);

  useEffect(() => {
    function handleResize() {
      setWidth(window.innerWidth);
    }

    window.addEventListener(
      "resize",
      handleResize
    );

    return () => {
      window.removeEventListener(
        "resize",
        handleResize
      );
    };
  }, []);

  return width;
}
```

Use:

```jsx
function App() {
  const width = useWindowWidth();

  return (
    <h1>
      Window Width: {width}
    </h1>
  );
}
```

---

## Custom Hook Rules

A custom Hook:

* Starts with `use`.
* Can call other Hooks.
* Should contain reusable React logic.
* Must follow the Rules of Hooks.
* Does **not** share state automatically between components.

---

## Important: Custom Hooks Don't Share State

Consider:

```jsx
const counterA = useCounter();
const counterB = useCounter();
```

These are two separate Hook instances.

```text
Component A
 └── useCounter()
      └── State A

Component B
 └── useCounter()
      └── State B
```

Custom Hooks share **logic**, not automatically the same state.

---

# 18. Hooks Comparison

| Hook                   | Main Purpose                                           |
| ---------------------- | ------------------------------------------------------ |
| `useState`             | Manage local state                                     |
| `useEffect`            | Synchronize with external systems                      |
| `useContext`           | Read context                                           |
| `useReducer`           | Manage complex state transitions                       |
| `useRef`               | Store mutable values / DOM references                  |
| `useMemo`              | Cache calculation results                              |
| `useCallback`          | Cache function definitions                             |
| `useLayoutEffect`      | Synchronize layout before browser repaint              |
| `useImperativeHandle`  | Customize ref API exposed by a component               |
| `useId`                | Generate unique IDs, especially for accessibility      |
| `useTransition`        | Mark updates as non-blocking                           |
| `useDeferredValue`     | Defer a non-critical value                             |
| `useSyncExternalStore` | Subscribe to external stores                           |
| `useInsertionEffect`   | Insert styles before DOM mutations; mainly library use |
| Custom Hooks           | Reuse stateful React logic                             |

---

# 19. Common Hook Mistakes

## Mistake 1 — Calling Hooks Inside Conditions

❌

```jsx
if (isLoggedIn) {
  useEffect(() => {});
}
```

✅

```jsx
useEffect(() => {
  if (isLoggedIn) {
    // logic
  }
}, [isLoggedIn]);
```

---

## Mistake 2 — Calling Hooks Inside Loops

❌

```jsx
for (const item of items) {
  useState(item);
}
```

Hooks must remain at the top level.

---

## Mistake 3 — Calling a Hook from an Event Handler

❌

```jsx
function handleClick() {
  useState(0);
}
```

---

## Mistake 4 — Using `useEffect` for Derived Data

❌

```jsx
useEffect(() => {
  setFullName(
    `${firstName} ${lastName}`
  );
}, [firstName, lastName]);
```

Better:

```jsx
const fullName =
  `${firstName} ${lastName}`;
```

---

## Mistake 5 — Forgetting Effect Cleanup

❌

```jsx
useEffect(() => {
  window.addEventListener(
    "resize",
    handleResize
  );
}, []);
```

Better:

```jsx
useEffect(() => {
  window.addEventListener(
    "resize",
    handleResize
  );

  return () => {
    window.removeEventListener(
      "resize",
      handleResize
    );
  };
}, []);
```

---

## Mistake 6 — Using `useMemo` Everywhere

❌

```jsx
const name = useMemo(
  () => firstName + lastName,
  [firstName, lastName]
);
```

For simple calculations, this adds unnecessary complexity.

Use:

```jsx
const name = firstName + lastName;
```

---

## Mistake 7 — Using `useCallback` Everywhere

`useCallback` is useful when function identity matters, such as when passing a callback to an optimized child.

Don't wrap every function automatically.

---

## Mistake 8 — Mutating State

❌

```jsx
user.name = "Rahul";
```

✅

```jsx
setUser(prev => ({
  ...prev,
  name: "Rahul"
}));
```

---

# 20. Quick Revision

## State Hooks

```text
useState
   ↓
Simple state

useReducer
   ↓
Complex state logic
```

---

## Context

```text
createContext
      ↓
   Provider
      ↓
useContext
      ↓
Read shared value
```

---

## Ref Hooks

```text
useRef
   ↓
DOM reference
Mutable value
Timer ID

useImperativeHandle
   ↓
Customize exposed ref API
```

---

## Effect Hooks

```text
useEffect
   ↓
External synchronization

useLayoutEffect
   ↓
Layout / visual synchronization before paint

useInsertionEffect
   ↓
CSS insertion / library-level work
```

---

## Performance Hooks

```text
useMemo
   ↓
Cache VALUE

useCallback
   ↓
Cache FUNCTION

useTransition
   ↓
Mark UPDATE as non-urgent

useDeferredValue
   ↓
Defer VALUE
```

---

## Other Hooks

```text
useId
   ↓
Unique IDs

useSyncExternalStore
   ↓
External store subscription

Custom Hook
   ↓
Reusable React logic
```

---

# 21. Interview Revision

### Q1. What are Hooks?

Hooks are special React functions that allow function components to use features such as state, context, refs, effects, and performance-related APIs.

---

### Q2. What are the Rules of Hooks?

Two core rules:

```text
1. Only call Hooks at the top level.
2. Only call Hooks from React functions.
```

That means don't call Hooks inside conditions, loops, event handlers, nested functions, or regular JavaScript functions.

---

### Q3. What is `useState`?

`useState` allows a function component to declare and update local state.

```jsx
const [count, setCount] = useState(0);
```

---

### Q4. What is `useEffect`?

`useEffect` allows a component to synchronize with an external system such as a timer, event subscription, network connection, browser API, or third-party library.

---

### Q5. Does `useEffect` run only once?

No.

Its behavior depends on its dependency array.

```jsx
useEffect(() => {});
```

Runs after every commit.

```jsx
useEffect(() => {}, []);
```

Does not re-run because of later dependency changes.

```jsx
useEffect(() => {}, [value]);
```

Re-runs when `value` changes.

---

### Q6. What is cleanup in `useEffect`?

Cleanup is the function returned from an Effect:

```jsx
useEffect(() => {
  // setup

  return () => {
    // cleanup
  };
}, []);
```

It is used to undo or stop whatever the Effect set up, such as subscriptions, timers, or connections. React runs cleanup before re-running an Effect with changed dependencies and when the component is removed.

---

### Q7. What is `useContext`?

`useContext` reads and subscribes to a context value without requiring the value to be passed manually through every intermediate component.

---

### Q8. `useState` vs `useReducer`?

```text
useState
→ Simple state

useReducer
→ Complex state transitions
```

Use `useReducer` when many related actions modify the same state.

---

### Q9. `useState` vs `useRef`?

```text
useState
→ UI-related state
→ Update causes render

useRef
→ Mutable value / DOM reference
→ Changing .current does not cause render
```

---

### Q10. What is `useMemo`?

`useMemo` caches the result of a calculation until its dependencies change.

```jsx
const result = useMemo(
  () => calculate(data),
  [data]
);
```

---

### Q11. What is `useCallback`?

`useCallback` caches a function definition until its dependencies change.

```jsx
const handleClick = useCallback(
  () => doSomething(id),
  [id]
);
```

---

### Q12. Difference between `useMemo` and `useCallback`?

```text
useMemo
→ caches a calculated VALUE

useCallback
→ caches a FUNCTION
```

---

### Q13. What is `useLayoutEffect`?

`useLayoutEffect` runs before the browser repaints after DOM updates, making it useful for layout measurement and visual synchronization.

---

### Q14. What is `useImperativeHandle`?

It customizes the value exposed through a component's ref.

It is useful for exposing imperative methods such as:

```text
focus()
clear()
open()
close()
```

---

### Q15. What is `useId` used for?

`useId` generates unique IDs, commonly for connecting accessible form elements such as labels, inputs, and descriptions.

---

### Q16. What is `useTransition`?

`useTransition` lets you mark a state update as non-urgent so React can keep more urgent interactions responsive.

---

### Q17. What is `useDeferredValue`?

It provides a deferred version of a value so a non-critical part of the UI can update later.

---

### Q18. Difference between `useTransition` and `useDeferredValue`?

```text
useTransition
→ Defer an UPDATE

useDeferredValue
→ Defer a VALUE
```

---

### Q19. What is `useSyncExternalStore`?

It allows React components to subscribe to state or data maintained outside React.

---

### Q20. What is `useInsertionEffect`?

It is a specialized Hook primarily intended for CSS-in-JS libraries to insert styles before React performs DOM mutations.

---

### Q21. What is a Custom Hook?

A Custom Hook is a function beginning with `use` that combines existing Hooks to reuse stateful React logic.

Example:

```jsx
function useWindowWidth() {
  const [width, setWidth] =
    useState(window.innerWidth);

  // ...
}
```

---

# 🧠 Final Mental Model

```text
                           HOOKS
                             │
        ┌────────────────────┼────────────────────┐
        ↓                    ↓                    ↓
      STATE                EFFECTS              REFS
        │                    │                    │
   useState              useEffect             useRef
   useReducer             useLayoutEffect      useImperativeHandle
                          useInsertionEffect
        │
        └─────────────────────────────────────────
                             │
                             ↓
                         CONTEXT
                             │
                       useContext
                             │
        ┌────────────────────┴────────────────────┐
        ↓                                         ↓
   PERFORMANCE                              OTHER / ADVANCED
        │                                         │
   useMemo                                  useId
   useCallback                              useSyncExternalStore
   useTransition
   useDeferredValue
        │
        └────────────────────┬────────────────────┘
                             ↓
                       CUSTOM HOOKS
                             │
                             ↓
                  Reusable React Logic
```

---

# 🔥 One-Line Hook Cheat Sheet

```text
useState()
→ Store and update local component state.

useEffect()
→ Synchronize the component with an external system.

useContext()
→ Read a context value.

useReducer()
→ Manage state using reducer + actions.

useRef()
→ Hold a mutable value or DOM reference without causing a render when .current changes.

useMemo()
→ Cache the result of a calculation.

useCallback()
→ Cache a function definition.

useLayoutEffect()
→ Run an Effect for layout/visual synchronization before browser repaint.

useImperativeHandle()
→ Customize what a component exposes through a ref.

useId()
→ Generate unique IDs, especially for accessibility.

useTransition()
→ Mark a state update as non-urgent.

useDeferredValue()
→ Defer a non-critical value.

useSyncExternalStore()
→ Subscribe to an external store.

useInsertionEffect()
→ Insert styles before DOM mutations; mainly for library authors.

Custom Hook
→ Reuse stateful React logic.
```

---

# 🚀 Hook Selection Guide

When you face a problem, think like this:

```text
Need to store changing UI data?
        ↓
    useState


State transitions are complex?
        ↓
    useReducer


Need shared context data?
        ↓
    useContext


Need DOM element / mutable value?
        ↓
    useRef


Need synchronization with external system?
        ↓
    useEffect


Need DOM measurement before paint?
        ↓
    useLayoutEffect


Need to customize exposed ref methods?
        ↓
    useImperativeHandle


Need to cache expensive calculation?
        ↓
    useMemo


Need stable callback reference?
        ↓
    useCallback


Need unique accessible ID?
        ↓
    useId


Need non-urgent state update?
        ↓
    useTransition


Need deferred value?
        ↓
    useDeferredValue


Need React integration with external store?
        ↓
    useSyncExternalStore


Building CSS-in-JS infrastructure?
        ↓
    useInsertionEffect


Need reusable stateful logic?
        ↓
    Custom Hook
```

---

# ⭐ Most Important Hooks for Interviews

Prioritize these first:

```text
⭐⭐⭐⭐⭐ useState
⭐⭐⭐⭐⭐ useEffect
⭐⭐⭐⭐⭐ useContext
⭐⭐⭐⭐⭐ useRef
⭐⭐⭐⭐⭐ useMemo
⭐⭐⭐⭐⭐ useCallback
⭐⭐⭐⭐⭐ Custom Hooks

⭐⭐⭐⭐  useReducer
⭐⭐⭐⭐  useLayoutEffect

⭐⭐⭐    useTransition
⭐⭐⭐    useDeferredValue

⭐⭐     useId
⭐⭐     useImperativeHandle

⭐      useSyncExternalStore
⭐      useInsertionEffect
```

> **Core idea:** Hooks let function components access React's capabilities. The most important mental model is not memorizing every Hook, but understanding **what problem each Hook solves and when you should not use it**.
