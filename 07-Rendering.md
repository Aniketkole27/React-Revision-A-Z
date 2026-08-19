# React A→Z Revision

**Phase 7 — Rendering 🔄**

> Rendering is the process through which React determines what the UI should look like based on the current props, state, and context, and then updates the DOM when necessary.

---

## 📚 Contents

1. [What is Rendering?](#1-what-is-rendering)
2. [Conditional Rendering](#2-conditional-rendering)
3. [Lists](#3-lists)
4. [Keys](#4-keys)
5. [Re-rendering](#5-re-rendering)
6. [Mounting](#6-mounting)
7. [Updating](#7-updating)
8. [Unmounting](#8-unmounting)
9. [Component Lifecycle Mental Model](#9-component-lifecycle-mental-model)
10. [Common Rendering Mistakes](#10-common-rendering-mistakes)
11. [Quick Revision](#11-quick-revision)
12. [Interview Revision](#12-interview-revision)

---

# 1. What is Rendering?

## Definition

**Rendering** in React is the process of calling components to determine the UI they should return based on the current state, props, and context.

Example:

```jsx
function App() {
  return <h1>Hello React</h1>;
}
```

React renders:

```text
App()
 ↓
<h1>Hello React</h1>
 ↓
Browser UI
```

---

## Important Concept

A React render does **not** necessarily mean:

> "React changed the DOM."

A render means React calculated what the UI should be.

After rendering, React compares the result with the previous result and commits only the necessary DOM changes.

```text
State / Props / Context change
          ↓
       Render
          ↓
React calculates UI
          ↓
Compare with previous result
          ↓
Commit necessary DOM changes
          ↓
Browser displays UI
```

---

## Real-Life Example

Imagine editing a document.

You change one word:

```text
Before:
I like JavaScript.

After:
I love JavaScript.
```

You don't need to rewrite the entire document.

You identify what changed:

```text
like → love
```

React follows a similar idea when updating the DOM.

---

# 2. Conditional Rendering

## Definition

**Conditional rendering** means displaying different UI depending on a condition.

Example:

```jsx
function App({ isLoggedIn }) {
  if (isLoggedIn) {
    return <h1>Welcome!</h1>;
  }

  return <h1>Please log in.</h1>;
}
```

---

## Real-Life Example

Think about a security gate:

```text
Is user authenticated?
       │
   ┌───┴───┐
  Yes      No
   ↓        ↓
Allow     Reject
```

React can use the same logic:

```text
Condition
   ↓
 ┌─┴─┐
Yes  No
 ↓    ↓
UI   UI
```

---

## Using `if`

```jsx
function Dashboard({ isLoggedIn }) {
  if (!isLoggedIn) {
    return <Login />;
  }

  return <DashboardContent />;
}
```

This is often the cleanest approach when the branches are substantial.

---

## Ternary Operator

Syntax:

```jsx
condition ? trueResult : falseResult
```

Example:

```jsx
function App({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn
        ? <Dashboard />
        : <Login />
      }
    </div>
  );
}
```

---

## Logical AND (`&&`)

Useful when you only want to render something when a condition is true.

```jsx
function Notification({ hasMessage }) {
  return (
    <div>
      {hasMessage && (
        <p>You have a new message.</p>
      )}
    </div>
  );
}
```

If:

```text
hasMessage = true
```

the paragraph renders.

If:

```text
hasMessage = false
```

nothing is rendered for that expression.

---

## Important `&&` Trap

Be careful with numeric values:

```jsx
{count && <p>Items available</p>}
```

If:

```text
count = 0
```

React can render:

```text
0
```

because JavaScript evaluates:

```javascript
0 && <p>...</p>
```

to:

```text
0
```

Safer:

```jsx
{count > 0 && (
  <p>Items available</p>
)}
```

---

## Conditional Rendering with Loading

```jsx
function Users({ loading, users }) {
  if (loading) {
    return <p>Loading...</p>;
  }

  if (users.length === 0) {
    return <p>No users found.</p>;
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

This is a common real-world rendering pattern:

```text
Loading
   ↓
Loaded?
 ┌─┴─────────┐
No           Yes
↓             ↓
Error?       Data?
              ↓
         Empty / Content
```

---

## Multiple Conditions

```jsx
function Status({ status }) {
  if (status === "loading") {
    return <p>Loading...</p>;
  }

  if (status === "error") {
    return <p>Something went wrong.</p>;
  }

  if (status === "success") {
    return <p>Data loaded.</p>;
  }

  return null;
}
```

---

## Returning `null`

A component can return `null` when it should render nothing.

```jsx
function AdminPanel({ isAdmin }) {
  if (!isAdmin) {
    return null;
  }

  return <div>Admin Panel</div>;
}
```

---

# 3. Lists

## Definition

**List rendering** means generating multiple React elements from an array or another collection of data.

The most common approach is `map()`.

---

## Basic Example

```jsx
function Users() {
  const users = [
    { id: 1, name: "Aniket" },
    { id: 2, name: "Rahul" },
    { id: 3, name: "Priya" }
  ];

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

---

## Real-Life Example

Imagine a restaurant menu:

```text
Menu
├── Pizza
├── Burger
├── Pasta
└── Sandwich
```

Instead of manually writing:

```jsx
<li>Pizza</li>
<li>Burger</li>
<li>Pasta</li>
<li>Sandwich</li>
```

you store the data:

```javascript
const menu = [
  "Pizza",
  "Burger",
  "Pasta",
  "Sandwich"
];
```

and generate the UI:

```jsx
menu.map(item => (
  <li>{item}</li>
))
```

---

## List of Components

```jsx
function UserList({ users }) {
  return (
    <div>
      {users.map(user => (
        <UserCard
          key={user.id}
          user={user}
        />
      ))}
    </div>
  );
}
```

---

## Filtering Before Mapping

You can combine `filter()` and `map()`:

```jsx
function ActiveUsers({ users }) {
  return (
    <ul>
      {users
        .filter(user => user.active)
        .map(user => (
          <li key={user.id}>
            {user.name}
          </li>
        ))}
    </ul>
  );
}
```

Flow:

```text
All Users
    ↓
filter()
    ↓
Active Users
    ↓
map()
    ↓
React Elements
```

---

## Rendering Different UI in a List

```jsx
function Users({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>
          {user.active
            ? `${user.name} - Active`
            : `${user.name} - Offline`}
        </li>
      ))}
    </ul>
  );
}
```

---

# 4. Keys

## Definition

A **key** is a special React prop used to identify an element among its siblings in a list.

Example:

```jsx
users.map(user => (
  <UserCard
    key={user.id}
    user={user}
  />
))
```

---

## Why Are Keys Needed?

React needs to determine which list item corresponds to which item across renders.

Example:

Before:

```text
A
B
C
```

After inserting a new item:

```text
X
A
B
C
```

React needs to understand:

```text
X → New
A → Existing
B → Existing
C → Existing
```

Keys help React preserve the identity of elements.

---

## Real-Life Example

Imagine students have roll numbers:

```text
101 → Aniket
102 → Rahul
103 → Priya
```

Even if their positions in a list change, their roll numbers identify them.

Keys work similarly.

```text
key → identity
```

---

## Good Key

Use a stable unique ID:

```jsx
users.map(user => (
  <li key={user.id}>
    {user.name}
  </li>
))
```

---

## Avoid Array Index When Possible

You can technically write:

```jsx
items.map((item, index) => (
  <li key={index}>
    {item}
  </li>
))
```

But this can cause problems when items are:

* Inserted
* Deleted
* Reordered
* Filtered

---

## Why Index Keys Can Cause Problems

Suppose:

```text
Initial:

Index 0 → A
Index 1 → B
Index 2 → C
```

Remove A:

```text
Index 0 → B
Index 1 → C
```

React sees:

```text
key 0 → A → B
key 1 → B → C
```

The identity associated with each position changed.

This can cause incorrect preservation of component state or DOM state.

---

## Example with Input

Suppose each item contains an input:

```jsx
function List({ items }) {
  return (
    <div>
      {items.map((item, index) => (
        <input
          key={index}
          defaultValue={item.name}
        />
      ))}
    </div>
  );
}
```

If items are reordered or removed, React may associate existing DOM/input state with the wrong item.

Prefer:

```jsx
<input
  key={item.id}
  defaultValue={item.name}
/>
```

---

## Important Key Rules

### Keys should be:

```text
Stable
Predictable
Unique among siblings
```

### Keys don't need to be globally unique.

This is valid:

```jsx
<div>
  {users.map(user => (
    <User key={user.id} />
  ))}
</div>
```

Another separate list can use the same IDs.

---

## Key Is Not a Normal Prop

This is important:

```jsx
<UserCard
  key={user.id}
  user={user}
/>
```

Inside `UserCard`:

```jsx
function UserCard(props) {
  console.log(props.key);
}
```

`props.key` is **not available**.

If the component needs the ID:

```jsx
<UserCard
  key={user.id}
  id={user.id}
/>
```

Then:

```jsx
function UserCard({ id }) {
  console.log(id);
}
```

---

# 5. Re-rendering

## Definition

A **re-render** occurs when React calls a component again to determine its next UI output.

Common causes include:

* State updates
* Parent re-render
* Context value changes
* External store updates
* Certain subscription updates

---

## State Update

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  console.log("Rendered");

  return (
    <button
      onClick={() =>
        setCount(prev => prev + 1)
      }
    >
      {count}
    </button>
  );
}
```

When:

```text
setCount()
```

updates the state:

```text
State changes
   ↓
Counter renders again
   ↓
React calculates new UI
```

---

## Re-render Does NOT Mean DOM Replacement

This is a very important concept.

Suppose:

```jsx
<h1>{count}</h1>
```

changes from:

```text
0
```

to:

```text
1
```

React doesn't normally destroy the entire `<h1>` and recreate the whole page.

Instead:

```text
Previous UI
   ↓
New UI
   ↓
React compares
   ↓
Necessary DOM update
```

---

## Parent Re-render

If a parent re-renders, its children are generally rendered again as part of the normal reconciliation process unless React can skip that work through mechanisms such as memoization or other optimizations.

Example:

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button
        onClick={() =>
          setCount(count + 1)
        }
      >
        {count}
      </button>

      <Child />
    </>
  );
}

function Child() {
  console.log("Child rendered");

  return <p>Hello</p>;
}
```

Updating `Parent` can cause `Child` to be rendered again.

---

## `React.memo`

`memo` can skip re-rendering a component when its props have not changed.

```jsx
import { memo } from "react";

const Child = memo(function Child() {
  console.log("Child rendered");

  return <p>Hello</p>;
});
```

However, `memo` is a performance optimization, not a guarantee that a component will never render.

---

## What Causes Re-rendering?

### 1. State update

```jsx
setCount(10);
```

### 2. Parent renders

```text
Parent
 ↓
Child
```

A parent rendering can cause its child to render.

### 3. Context changes

```jsx
const theme = useContext(ThemeContext);
```

When the consumed context value changes, the consumer is updated.

### 4. External store update

Components subscribed to an external store can update when the relevant store snapshot changes.

---

## State Update With Same Value

React can often skip an update when the new state is `Object.is`-equal to the current state.

```jsx
setCount(0);
```

when `count` is already `0` may result in no meaningful update.

For objects:

```jsx
setUser(user);
```

passes the same object reference, so React can recognize that the state value has not changed.

---

# 6. Mounting

## Definition

**Mounting** is the process of a component being created and added to the UI for the first time.

Example:

```jsx
function App() {
  return <h1>Hello</h1>;
}
```

When `App` first appears:

```text
App
 ↓
Render
 ↓
DOM committed
 ↓
Mounted
```

---

## Real-Life Example

Think about a new employee joining a company:

```text
Employee hired
     ↓
Employee added to company
     ↓
Employee starts working
```

Similarly:

```text
Component created
      ↓
Rendered
      ↓
Committed
      ↓
Mounted
```

---

## Mounting Example

```jsx
function App() {
  return (
    <div>
      <UserProfile />
    </div>
  );
}
```

When `UserProfile` first appears:

```text
UserProfile
    ↓
Render
    ↓
Commit
    ↓
Mounted
```

---

## Effect During Mount

```jsx
function UserProfile() {
  useEffect(() => {
    console.log("Mounted");
  }, []);

  return <h1>User</h1>;
}
```

The Effect setup runs after the component is committed, subject to React's Effect scheduling and development behavior.

---

# 7. Updating

## Definition

**Updating** occurs when an already-mounted component receives new props, state, context, or relevant external-store data, causing React to calculate its next UI.

---

## Example

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button
      onClick={() =>
        setCount(prev => prev + 1)
      }
    >
      {count}
    </button>
  );
}
```

Initial:

```text
count = 0
```

After clicking:

```text
count = 1
```

Flow:

```text
User clicks
    ↓
setCount()
    ↓
State changes
    ↓
Component renders again
    ↓
React compares result
    ↓
DOM updated if needed
```

---

## Props Update

Parent:

```jsx
function Parent() {
  const [name, setName] =
    useState("Aniket");

  return (
    <Child name={name} />
  );
}
```

Child:

```jsx
function Child({ name }) {
  return <h1>{name}</h1>;
}
```

When:

```jsx
setName("Rahul");
```

the child receives new props.

```text
Parent state changes
      ↓
Parent renders
      ↓
Child receives new props
      ↓
Child renders
      ↓
UI updates
```

---

## Updating an Effect

```jsx
function User({ userId }) {
  useEffect(() => {
    console.log(
      "Fetching user:",
      userId
    );
  }, [userId]);

  return <h1>{userId}</h1>;
}
```

If:

```text
userId = 1
```

changes to:

```text
userId = 2
```

React updates the component and the Effect is re-synchronized because its dependency changed.

---

# 8. Unmounting

## Definition

**Unmounting** occurs when a component is removed from the React tree.

Example:

```jsx
function App() {
  const [show, setShow] = useState(true);

  return (
    <>
      <button
        onClick={() =>
          setShow(false)
        }
      >
        Remove
      </button>

      {show && <Child />}
    </>
  );
}
```

Initially:

```text
App
 ↓
Child
```

After clicking:

```text
App
```

`Child` has been unmounted.

---

## Real-Life Example

Think about an employee leaving a company:

```text
Employee
   ↓
Leaves company
   ↓
Access removed
   ↓
Resources cleaned up
```

Similarly:

```text
Component
   ↓
Unmount
   ↓
Cleanup
```

---

## Cleanup During Unmount

```jsx
function ChatRoom() {
  useEffect(() => {
    const connection =
      createConnection();

    connection.connect();

    return () => {
      connection.disconnect();
    };
  }, []);

  return <h1>Chat</h1>;
}
```

When the component unmounts:

```text
ChatRoom removed
      ↓
Cleanup runs
      ↓
connection.disconnect()
```

---

## Common Cleanup Tasks

```text
Clear timers
Remove event listeners
Unsubscribe from subscriptions
Close WebSocket connections
Disconnect external libraries
Abort/cancel ongoing work when appropriate
```

---

# 9. Component Lifecycle Mental Model

React function components don't use the old class lifecycle methods, but you can understand their behavior using three broad phases:

```text
                 COMPONENT
                     │
                     ↓
                  MOUNT
                     │
                     ↓
                  UPDATE
                 ↙       ↘
        State/Props     Context
          change         change
                 ↘       ↙
                  UPDATE
                     │
                     ↓
                UNMOUNT
```

---

## Complete Flow

```text
                Component
                    │
                    ↓
                 Render
                    │
                    ↓
                 Commit
                    │
                    ↓
                 Mounted
                    │
                    ↓
          ┌─────────┴─────────┐
          │                   │
      State/Props          Context/
       changes           external store
          │                   │
          └─────────┬─────────┘
                    ↓
                 Render
                    ↓
                 Commit
                    ↓
                 Updated
                    │
                    ↓
              Component removed
                    │
                    ↓
                Unmounted
                    │
                    ↓
                 Cleanup
```

---

# 10. Common Rendering Mistakes

## Mistake 1 — Using Index as Key Everywhere

❌

```jsx
items.map((item, index) => (
  <Item key={index} />
))
```

Prefer:

```jsx
items.map(item => (
  <Item key={item.id} />
))
```

when a stable ID exists.

---

## Mistake 2 — Using Random Keys

❌

```jsx
<Item key={Math.random()} />
```

This creates a new key every render.

React may treat the element as a completely new element each time.

This can cause:

* Unnecessary remounts
* Lost component state
* Lost DOM/input state
* Poor performance

---

## Mistake 3 — Thinking Every Render Changes the DOM

Rendering:

```text
Component function runs
```

is different from:

```text
DOM changes
```

React can render and then determine that no DOM change is necessary.

---

## Mistake 4 — Mutating State

❌

```jsx
user.name = "Rahul";
```

Instead:

```jsx
setUser(prev => ({
  ...prev,
  name: "Rahul"
}));
```

React state should be treated as immutable.

---

## Mistake 5 — Unnecessary State

❌

```jsx
const [fullName, setFullName] =
  useState("");
```

when:

```jsx
const fullName =
  `${firstName} ${lastName}`;
```

can derive the value directly.

---

## Mistake 6 — Using `useEffect` to Force Rendering

Avoid patterns such as:

```jsx
useEffect(() => {
  setSomething(...);
}, [...]);
```

when the value can be calculated directly during rendering.

Effects should primarily synchronize with external systems.

---

## Mistake 7 — Confusing Re-rendering with Remounting

These are very different.

### Re-render

```text
Existing component
       ↓
Render again
       ↓
Same component identity
```

### Remount

```text
Old component
       ↓
Unmount
       ↓
New component
       ↓
Mount
```

Remounting resets the component's local state.

---

## Example

Changing:

```jsx
<Component key="A" />
```

to:

```jsx
<Component key="B" />
```

can cause React to treat it as a different component identity:

```text
key A
 ↓
Unmount

key B
 ↓
Mount
```

This is an important use of keys beyond lists.

---

# 11. Quick Revision

## Conditional Rendering

```jsx
{isLoggedIn
  ? <Dashboard />
  : <Login />
}
```

> Display UI based on a condition.

---

## List Rendering

```jsx
items.map(item => (
  <Item key={item.id} />
))
```

> Generate multiple elements from data.

---

## Keys

```jsx
key={item.id}
```

> Give list elements stable identity among siblings.

---

## Re-rendering

```text
State / Props / Context change
        ↓
Component renders again
```

> Re-rendering recalculates the component's UI; it does not necessarily mean the DOM changes.

---

## Mounting

```text
Component appears for first time
```

> Component is added to the React tree.

---

## Updating

```text
Existing component
        ↓
New state/props/context
        ↓
Render again
```

> React calculates the updated UI.

---

## Unmounting

```text
Component removed
        ↓
Cleanup
```

> Component is removed from the React tree.

---

# 12. Interview Revision

### Q1. What is rendering in React?

Rendering is the process of React calling components to determine what UI should be displayed based on their current inputs such as props, state, and context.

---

### Q2. Does every render cause a DOM update?

**No.**

A render calculates the next UI. React then reconciles the result and commits only the necessary DOM changes.

```text
Render ≠ DOM update
```

---

### Q3. What causes a component to re-render?

Common causes include:

```text
State update
Parent render
Consumed context change
Relevant external store update
```

---

### Q4. What is conditional rendering?

Displaying different UI based on a condition.

```jsx
{isLoggedIn
  ? <Dashboard />
  : <Login />
}
```

---

### Q5. How do you render a list in React?

Usually using JavaScript's `map()`:

```jsx
users.map(user => (
  <User
    key={user.id}
    user={user}
  />
))
```

---

### Q6. Why are keys required in React lists?

Keys help React identify which elements correspond to which data items between renders.

---

### Q7. Why shouldn't we use array indexes as keys?

If the list can be reordered, inserted into, or deleted from, indexes can change. This can cause React to associate existing component/DOM state with the wrong item.

---

### Q8. Why shouldn't we use `Math.random()` as a key?

Because the key changes every render, causing React to treat elements as new identities and potentially remount them.

---

### Q9. What is mounting?

Mounting is when a component is created and added to the React tree.

---

### Q10. What is updating?

Updating is when an existing component receives changed state, props, context, or relevant external data and React calculates its next UI.

---

### Q11. What is unmounting?

Unmounting is when a component is removed from the React tree.

---

### Q12. What happens to an Effect during unmount?

If an Effect returns a cleanup function, React runs that cleanup when the component is removed.

```jsx
useEffect(() => {
  // setup

  return () => {
    // cleanup
  };
}, []);
```

---

### Q13. What is the difference between re-rendering and remounting?

```text
Re-render
→ Existing component renders again.

Remount
→ Existing component is removed and a new component instance is created.
```

Re-rendering generally preserves state.

Remounting resets local component state because it is a new component identity.

---

### Q14. What does changing a key do?

Changing a component's key can change its identity.

```jsx
<Component key={id} />
```

If `id` changes, React can treat the old and new components as different components, resulting in unmount/mount behavior.

---

### Q15. What is the difference between `target` and `currentTarget`?

```text
target
→ Element where the event originated.

currentTarget
→ Element whose event handler is executing.
```

---

# 🧠 Final Mental Model

```text
                         RENDERING
                            │
           ┌────────────────┼────────────────┐
           ↓                ↓                ↓
      Conditional          Lists           Lifecycle
      Rendering              │                │
           │                 ↓                │
           ↓               Keys               │
       if / ternary          │                │
       && / null             │                │
                             │                │
                             └──────┬─────────┘
                                    ↓
                               COMPONENT
                                    │
                     ┌──────────────┼──────────────┐
                     ↓              ↓              ↓
                   Mount          Update         Unmount
                     │              │              │
                     ↓              ↓              ↓
                  Render         Render         Cleanup
                     │              │
                     ↓              ↓
                  Commit         Commit
                     │              │
                     ↓              ↓
                    DOM            DOM
```

---

# 🔥 Rendering Decision Guide

```text
Need to show something only sometimes?
        ↓
Conditional Rendering


Need to display array data?
        ↓
map()


Need to identify list items?
        ↓
Stable key


State/props/context changed?
        ↓
Re-render


Component appears for first time?
        ↓
Mount


Existing component receives new data?
        ↓
Update


Component removed from tree?
        ↓
Unmount + cleanup
```

---

# ⭐ One-Line Cheat Sheet

```text
Rendering
→ React calculates the UI from current component inputs.

Conditional Rendering
→ Render different UI based on a condition.

Lists
→ Generate multiple React elements from collection data.

Keys
→ Give list elements stable identity among siblings.

Re-rendering
→ Run a component again to calculate its next UI.

Mounting
→ Component is added to the React tree.

Updating
→ Existing component receives changed inputs and renders again.

Unmounting
→ Component is removed from the React tree.

Re-render ≠ DOM Update
→ React may render without changing the DOM.

Re-render ≠ Remount
→ Re-render preserves component identity; remount creates a new identity.
```

> **Core idea:** React rendering is about **calculating UI and preserving component identity**. The most important interview distinction is **render → reconciliation → commit**, and understanding that **re-rendering is not the same as changing the DOM or remounting a component**.
