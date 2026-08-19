# React A→Z Revision

**Phase 14 — Interview Concepts ⭐**

> Core React internals and JavaScript concepts that are frequently tested in React interviews. These concepts explain **how React updates the UI, why components re-render, how state updates are processed, and how React applications stay performant.**

---

# Contents

1. [Interview Concepts Overview](#1-interview-concepts-overview)
2. [Virtual DOM](#2-virtual-dom)
3. [Reconciliation](#3-reconciliation)
4. [Fiber](#4-fiber)
5. [Re-rendering](#5-re-rendering)
6. [State Batching](#6-state-batching)
7. [Closures](#7-closures)
8. [Stale Closures](#8-stale-closures)
9. [Referential Equality](#9-referential-equality)
10. [Immutability](#10-immutability)
11. [Performance Optimization](#11-performance-optimization)
12. [How Everything Connects](#12-how-everything-connects)
13. [Quick Revision](#13-quick-revision)
14. [Interview Questions](#14-interview-questions)
15. [Final Mental Map](#15-final-mental-map)

---

# 1. Interview Concepts Overview

These concepts answer some of the most common React interview questions:

```text
How does React update the DOM?
        ↓
Virtual DOM
        ↓
Reconciliation
        ↓
Fiber
        ↓
Commit
```

And:

```text
Why does a component render again?
        ↓
State / Props / Context changes
        ↓
Re-render
```

And:

```text
Why doesn't state update immediately?
        ↓
State Snapshot
        ↓
Batching
```

And:

```text
Why does my callback use old state?
        ↓
Closure
        ↓
Stale Closure
```

And:

```text
Why did React consider these objects/functions different?
        ↓
Referential Equality
```

And:

```text
Why should I create a new object instead of modifying the old one?
        ↓
Immutability
```

---

# 2. Virtual DOM

## Definition

The **Virtual DOM** is a JavaScript representation of the UI that React uses to determine what changes need to be applied to the actual DOM.

---

## Real-Life Example

Imagine a building blueprint.

You don't rebuild the entire building every time you want to change one window.

Instead:

```text
Blueprint
    ↓
Compare old design with new design
    ↓
Find changes
    ↓
Modify only required parts
```

React follows a similar conceptual process.

---

## Traditional DOM Update

Suppose the page contains:

```html
<div>
  <h1>Counter</h1>
  <p>Count: 0</p>
  <button>+</button>
</div>
```

When the count changes:

```text
Count: 0
   ↓
Count: 1
```

We don't want to unnecessarily recreate the entire DOM tree.

---

## React's Conceptual Flow

```text
State Change
     ↓
Component renders
     ↓
New React element tree
     ↓
Compare with previous tree
     ↓
Determine changes
     ↓
Update actual DOM
```

---

## Important Interview Point

The Virtual DOM is **not simply "a faster DOM."**

A better explanation is:

> React uses an in-memory representation of the UI to determine the minimal set of DOM mutations needed to synchronize the UI with the latest state.

---

## Virtual DOM vs Real DOM

| Virtual DOM                 | Real DOM                        |
| --------------------------- | ------------------------------- |
| JavaScript representation   | Browser DOM                     |
| Lightweight representation  | Actual document structure       |
| Used during React rendering | Browser renders it              |
| Can be compared efficiently | DOM operations can be expensive |
| Not directly displayed      | Actually displayed              |

---

# 3. Reconciliation

## Definition

**Reconciliation** is the process React uses to compare the previous rendered element tree with the new one and determine what needs to change.

---

## Real-Life Example

Imagine comparing two versions of a document:

```text
Version 1
---------
Name: Aniket
Age: 20
City: Pune
```

Version 2:

```text
Name: Aniket
Age: 21
City: Pune
```

Only:

```text
Age
```

changed.

There is no need to rewrite the entire document.

React performs a similar comparison process.

---

## Example

Initial:

```jsx
<div>
  <h1>Hello</h1>
  <p>Count: 0</p>
</div>
```

After state update:

```jsx
<div>
  <h1>Hello</h1>
  <p>Count: 1</p>
</div>
```

React identifies:

```text
<h1> → unchanged
<p> → text changed
```

So the DOM update is focused on the changed part.

---

# Keys and Reconciliation

Keys are extremely important when rendering lists.

```jsx
users.map(user => (
  <User
    key={user.id}
    user={user}
  />
))
```

React uses keys to determine the identity of list items.

---

## Bad Key

```jsx
users.map((user, index) => (
  <User
    key={index}
    user={user}
  />
))
```

Using array indexes can cause problems when items are:

```text
Inserted
Removed
Reordered
```

---

## Good Key

```jsx
users.map(user => (
  <User
    key={user.id}
    user={user}
  />
))
```

The key should represent the stable identity of the item.

---

## Reconciliation Mental Model

```text
Old Tree
   ↓
New Tree
   ↓
Compare
   ↓
Identify changes
   ↓
Commit required DOM updates
```

---

# 4. Fiber

## Definition

**Fiber** is React's internal architecture for representing and scheduling rendering work.

It enables React to:

```text
Pause work
Resume work
Prioritize work
Abandon outdated work
Schedule updates
```

---

## Real-Life Example

Imagine a student solving several assignments:

```text
Assignment A → Very difficult
Assignment B → Urgent
Assignment C → Normal
```

Instead of finishing A completely before touching B:

```text
A → A → A → A → B
```

the student can prioritize:

```text
A
↓
B arrives
↓
Pause A
↓
Complete important B
↓
Continue A
```

Fiber gives React the architecture needed for this kind of scheduling.

---

## Fiber Tree

React internally represents component work using Fiber nodes.

Conceptually:

```text
App Fiber
   │
   ├── Navbar Fiber
   │
   ├── Dashboard Fiber
   │      ├── Stats Fiber
   │      └── TaskList Fiber
   │
   └── Footer Fiber
```

Each Fiber contains information about a unit of work.

---

## Why Fiber Was Introduced

Older React rendering was more difficult to interrupt.

Fiber enabled React to break rendering work into smaller units.

```text
Large Render
     ↓
Small units of work
     ↓
Process
Pause if necessary
Resume later
```

---

## Fiber and Concurrent Rendering

A useful interview relationship:

```text
Fiber
  ↓
Internal architecture
  ↓
Enables scheduling
  ↓
Concurrent rendering capabilities
```

---

## Important Interview Answer

Do not say:

> "Fiber is the Virtual DOM."

Better:

> **Fiber is React's internal data structure and reconciliation architecture that represents units of rendering work and enables scheduling, prioritization, interruption, and resumption of that work.**

---

# 5. Re-rendering

## Definition

A **re-render** occurs when React calls a component again to calculate what its UI should look like based on the latest props, state, or context.

---

## What Causes a Re-render?

Common causes:

```text
State update
Props change
Context value change
Parent re-render
External store update
```

---

# State Update

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button
      onClick={() => setCount(count + 1)}
    >
      {count}
    </button>
  );
}
```

When:

```js
setCount(1);
```

is called:

```text
State changes
    ↓
Component scheduled to render
    ↓
Component function runs again
    ↓
React compares output
    ↓
DOM updated if necessary
```

---

# Parent Re-render

Consider:

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button
        onClick={() => setCount(count + 1)}
      >
        {count}
      </button>

      <Child />
    </>
  );
}
```

When `Parent` re-renders, `Child` can also render.

This is one reason techniques such as:

```text
React.memo
useMemo
useCallback
```

can sometimes be useful.

---

## Important

A **re-render does not necessarily mean a DOM update**.

This distinction is very important.

```text
Component re-render
        ↓
React calculates new UI
        ↓
Reconciliation
        ↓
Maybe DOM changes
```

The component function can execute even when React ultimately makes no DOM mutation.

---

# 6. State Batching

## Definition

**State batching** means React can group multiple state updates together and process them as a batch rather than causing a separate render for every update.

---

## Real-Life Example

Suppose you need to send 5 documents.

Instead of making:

```text
Delivery 1
Delivery 2
Delivery 3
Delivery 4
Delivery 5
```

you package them together:

```text
One delivery
    ↓
5 documents
```

React similarly groups compatible state updates.

---

## Example

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
  }

  return <p>{count}</p>;
}
```

You might expect:

```text
0 → 1 → 2 → 3
```

But that is not what happens here.

All three updates use the same render's `count` value:

```text
count = 0

setCount(1)
setCount(1)
setCount(1)
```

The final result is:

```text
1
```

---

# Functional Updates

If you need multiple updates based on the previous state:

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Now React processes them sequentially:

```text
0
 ↓
1
 ↓
2
 ↓
3
```

Final result:

```text
3
```

---

## React 18+ Automatic Batching

Modern React batches state updates in many asynchronous contexts as well.

Example:

```jsx
setTimeout(() => {
  setCount(c => c + 1);
  setName("Aniket");
}, 1000);
```

React can batch these updates into a single render.

---

## Interview Question

**Why does React batch state updates?**

Answer:

> To reduce unnecessary rendering work and improve application performance by processing multiple state updates together.

---

# 7. Closures

## Definition

A **closure** is created when a function remembers and can access variables from its surrounding lexical scope even after that outer function has finished executing.

---

## Simple JavaScript Example

```js
function createCounter() {
  let count = 0;

  return function increment() {
    count++;

    console.log(count);
  };
}

const counter = createCounter();

counter();
counter();
counter();
```

Output:

```text
1
2
3
```

Why?

Because `increment()` remembers:

```text
count
```

from its outer scope.

---

## Real-Life Example

Imagine you receive a locker key.

```text
Student
  ↓
Locker
  ↓
Key
```

Even after leaving the classroom, the student still has the key and can access the locker.

The function similarly retains access to its lexical environment.

---

# Closures in React

Consider:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    console.log(count);
  }

  return (
    <button onClick={handleClick}>
      Count: {count}
    </button>
  );
}
```

`handleClick` closes over the `count` value from the render in which that function was created.

---

## Important React Mental Model

Each render creates a new snapshot:

```text
Render 1
count = 0
handleClick → remembers 0

Render 2
count = 1
handleClick → remembers 1

Render 3
count = 2
handleClick → remembers 2
```

This leads directly to the concept of stale closures.

---

# 8. Stale Closures

## Definition

A **stale closure** occurs when a function captures an older value from a previous render and later executes using that outdated value.

---

## Real-Life Example

Imagine someone gives you an old printed timetable.

```text
Old timetable:
Meeting → 10:00 AM
```

Later the meeting changes:

```text
New timetable:
Meeting → 12:00 PM
```

But you still have the old paper.

You are working with stale information.

---

# Example

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      console.log(count);
    }, 1000);

    return () => clearInterval(timer);
  }, []);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

The effect runs once because:

```js
[]
```

The callback captures:

```text
count = 0
```

So it may continue logging:

```text
0
0
0
0
```

even when the UI displays:

```text
1
2
3
```

---

# Solution 1: Dependency Array

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log(count);
  }, 1000);

  return () => clearInterval(timer);
}, [count]);
```

Now the effect is recreated when `count` changes.

---

# Solution 2: Functional State Update

If the callback needs to update state based on the previous state:

```jsx
setCount(prev => prev + 1);
```

This avoids depending on a potentially stale captured value.

---

# Solution 3: Ref for Latest Mutable Value

Sometimes you need a stable callback that can read the latest value.

```jsx
const countRef = useRef(count);

useEffect(() => {
  countRef.current = count;
}, [count]);
```

Then:

```jsx
console.log(countRef.current);
```

can access the latest stored value.

---

## Stale Closure Mental Model

```text
Render 1
count = 0
   ↓
Callback created
   ↓
Callback remembers 0

Render 2
count = 1

Render 3
count = 2

Old callback still remembers 0
```

---

# 9. Referential Equality

## Definition

**Referential equality** means comparing whether two variables refer to the exact same object or function in memory.

For primitives:

```js
1 === 1
```

returns:

```text
true
```

For objects:

```js
{} === {}
```

returns:

```text
false
```

because these are two different object references.

---

## Real-Life Example

Two identical houses are not the same physical house.

```text
House A → Address 101
House B → Address 102
```

Even if they look identical:

```text
House A !== House B
```

Objects work similarly.

---

# Objects

```js
const user1 = {
  name: "Aniket"
};

const user2 = {
  name: "Aniket"
};

console.log(user1 === user2);
```

Result:

```text
false
```

---

# Same Reference

```js
const user1 = {
  name: "Aniket"
};

const user2 = user1;

console.log(user1 === user2);
```

Result:

```text
true
```

Both variables point to the same object.

---

# Why React Cares

React uses reference comparisons in several optimization and state-related scenarios.

For example:

```jsx
const user = {
  name: "Aniket"
};
```

If you create a new object:

```jsx
<User user={{ name: "Aniket" }} />
```

the object reference changes on every render.

This matters when using:

```text
React.memo
useMemo
useCallback
Context
Dependency arrays
```

---

# Function References

Functions are objects too.

```js
const fn1 = () => {};
const fn2 = () => {};

console.log(fn1 === fn2);
```

Result:

```text
false
```

Even though their code looks identical.

This is one reason `useCallback()` can matter when passing functions to memoized children.

---

# 10. Immutability

## Definition

**Immutability** means treating existing state/data as unchanged and creating a new value when you need to represent a change.

---

## Real-Life Example

Imagine a printed document.

Instead of modifying the original document:

```text
Original document
      ↓
Write over it
```

you create:

```text
Original document
      ↓
Create updated copy
```

This makes changes easier to track.

---

# Bad State Mutation

```jsx
const [user, setUser] = useState({
  name: "Aniket",
  age: 20
});

user.age = 21;

setUser(user);
```

The same object reference is passed back:

```text
oldUser === newUser
```

This can prevent React from recognizing the state change as expected.

---

# Correct Approach

```jsx
setUser(prev => ({
  ...prev,
  age: 21
}));
```

Now:

```text
oldUser !== newUser
```

because a new object was created.

---

# Array Example

Bad:

```jsx
users.push(newUser);

setUsers(users);
```

The original array was mutated.

---

## Correct

```jsx
setUsers(prev => [
  ...prev,
  newUser
]);
```

A new array is created.

---

# Updating Objects

```jsx
setUser(prev => ({
  ...prev,
  address: {
    ...prev.address,
    city: "Pune"
  }
}));
```

This preserves immutability at each changed level.

---

# Why Immutability Matters

It helps React and your application with:

```text
Predictable state updates
Referential comparisons
Memoization
Debugging
State history
Optimization
```

---

# Immutability Does NOT Mean

It does not mean:

```text
"Never change any data anywhere."
```

It means:

> Do not directly mutate React state or other values whose identity React relies on; create the appropriate new value instead.

---

# 11. Performance Optimization

## Definition

**Performance optimization** means reducing unnecessary work so the React application remains fast and responsive.

---

# Main Sources of React Performance Problems

```text
Unnecessary renders
Expensive calculations
Large component trees
Large lists
Large JavaScript bundles
Too many API requests
Unoptimized images
Excessive event handlers
```

---

# React.memo

Prevents unnecessary child renders when props are unchanged.

```jsx
const User = React.memo(function User({
  user
}) {
  return <p>{user.name}</p>;
});
```

---

# useMemo

Memoizes an expensive calculated value.

```jsx
const filteredUsers = useMemo(
  () => users.filter(user => user.active),
  [users]
);
```

---

# useCallback

Memoizes a function reference.

```jsx
const handleSelect = useCallback(
  id => {
    setSelectedId(id);
  },
  []
);
```

---

# Lazy Loading

Load components only when required.

```jsx
const Admin = lazy(
  () => import("./Admin")
);
```

---

# Code Splitting

Break a large JavaScript bundle into smaller chunks.

```text
main.js
   ↓
admin.js
dashboard.js
settings.js
```

---

# Virtualization

Render only visible items from a large list.

```text
100,000 items
      ↓
Only visible items
```

---

# Debouncing

Useful for:

```text
Search
Autocomplete
Validation
```

```text
Typing
 ↓
Wait
 ↓
Request
```

---

# Throttling

Useful for:

```text
Scroll
Resize
Mouse movement
```

```text
Event Event Event Event
       ↓
Controlled execution
```

---

# Performance Optimization Principle

Do not optimize blindly.

Use:

```text
Measure
  ↓
Identify bottleneck
  ↓
Optimize
  ↓
Measure again
```

---

# 12. How Everything Connects

This is one of the most useful interview mental models.

Suppose:

```jsx
setCount(count + 1);
```

is called.

---

## Step 1 — State Update

```text
setCount()
   ↓
React schedules update
```

---

## Step 2 — Batching

If multiple updates occur:

```text
setCount(...)
setName(...)
setOpen(...)
```

React may batch them.

```text
Multiple updates
      ↓
One rendering cycle
```

---

## Step 3 — Re-render

React calls the affected component again.

```text
Component function
       ↓
New React element tree
```

---

## Step 4 — Reconciliation

React compares:

```text
Previous tree
     vs
New tree
```

---

## Step 5 — Fiber

Fiber provides the internal representation and scheduling system used to process rendering work.

```text
Fiber
 ↓
Schedule / prioritize work
 ↓
Process reconciliation
```

---

## Step 6 — Commit

React applies necessary changes to the actual DOM.

```text
Reconciliation
     ↓
Required DOM mutations
     ↓
Browser DOM
```

---

## Step 7 — Browser Paint

The browser updates what the user sees.

```text
React
 ↓
DOM
 ↓
Browser rendering
 ↓
User sees UI
```

---

# Complete Flow

```text
             State Update
                  ↓
             setState()
                  ↓
              Batching
                  ↓
            Render Phase
                  ↓
               Fiber
                  ↓
          Reconciliation
                  ↓
             Commit Phase
                  ↓
             Actual DOM
                  ↓
             Browser Paint
```

---

# Closures + State

Now consider:

```jsx
setCount(count + 1);
```

The `count` used here belongs to the current render's snapshot.

Therefore:

```text
Render 1
count = 0
      ↓
Event handler remembers 0
      ↓
setCount(count + 1)
      ↓
setCount(1)
```

After rendering:

```text
Render 2
count = 1
```

This explains why React state should be thought of as a **snapshot for a particular render**.

---

# Referential Equality + Immutability

These two concepts are strongly connected.

Bad:

```jsx
user.age = 21;
setUser(user);
```

Reference:

```text
oldUser === newUser
```

Correct:

```jsx
setUser(prev => ({
  ...prev,
  age: 21
}));
```

Reference:

```text
oldUser !== newUser
```

Therefore React can more easily detect that the state value has changed.

---

# 13. Quick Revision

| Concept                  | One-Line Definition                                                          |
| ------------------------ | ---------------------------------------------------------------------------- |
| Virtual DOM              | In-memory representation of UI used during React rendering                   |
| Reconciliation           | Process of determining what changed between renders                          |
| Fiber                    | React's internal architecture for representing and scheduling rendering work |
| Re-rendering             | Re-executing a component to calculate its latest UI                          |
| State Batching           | Grouping multiple state updates to reduce rendering work                     |
| Closure                  | Function retaining access to its lexical environment                         |
| Stale Closure            | Function using an outdated value captured from an earlier render             |
| Referential Equality     | Comparing whether two values have the same object/function reference         |
| Immutability             | Creating new values instead of directly mutating existing state              |
| Performance Optimization | Reducing unnecessary computation, rendering, and resource usage              |

---

# Most Important Interview Distinctions

## Re-render vs DOM Update

```text
Re-render
→ Component executes again

DOM update
→ Actual browser DOM changes
```

A re-render does **not** guarantee a DOM update.

---

## Virtual DOM vs Fiber

```text
Virtual DOM
→ Conceptual in-memory UI representation

Fiber
→ Internal React architecture/data structure
   used to represent and schedule rendering work
```

---

## Reconciliation vs Rendering

```text
Rendering
→ React calculates the next UI

Reconciliation
→ React determines what changed
```

---

## Closure vs Stale Closure

```text
Closure
→ Function remembers surrounding variables

Stale Closure
→ Function remembers an outdated value
```

---

## Referential Equality vs Value Equality

```text
Primitive:
1 === 1
→ true

Object:
{} === {}
→ false
```

Because objects are compared by reference.

---

## Mutation vs Immutability

```text
Mutation:
user.age = 21

Immutability:
setUser(prev => ({
  ...prev,
  age: 21
}))
```

---

# 14. Interview Questions

## Basic

1. What is the Virtual DOM?
2. How does React update the DOM?
3. What is reconciliation?
4. What is Fiber?
5. What causes a React component to re-render?
6. Does every re-render cause a DOM update?
7. What is state batching?
8. What is a closure?
9. What is a stale closure?
10. What is referential equality?
11. What is immutability?
12. Why is immutability important in React?

---

## Intermediate

13. Explain the Virtual DOM with an example.
14. Explain React's reconciliation process.
15. Why are keys important during reconciliation?
16. Why is using array indexes as keys sometimes problematic?
17. What problem does Fiber solve?
18. What is the difference between render and commit phases?
19. Why does this result in `1`?

```jsx
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);
```

20. Why does the functional updater produce `3`?

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

21. Explain stale closures in React.
22. How can you prevent stale closures?
23. Why does `{}` !== `{}`?
24. Why should React state not be mutated directly?
25. How does immutability help performance?

---

## Advanced

26. Explain React's rendering pipeline from state update to DOM update.
27. How does Fiber enable concurrent rendering?
28. What is the difference between reconciliation and committing?
29. Explain how React uses keys during reconciliation.
30. How does referential equality affect `React.memo`?
31. How does referential equality affect `useMemo` and `useCallback`?
32. Why can creating an object inside a component cause unnecessary re-renders?
33. Explain a stale closure caused by `useEffect`.
34. How would you debug unnecessary React re-renders?
35. What strategies would you use to optimize a React application?
36. Why doesn't React simply update the DOM directly whenever state changes?
37. What happens internally after calling a state setter?
38. Explain batching, rendering, reconciliation, and commit as one complete process.
39. How do immutability and referential equality work together?
40. How would you optimize a component rendering 100,000 records?

---

# 15. Final Mental Map

```text
                         REACT INTERNALS
                               │
             ┌─────────────────┼─────────────────┐
             ↓                 ↓                 ↓
        State Update       Component         Performance
             │             Rendering             │
             ↓                 ↓                 ↓
         Batching           Re-render       memo / memoization
             │                 │            virtualization
             ↓                 ↓            lazy loading
           Fiber          New UI Tree       debounce
             │                 │            throttle
             ↓                 ↓
      Reconciliation ← Virtual DOM
             │
             ↓
          Commit
             │
             ↓
         Actual DOM
```

And JavaScript concepts support the whole model:

```text
                 JAVASCRIPT CONCEPTS
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
          Closures   References   Immutability
             │           │           │
             ↓           ↓           ↓
       Stale Closure  Referential   New Objects
                      Equality
             │           │           │
             └───────────┼───────────┘
                         ↓
                  React Behavior
```

---

# ⭐ Ultimate Interview Explanation

If an interviewer asks:

> **"What happens when you call `setState` in React?"**

A strong answer is:

```text
1. Calling the state setter schedules a state update.

2. React may batch the update with other state updates.

3. React renders the affected component using the new state snapshot.

4. React creates the next React element tree.

5. Fiber represents and schedules the rendering work.

6. React performs reconciliation to determine what changed.

7. During the commit phase, React applies the necessary DOM mutations.

8. The browser then renders the updated UI.
```

And remember:

```text
Re-render ≠ DOM update
```

A component can execute again while React determines that no actual DOM mutation is necessary.

> **The key to React interviews is understanding the relationship between state updates, batching, rendering, Fiber, reconciliation, commit, closures, references, immutability, and performance. Once these concepts are connected, many "tricky" React interview questions become straightforward.**
