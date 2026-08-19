# React A→Z Revision

**Phase 1 — React Fundamentals ⚛️**

> A concise but complete revision guide covering the core concepts required to understand React.

---

## 📚 Contents

1. [What is React?](#1-what-is-react)
2. [Why React?](#2-why-react)
3. [SPA — Single Page Application](#3-spa--single-page-application)
4. [Components](#4-components)
5. [JSX](#5-jsx)
6. [Virtual DOM](#6-virtual-dom)
7. [Rendering](#7-rendering)
8. [Reconciliation](#8-reconciliation)
9. [Quick Revision](#9-quick-revision)

---

# 1. What is React?

## Definition

**React** is a JavaScript library for building **user interfaces (UIs)** using reusable and composable components.

React was originally developed by **Facebook (now Meta)** and is mainly used for building interactive frontend applications.

```text
React
  ↓
Build UI
  ↓
Using Components
  ↓
Based on Data/State
  ↓
Update UI efficiently
```

## Simple Explanation

Instead of writing one huge HTML page, React allows us to divide the UI into small reusable pieces called **components**.

For example, an e-commerce website can be divided into:

```text
App
├── Navbar
├── Sidebar
├── ProductList
│   └── ProductCard
├── Cart
└── Footer
```

Each part can be developed and maintained independently.

## Real-Life Example

Think of a **car manufacturing factory**.

Instead of building the entire car as one giant object, the factory has separate components:

```text
Car
├── Engine
├── Wheels
├── Steering
├── Seats
└── Dashboard
```

Each component has a specific responsibility.

React follows a similar approach:

```text
Website
├── Navbar
├── ProductCard
├── Button
├── Form
└── Footer
```

## Basic Example

```jsx
function App() {
  return (
    <div>
      <h1>Hello React</h1>
      <p>Welcome to my application.</p>
    </div>
  );
}

export default App;
```

## Important Points

* React is a **JavaScript library**, not a complete framework.
* React is primarily used for **building UIs**.
* React applications are built using **components**.
* React uses **declarative programming**.
* React uses **JSX** to describe UI.
* React efficiently updates the UI when data changes.

---

# 2. Why React?

## Definition

React is used to build **interactive, scalable, maintainable, and reusable user interfaces**.

## Why Do We Need React?

Consider a traditional JavaScript application.

```javascript
document.getElementById("name").innerText = user.name;
document.getElementById("email").innerText = user.email;
document.getElementById("profile").style.display = "block";
```

As the application becomes larger, manually manipulating the DOM becomes difficult.

React allows us to describe **what the UI should look like based on the current data**.

```jsx
function User({ name }) {
  return <h1>Hello {name}</h1>;
}
```

React handles the necessary UI updates.

## Major Advantages

### 1. Component-Based Architecture

Build the application using reusable components.

```jsx
function Button() {
  return <button>Click Me</button>;
}
```

The same component can be reused:

```jsx
<Button />
<Button />
<Button />
```

---

### 2. Reusability

Create once and reuse multiple times.

```jsx
function UserCard({ name }) {
  return (
    <div>
      <h2>{name}</h2>
    </div>
  );
}
```

```jsx
<UserCard name="Aniket" />
<UserCard name="Rahul" />
<UserCard name="Priya" />
```

---

### 3. Declarative UI

You tell React **what the UI should look like**, instead of manually describing every DOM operation.

```jsx
function Counter({ count }) {
  return <h1>{count}</h1>;
}
```

If `count` changes:

```text
Old UI → count = 1

New UI → count = 2
```

React determines what needs to change.

---

### 4. Efficient UI Updates

React uses mechanisms such as the **Virtual DOM and reconciliation** to efficiently update the browser DOM.

---

### 5. Large Ecosystem

React has a large ecosystem for:

* Routing
* State management
* Forms
* API communication
* Testing
* Animation
* Server-side rendering

Examples:

```text
React Router
Redux Toolkit
TanStack Query
React Hook Form
Next.js
```

---

# 3. SPA — Single Page Application

## Definition

A **Single Page Application (SPA)** is a web application where the browser initially loads a single HTML document and JavaScript dynamically updates the UI without requiring a complete page reload for every route.

## Traditional Website

In a traditional website:

```text
User
 ↓
Request /about
 ↓
Server
 ↓
New HTML
 ↓
Browser reloads page
```

For another page:

```text
Request /contact
 ↓
Server
 ↓
New HTML
 ↓
Page reload
```

---

## SPA

In an SPA:

```text
Browser
   ↓
Load application
   ↓
JavaScript
   ↓
Change UI dynamically
```

For example:

```text
/login
/home
/products
/profile
```

The browser can change the displayed UI without completely reloading the document.

## Real-Life Example

Think of a **restaurant menu board**.

### Traditional Website

Every time you ask for a different menu:

```text
Ask for Pizza Menu
        ↓
Remove current menu
        ↓
Bring completely new menu
```

### SPA

The restaurant has one digital screen:

```text
One Screen
   ↓
Change only the displayed section
```

The application behaves similarly.

## React SPA Example

```jsx
function App() {
  const [page, setPage] = useState("home");

  return (
    <>
      <button onClick={() => setPage("home")}>
        Home
      </button>

      <button onClick={() => setPage("about")}>
        About
      </button>

      {page === "home" && <h1>Home Page</h1>}
      {page === "about" && <h1>About Page</h1>}
    </>
  );
}
```

The UI changes without manually reloading the browser page.

> In real React applications, routing is usually handled using a routing library such as React Router.

## SPA vs Traditional MPA

| Feature          | SPA            | Traditional MPA         |
| ---------------- | -------------- | ----------------------- |
| Full page reload | Usually no     | Usually yes             |
| UI updates       | Dynamically    | New HTML page           |
| User experience  | App-like       | Page-based              |
| Initial JS       | Usually higher | Usually lower           |
| Routing          | Client-side    | Server-side             |
| Example          | React SPA      | Traditional PHP website |

---

# 4. Components

## Definition

A **component** is an independent, reusable piece of UI that contains its own structure, behavior, and sometimes state.

Components are the **building blocks of React applications**.

## Real-Life Example

Think about a house.

```text
House
├── Door
├── Window
├── Kitchen
├── Bedroom
└── Bathroom
```

Each part has a specific responsibility.

Similarly:

```text
Application
├── Navbar
├── Sidebar
├── ProductCard
├── LoginForm
└── Footer
```

---

## Functional Component

Modern React primarily uses **function components**.

```jsx
function Welcome() {
  return <h1>Welcome to React</h1>;
}
```

Use it like:

```jsx
<Welcome />
```

---

## Component with Props

```jsx
function UserCard({ name, age }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
    </div>
  );
}
```

Usage:

```jsx
<UserCard name="Aniket" age={21} />
```

Here:

```text
name → "Aniket"
age  → 21
```

are **props**.

---

## Component Composition

Components can be combined together.

```jsx
function Navbar() {
  return <nav>Navbar</nav>;
}

function Footer() {
  return <footer>Footer</footer>;
}

function App() {
  return (
    <>
      <Navbar />
      <main>Content</main>
      <Footer />
    </>
  );
}
```

This is called **component composition**.

## Important Points

* Components should generally follow the **Single Responsibility Principle**.
* Components can receive data through **props**.
* Components can maintain data using **state**.
* Components can contain other components.
* Component names should start with an **uppercase letter**.

---

# 5. JSX

## Definition

**JSX (JavaScript XML)** is a syntax extension for JavaScript that allows us to write HTML-like UI structures inside JavaScript.

Example:

```jsx
const element = <h1>Hello React</h1>;
```

JSX is **not HTML** and browsers do not directly understand JSX.

It is transformed into JavaScript.

Conceptually:

```jsx
<h1>Hello React</h1>
```

becomes something similar to:

```javascript
React.createElement(
  "h1",
  null,
  "Hello React"
);
```

Modern React tooling typically transforms JSX into React element creation calls using the JSX transform.

---

## JavaScript Inside JSX

Use `{}` to insert JavaScript expressions.

```jsx
const name = "Aniket";

function App() {
  return <h1>Hello {name}</h1>;
}
```

Output:

```text
Hello Aniket
```

---

## Expressions

You can use JavaScript expressions:

```jsx
function App() {
  const age = 21;

  return (
    <div>
      <h1>Age: {age}</h1>
      <p>{age >= 18 ? "Adult" : "Minor"}</p>
    </div>
  );
}
```

---

## JSX Attributes

HTML:

```html
<div class="container"></div>
```

JSX:

```jsx
<div className="container"></div>
```

Some common differences:

```text
HTML          JSX
-------------------------
class         className
for           htmlFor
onclick       onClick
tabindex      tabIndex
```

---

## JSX Must Have One Root

This is invalid:

```jsx
return (
  <h1>Hello</h1>
  <p>World</p>
);
```

Use a parent:

```jsx
return (
  <div>
    <h1>Hello</h1>
    <p>World</p>
  </div>
);
```

Or a Fragment:

```jsx
return (
  <>
    <h1>Hello</h1>
    <p>World</p>
  </>
);
```

---

## Real-Life Example

JSX is like writing a **template with embedded instructions**.

For example:

```jsx
<h1>Hello {user.name}</h1>
```

is conceptually saying:

> "Create a heading and dynamically insert the user's name."

---

# 6. Virtual DOM

## Definition

The **Virtual DOM** is an in-memory representation of the UI that React uses to determine what changes need to be applied to the actual browser DOM.

Simplified flow:

```text
React State Changes
       ↓
New React Element Tree
       ↓
Compare with previous tree
       ↓
Determine required changes
       ↓
Update browser DOM
```

## Real DOM vs Virtual Representation

### Real DOM

The browser maintains the actual document:

```text
HTML
 ↓
DOM
 ↓
Browser
```

### React

React maintains a representation of the UI:

```text
React Components
       ↓
React Element Tree
       ↓
Compare changes
       ↓
Real DOM
```

> A common interview simplification is to call this representation the "Virtual DOM." Modern React internals are more nuanced, so avoid thinking of it as simply a second copy of the browser DOM.

---

## Real-Life Example

Imagine an office notice board.

Current board:

```text
Name: Aniket
Age: 21
City: Pune
```

Only the city changes:

```text
Name: Aniket
Age: 21
City: Mumbai
```

Instead of rebuilding the entire board, you can replace only:

```text
Pune → Mumbai
```

This is similar to the goal of React's update process.

---

## Example

```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>Counter</h1>

      <p>{count}</p>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

When the state changes:

```text
count = 0
   ↓
setCount(1)
   ↓
React renders updated UI
   ↓
React determines what changed
   ↓
Required DOM update
```

---

# 7. Rendering

## Definition

**Rendering in React** means React calculates what the UI should look like based on the current props, state, and other inputs.

Rendering does **not necessarily mean updating the real DOM**.

This distinction is very important.

---

## Initial Render

When the application starts:

```text
React Application
       ↓
Component function executes
       ↓
React creates element tree
       ↓
React commits required DOM changes
       ↓
Browser displays UI
```

Example:

```jsx
function App() {
  return <h1>Hello React</h1>;
}
```

React renders:

```text
<h1>Hello React</h1>
```

into the browser.

---

## Re-render

A component can render again when relevant inputs change, such as:

* State changes
* Props change
* Context value changes
* Parent renders and passes new values

Example:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <h1>{count}</h1>

      <button onClick={() => setCount(count + 1)}>
        +
      </button>
    </>
  );
}
```

When:

```javascript
setCount(count + 1);
```

runs:

```text
State changes
    ↓
React schedules update
    ↓
Component renders again
    ↓
React compares results
    ↓
DOM is updated if necessary
```

---

## Important Interview Point

### Re-render ≠ DOM update

A component rendering again does **not** mean React replaces the entire DOM.

For example:

```text
Component renders
      ↓
New UI description
      ↓
Compare with previous result
      ↓
Only necessary DOM changes
```

---

# 8. Reconciliation

## Definition

**Reconciliation** is React's process of comparing the previous rendered element tree with the new one to determine what needs to change in the UI.

In simplified terms:

```text
Previous UI
    +
New UI
    ↓
Compare
    ↓
Determine changes
    ↓
Commit changes to DOM
```

---

## Real-Life Example

Imagine a teacher checking two versions of a student list.

### Previous

```text
1. Aniket
2. Rahul
3. Priya
```

### New

```text
1. Aniket
2. Rahul
3. Neha
```

The teacher doesn't rewrite the entire list.

They identify:

```text
Priya → Neha
```

React performs a similar comparison process for the rendered UI.

---

## Example

```jsx
function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return (
    <div>
      <h1>My App</h1>

      {isLoggedIn ? (
        <p>Welcome back!</p>
      ) : (
        <p>Please log in.</p>
      )}
    </div>
  );
}
```

Initially:

```text
<h1>My App</h1>
<p>Please log in.</p>
```

After:

```javascript
setIsLoggedIn(true);
```

React gets a new UI description:

```text
<h1>My App</h1>
<p>Welcome back!</p>
```

React determines the relevant change.

---

## Reconciliation and Keys

Keys are especially important when rendering lists.

```jsx
const users = [
  { id: 1, name: "Aniket" },
  { id: 2, name: "Rahul" },
  { id: 3, name: "Priya" }
];

function Users() {
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

The `key` helps React identify list items across renders.

### Good Key

```jsx
key={user.id}
```

### Usually Bad Key

```jsx
key={Math.random()}
```

Random keys change on every render and prevent React from reliably preserving item identity.

Using an array index as a key can also cause problems when list items are inserted, removed, or reordered.

---

# 9. Quick Revision

## React Fundamentals in One Diagram

```text
                    REACT
                      │
                      ▼
                Components
                      │
                      ▼
                    JSX
                      │
                      ▼
              React Element Tree
                      │
                      ▼
                  Rendering
                      │
                      ▼
             Reconciliation
                      │
                      ▼
             Required DOM Changes
                      │
                      ▼
                   Browser
```

---

## One-Line Definitions

| Concept            | One-Line Definition                                                                                      |
| ------------------ | -------------------------------------------------------------------------------------------------------- |
| **React**          | JavaScript library for building component-based user interfaces.                                         |
| **SPA**            | Application that dynamically updates the UI without full page reloads for normal client-side navigation. |
| **Component**      | Reusable and independent piece of a React UI.                                                            |
| **JSX**            | JavaScript syntax extension used to describe React UI using HTML-like syntax.                            |
| **Virtual DOM**    | In-memory representation of UI used during React's update process.                                       |
| **Rendering**      | Process where React calculates the UI representation from current inputs.                                |
| **Reconciliation** | Process of determining what changed between previous and new rendered output.                            |

---

# 🔥 Interview Revision

### Q1. What is React?

React is a JavaScript library developed by Meta for building reusable and interactive user interfaces using components.

### Q2. Why is React popular?

Because it provides:

* Component-based architecture
* Reusability
* Declarative UI
* Efficient UI updates
* Large ecosystem
* Strong community support

### Q3. What is JSX?

JSX is a JavaScript syntax extension that allows developers to write HTML-like UI syntax inside JavaScript.

### Q4. Is JSX HTML?

**No.**

JSX looks similar to HTML but is JavaScript syntax that is transformed into React element creation code.

### Q5. What is Virtual DOM?

It is an in-memory representation of the UI that React uses as part of its process for determining efficient DOM updates.

### Q6. What is reconciliation?

Reconciliation is React's process of comparing the previous and new rendered element trees to determine the necessary UI changes.

### Q7. Does every re-render update the DOM?

**No.**

A re-render means React recalculates the component's UI. React then determines whether actual DOM changes are required.

### Q8. Why are keys used in React lists?

Keys provide stable identity for list items so React can correctly track items between renders.

---

# 🧠 Remember This

```text
React
 ↓
Components build UI
 ↓
JSX describes UI
 ↓
State/Props change
 ↓
Component renders
 ↓
React compares previous/new output
 ↓
Reconciliation
 ↓
Necessary DOM changes
 ↓
Browser updates UI
```

> **Core idea:** You describe the UI you want; React manages the process of keeping the browser UI synchronized with your application's data.
