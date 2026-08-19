# React A→Z Revision

**Phase 2 — Components ⚛️**

> Learn how React components receive data, manage state, accept children, compose with other components, and handle form data.

---

## 📚 Contents

1. [Functional Components](#1-functional-components)
2. [Props](#2-props)
3. [State](#3-state)
4. [Children](#4-children)
5. [Component Composition](#5-component-composition)
6. [Controlled vs Uncontrolled](#6-controlled-vs-uncontrolled)
7. [Props vs State](#7-props-vs-state)
8. [Quick Revision](#8-quick-revision)
9. [Interview Revision](#9-interview-revision)

---

# 1. Functional Components

## Definition

A **Functional Component** is a JavaScript function that returns a React element describing a part of the UI.

```jsx
function Welcome() {
  return <h1>Welcome to React</h1>;
}
```

A component can then be used like:

```jsx
<Welcome />
```

---

## Simple Explanation

Think of a component as a **machine**.

You give it some input:

```text
Input
  ↓
Component
  ↓
UI
```

For example:

```text
User Name
   ↓
UserCard
   ↓
Profile Card
```

The component can receive input through **props** and maintain changing data through **state**.

---

## Basic Component

```jsx
function Header() {
  return (
    <header>
      <h1>My Website</h1>
    </header>
  );
}
```

Use it:

```jsx
function App() {
  return (
    <div>
      <Header />
      <main>Content</main>
    </div>
  );
}
```

---

## Component with JavaScript

A component is just a JavaScript function, so it can contain JavaScript logic.

```jsx
function User() {
  const name = "Aniket";
  const age = 21;

  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
    </div>
  );
}
```

---

## Component Naming Rule

Component names should start with an **uppercase letter**.

Correct:

```jsx
function UserCard() {
  return <div>User Card</div>;
}
```

Incorrect:

```jsx
function userCard() {
  return <div>User Card</div>;
}
```

React uses capitalization to distinguish components from regular HTML elements.

```jsx
<UserCard />
```

vs

```jsx
<div />
```

---

## Component with Props

```jsx
function UserCard({ name, role }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{role}</p>
    </div>
  );
}
```

Usage:

```jsx
<UserCard
  name="Aniket"
  role="Frontend Developer"
/>
```

---

## Component with State

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>{count}</h2>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

---

## Important Points

* Functional components are JavaScript functions.
* They return React elements.
* They can receive **props**.
* They can use **state and Hooks**.
* Component names should begin with an uppercase letter.
* Components should generally have a clear responsibility.

---

# 2. Props

## Definition

**Props (properties)** are read-only values passed from a parent component to a child component.

```text
Parent
  │
  │ props
  ▼
Child
```

Example:

```jsx
<UserCard name="Aniket" age={21} />
```

Here:

```text
name → "Aniket"
age  → 21
```

are props.

---

## Real-Life Example

Think of ordering food at a restaurant.

You tell the kitchen:

```text
Food: Pizza
Size: Large
Extra Cheese: Yes
```

The kitchen receives these values and prepares the food.

Similarly:

```text
Parent
   │
   │ name="Aniket"
   │ age={21}
   ▼
UserCard
```

The child component receives those values as props.

---

## Passing Props

Parent:

```jsx
function App() {
  return (
    <User
      name="Aniket"
      age={21}
      role="Developer"
    />
  );
}
```

Child:

```jsx
function User({ name, age, role }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Role: {role}</p>
    </div>
  );
}
```

---

## Props Can Have Different Data Types

### String

```jsx
<User name="Aniket" />
```

### Number

```jsx
<User age={21} />
```

### Boolean

```jsx
<User isAdmin={true} />
```

### Array

```jsx
<User skills={["React", "Node.js", "MongoDB"]} />
```

### Object

```jsx
<User
  user={{
    name: "Aniket",
    age: 21
  }}
/>
```

### Function

```jsx
<Button onClick={handleClick} />
```

### Component / JSX

```jsx
<Card
  header={<h2>User Profile</h2>}
/>
```

---

## Props Are Read-Only

A child should **not directly modify its props**.

```jsx
function User({ name }) {
  // ❌ Don't do this
  name = "Rahul";

  return <h1>{name}</h1>;
}
```

Props belong to the parent.

If the child needs to request a change, the parent can provide a callback.

---

## Passing Functions Through Props

Parent:

```jsx
function App() {
  function handleDelete() {
    console.log("Delete user");
  }

  return <User onDelete={handleDelete} />;
}
```

Child:

```jsx
function User({ onDelete }) {
  return (
    <button onClick={onDelete}>
      Delete
    </button>
  );
}
```

Data flow:

```text
Parent
  │
  │ callback prop
  ▼
Child
  │
  │ calls callback
  ▼
Parent logic executes
```

This is commonly called **lifting state up** when the callback is used to communicate changes back to the component that owns the state.

---

## Important Points

* Props are passed from **parent → child**.
* Props are **read-only** from the receiving component's perspective.
* Props can contain almost any JavaScript value.
* Props enable component reuse.
* Functions can be passed as props.
* Props are useful for communication between components.

---

# 3. State

## Definition

**State** is data managed by a component that can change over time and cause the component to render again when updated.

The most common way to create local state in a functional component is the `useState` Hook.

```jsx
const [count, setCount] = useState(0);
```

Here:

```text
count    → current state
setCount → function used to update state
0        → initial value
```

---

## Real-Life Example

Think about a **digital counter**.

Initially:

```text
Count = 0
```

You press `+`:

```text
Count = 1
```

Press again:

```text
Count = 2
```

The current count is the component's state.

---

## Basic State Example

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>{count}</h1>

      <button onClick={() => setCount(count + 1)}>
        +
      </button>

      <button onClick={() => setCount(count - 1)}>
        -
      </button>
    </div>
  );
}
```

---

## State Update Flow

```text
User clicks button
       ↓
setCount(...)
       ↓
State changes
       ↓
React schedules an update
       ↓
Component renders again
       ↓
UI reflects new state
```

---

## State Should Not Be Mutated Directly

❌ Incorrect:

```jsx
const [user, setUser] = useState({
  name: "Aniket",
  age: 21
});

user.age = 22;
```

Instead:

```jsx
setUser({
  ...user,
  age: 22
});
```

For arrays:

❌ Avoid:

```jsx
items.push(newItem);
```

Prefer:

```jsx
setItems([
  ...items,
  newItem
]);
```

---

## State with Objects

```jsx
function User() {
  const [user, setUser] = useState({
    name: "Aniket",
    age: 21
  });

  function increaseAge() {
    setUser({
      ...user,
      age: user.age + 1
    });
  }

  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.age}</p>

      <button onClick={increaseAge}>
        Increase Age
      </button>
    </div>
  );
}
```

---

## State with Arrays

```jsx
function TodoList() {
  const [todos, setTodos] = useState([]);

  function addTodo() {
    setTodos([
      ...todos,
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

For dynamic lists, prefer a stable unique ID as the key when available.

---

## Functional State Update

When the new state depends on the previous state, use the functional updater form.

```jsx
setCount(prevCount => prevCount + 1);
```

This is preferable to:

```jsx
setCount(count + 1);
```

when multiple updates may be queued based on the same previous value.

Example:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  function increase() {
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
  }

  return (
    <>
      <h1>{count}</h1>

      <button onClick={increase}>
        +2
      </button>
    </>
  );
}
```

---

## Important Points

* State represents changing data.
* State is managed by the component that owns it.
* Updating state causes React to schedule a render.
* Don't mutate state directly.
* Use the setter returned by `useState`.
* Use functional updates when the next state depends on previous state.

---

# 4. Children

## Definition

`children` is a special prop that contains whatever JSX or content is placed between a component's opening and closing tags.

Example:

```jsx
<Card>
  <h2>Hello</h2>
</Card>
```

The content:

```jsx
<h2>Hello</h2>
```

is available inside `Card` through `children`.

---

## Basic Example

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
  <h2>User Profile</h2>
  <p>Frontend Developer</p>
</Card>
```

Conceptually:

```text
<Card>
   │
   └── children
          │
          ├── <h2>
          └── <p>
```

---

## Real-Life Example

Think of a **container/box**.

You have a reusable box:

```text
┌──────────────────────┐
│                      │
│       CONTENT        │
│                      │
└──────────────────────┘
```

The box doesn't care what you put inside it.

It can contain:

```text
Text
Image
Form
Button
Another Component
```

This is exactly what `children` provides.

---

## Different Children

### Text

```jsx
<Card>
  Hello World
</Card>
```

### JSX

```jsx
<Card>
  <h1>Hello</h1>
</Card>
```

### Multiple Elements

```jsx
<Card>
  <h1>Profile</h1>
  <p>Developer</p>
  <button>Follow</button>
</Card>
```

### Component

```jsx
<Card>
  <UserProfile />
</Card>
```

---

## Children with a Layout Component

```jsx
function Layout({ children }) {
  return (
    <div>
      <header>Header</header>

      <main>
        {children}
      </main>

      <footer>Footer</footer>
    </div>
  );
}
```

Usage:

```jsx
function App() {
  return (
    <Layout>
      <h1>Dashboard</h1>
      <p>Welcome back!</p>
    </Layout>
  );
}
```

This pattern is heavily used for:

* Layouts
* Cards
* Modals
* Dialogs
* Wrappers
* Reusable containers

---

# 5. Component Composition

## Definition

**Component composition** means building complex UIs by combining smaller, reusable components.

Instead of creating one huge component:

```text
App
└── 1000 lines of code
```

we create:

```text
App
├── Navbar
├── Sidebar
├── Dashboard
│   ├── StatsCard
│   ├── Chart
│   └── RecentTasks
└── Footer
```

---

## Real-Life Example

Think about building a **house using Lego blocks**.

You don't manufacture an entire house as one block.

You combine:

```text
Block
Block
Block
Block
   ↓
Room
   ↓
House
```

React follows the same idea:

```text
Button
   ↓
Card
   ↓
Dashboard Section
   ↓
Dashboard
   ↓
Application
```

---

## Basic Composition

```jsx
function Header() {
  return <header>Header</header>;
}

function Sidebar() {
  return <aside>Sidebar</aside>;
}

function Content() {
  return <main>Content</main>;
}

function App() {
  return (
    <>
      <Header />
      <Sidebar />
      <Content />
    </>
  );
}
```

---

## Composition Using Children

```jsx
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```

Now different content can be composed inside it:

```jsx
<Card>
  <h2>Profile</h2>
  <p>Frontend Developer</p>
</Card>
```

Another:

```jsx
<Card>
  <h2>Settings</h2>
  <button>Save</button>
</Card>
```

The `Card` component is reusable because it doesn't need to know exactly what content it will contain.

---

## Composition vs Duplication

❌ Bad:

```jsx
function UserCard() {
  // lots of repeated UI
}

function AdminCard() {
  // same UI repeated again
}
```

Better:

```jsx
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```

Then compose:

```jsx
<Card>
  <User />
</Card>

<Card>
  <Admin />
</Card>
```

---

## Important Points

* Composition promotes **reusability**.
* Large UIs can be divided into smaller components.
* `children` is one of the most common composition mechanisms.
* Composition is generally preferred over creating highly complicated components.
* Components can be nested to form a component tree.

---

# 6. Controlled vs Uncontrolled

This concept is most commonly discussed with **form elements** such as:

```text
<input>
<textarea>
<select>
```

---

# 6.1 Controlled Components

## Definition

A **controlled component** is a form element whose current value is controlled by React state.

```text
React State
    ↓
Input value
    ↓
User changes input
    ↓
Event handler
    ↓
React State updates
    ↓
Input receives new value
```

---

## Example

```jsx
import { useState } from "react";

function LoginForm() {
  const [email, setEmail] = useState("");

  return (
    <form>
      <input
        type="email"
        value={email}
        onChange={e => setEmail(e.target.value)}
      />

      <p>Email: {email}</p>
    </form>
  );
}
```

Here:

```jsx
value={email}
```

means React controls the input's value.

---

## Real-Life Example

Think of a **bank account controlled by a bank system**.

Every transaction goes through the bank:

```text
User
 ↓
Request
 ↓
Bank System
 ↓
Updated Balance
```

Similarly:

```text
User types
 ↓
onChange
 ↓
React state
 ↓
value
 ↓
Input
```

React is the source of truth.

---

## Controlled Login Form

```jsx
function LoginForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  function handleSubmit(e) {
    e.preventDefault();

    console.log({
      email,
      password
    });
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={e => setEmail(e.target.value)}
      />

      <input
        type="password"
        value={password}
        onChange={e => setPassword(e.target.value)}
      />

      <button type="submit">
        Login
      </button>
    </form>
  );
}
```

---

## Advantages

Controlled components make it easy to:

* Validate input
* Display live values
* Disable buttons conditionally
* Format input
* Reset forms
* Conditionally render UI
* Keep form data in React state

---

# 6.2 Uncontrolled Components

## Definition

An **uncontrolled component** is a form element where the DOM itself maintains the current value rather than React state.

React can access the value using a **ref** when needed.

```text
User
 ↓
Input
 ↓
DOM stores value
 ↓
React reads value using ref
```

---

## Example

```jsx
import { useRef } from "react";

function LoginForm() {
  const emailRef = useRef(null);

  function handleSubmit(e) {
    e.preventDefault();

    console.log(emailRef.current.value);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        ref={emailRef}
      />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```

Here React isn't storing the input value in state.

The DOM owns the value.

---

## Real-Life Example

Imagine writing information on a **paper form**.

The form itself stores the information:

```text
User
 ↓
Paper Form
 ↓
Information stored there
```

You only read the information when needed.

That's similar to an uncontrolled input:

```text
User
 ↓
DOM Input
 ↓
Value stored in DOM
 ↓
React reads using ref
```

---

# 6.3 Controlled vs Uncontrolled

| Feature              | Controlled             | Uncontrolled          |
| -------------------- | ---------------------- | --------------------- |
| Source of truth      | React state            | DOM                   |
| Uses `value`         | Usually yes            | Usually no            |
| Uses `onChange`      | Usually yes            | Not necessarily       |
| Uses `ref`           | Not required for value | Common                |
| Validation           | Easy                   | More manual           |
| Live UI updates      | Easy                   | Less direct           |
| Form complexity      | Good for complex forms | Good for simple cases |
| React controls value | Yes                    | No                    |

---

## Controlled

```jsx
<input
  value={name}
  onChange={e => setName(e.target.value)}
/>
```

Think:

```text
React → Input
Input → React
```

---

## Uncontrolled

```jsx
<input ref={nameRef} />
```

Think:

```text
DOM → owns value
React → reads when needed
```

---

# 7. Props vs State

This is one of the most important React concepts.

| Props                           | State                                         |
| ------------------------------- | --------------------------------------------- |
| Passed from parent              | Managed by component                          |
| Read-only                       | Updated using setter/dispatch                 |
| Used to pass data               | Used to manage changing data                  |
| External to receiving component | Internal to owner                             |
| Parent controls the value       | Owner controls the value                      |
| Can contain functions           | Can contain objects, arrays, primitives, etc. |

---

## Example

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <Counter
      count={count}
      onIncrement={() => setCount(count + 1)}
    />
  );
}
```

Here:

```text
Parent state
     ↓
   count
     ↓
  props
     ↓
Counter
```

The child receives:

```jsx
count
onIncrement
```

as props.

---

# 8. Quick Revision

## Components

```text
Component
   │
   ├── Props
   │      ↓
   │   Data from parent
   │
   ├── State
   │      ↓
   │   Internal changing data
   │
   ├── Children
   │      ↓
   │   Content between component tags
   │
   └── Composition
          ↓
       Combine components
```

---

## Component Communication

```text
Parent
  │
  │ Props
  ▼
Child
```

For changing parent-owned data:

```text
Parent State
     │
     │ callback prop
     ▼
   Child
     │
     │ calls callback
     ▼
Parent updates state
```

---

## Forms

```text
Controlled
React State
    ↕
Input
```

```text
Uncontrolled
DOM
 ↑
Input
```

---

# 9. Interview Revision

### Q1. What is a component?

A component is a reusable piece of UI represented by a JavaScript function or class. Modern React primarily uses function components.

### Q2. What are props?

Props are read-only values passed from a parent component to a child component.

### Q3. Can a child modify its props?

No. Props should be treated as read-only by the receiving component.

### Q4. What is state?

State is data managed by a component that can change over time and cause React to schedule another render.

### Q5. What is the difference between props and state?

Props are inputs received from a parent, while state is data managed by the component that owns it.

### Q6. What is `children`?

`children` is a special prop containing the content passed between a component's opening and closing tags.

### Q7. What is component composition?

Component composition is the practice of building complex UIs by combining smaller reusable components.

### Q8. What is a controlled component?

A controlled form element has its current value controlled by React state.

### Q9. What is an uncontrolled component?

An uncontrolled form element stores its value in the DOM, and React can access it using a ref.

### Q10. When should you use controlled components?

Use controlled components when you need React to actively manage form values, validation, conditional UI, formatting, or other interactions.

### Q11. Why should state not be mutated directly?

React relies on state updates to schedule rendering and uses object identity during parts of its update process. Direct mutation can lead to incorrect or stale UI behavior.

### Q12. What is lifting state up?

**Lifting state up** means moving shared state to the closest common parent so multiple child components can access or modify the same source of truth through props and callbacks.

Example:

```text
        Parent
       /      \
      ↓        ↓
  Child A   Child B
```

If both children need the same state:

```text
        Parent
       State
       /   \
      ↓     ↓
 Child A  Child B
```

---

# 🧠 Final Mental Model

```text
                    COMPONENT
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
        Props         State        Children
          │             │             │
          ↓             ↓             ↓
     Parent Data   Changing Data   Nested UI
          │             │             │
          └─────────────┼─────────────┘
                        ↓
                  Component UI
                        │
                        ↓
                  Composition
                        │
                        ↓
                Complete Application
```

### Remember

```text
Props  → Data coming in
State  → Data changing inside/owned by component
Children → Content passed inside component
Composition → Build big UI from small components
Controlled → React owns form value
Uncontrolled → DOM owns form value
```

> **Core idea:** React components become powerful when they can receive data through props, manage changing data through state, accept nested content through `children`, and compose together to build larger interfaces.
