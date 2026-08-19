# React A→Z Revision

**Phase 3 — JSX ⚛️**

> A revision-focused guide covering JSX expressions, conditional rendering, lists, keys, fragments, and important JSX rules.

---

## 📚 Contents

1. [JSX Expressions](#1-jsx-expressions)
2. [Conditional Rendering](#2-conditional-rendering)
3. [Lists](#3-lists)
4. [Keys](#4-keys)
5. [Fragments](#5-fragments)
6. [JSX Rules](#6-jsx-rules)
7. [Common JSX Mistakes](#7-common-jsx-mistakes)
8. [Quick Revision](#8-quick-revision)
9. [Interview Revision](#9-interview-revision)

---

# 1. JSX Expressions

## Definition

A **JSX expression** is a JavaScript expression embedded inside JSX using curly braces `{}`.

```jsx
<h1>{expression}</h1>
```

Example:

```jsx
function App() {
  const name = "Aniket";

  return <h1>Hello {name}</h1>;
}
```

Output:

```text
Hello Aniket
```

---

## Real-Life Example

Think of a printed bill:

```text
Customer: Aniket
Amount: ₹500
```

The bill format is fixed, but the actual customer and amount are dynamic.

JSX works similarly:

```jsx
<h2>Customer: {name}</h2>
<p>Amount: ₹{amount}</p>
```

The structure remains the same while the values can change.

---

## What Can Be Used Inside `{}`?

You can use JavaScript **expressions**.

### Variables

```jsx
const name = "Aniket";

return <h1>{name}</h1>;
```

### Arithmetic

```jsx
const price = 500;
const quantity = 2;

return <p>Total: ₹{price * quantity}</p>;
```

### Function Calls

```jsx
function getName() {
  return "Aniket";
}

return <h1>{getName()}</h1>;
```

### Ternary Operator

```jsx
const isLoggedIn = true;

return (
  <h1>
    {isLoggedIn ? "Welcome" : "Please Login"}
  </h1>
);
```

### Object Property

```jsx
const user = {
  name: "Aniket",
  age: 21
};

return <p>{user.name}</p>;
```

### Array Methods

```jsx
const users = ["Aniket", "Rahul", "Priya"];

return (
  <ul>
    {users.map(user => (
      <li key={user}>{user}</li>
    ))}
  </ul>
);
```

---

## Expressions vs Statements

This is an important distinction.

### Expression

Produces a value:

```jsx
name
age + 10
isLoggedIn ? "Yes" : "No"
getName()
```

These can be used inside JSX:

```jsx
<p>{name}</p>
```

### Statement

Performs an action but does not directly produce a value:

```javascript
if (isLoggedIn) {
  console.log("Logged in");
}
```

You cannot directly put a normal `if` statement inside JSX:

```jsx
// ❌ Invalid
return (
  <div>
    {if (isLoggedIn) {
      <p>Welcome</p>
    }}
  </div>
);
```

Instead, use conditional rendering techniques.

---

## JavaScript Object Issue

You cannot directly render a normal object:

```jsx
const user = {
  name: "Aniket",
  age: 21
};

// ❌ Don't do this
<div>{user}</div>
```

React cannot render a plain object as a child.

Instead:

```jsx
<div>{user.name}</div>
```

Or:

```jsx
<pre>{JSON.stringify(user, null, 2)}</pre>
```

---

# 2. Conditional Rendering

## Definition

**Conditional rendering** means displaying different UI based on a condition.

Example:

```text
User logged in?
     │
 ┌───┴───┐
Yes      No
 ↓        ↓
Home    Login
```

React provides several common ways to do this.

---

# 2.1 `if` Statement

Use `if` before returning JSX.

```jsx
function Dashboard({ isLoggedIn }) {
  if (!isLoggedIn) {
    return <h1>Please Login</h1>;
  }

  return <h1>Welcome to Dashboard</h1>;
}
```

This is useful when the entire component output changes.

---

# 2.2 Ternary Operator

## Definition

The ternary operator is:

```javascript
condition ? valueIfTrue : valueIfFalse
```

Example:

```jsx
function App({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? (
        <h1>Welcome</h1>
      ) : (
        <h1>Please Login</h1>
      )}
    </div>
  );
}
```

Real-life example:

```text
Is shop open?

YES → "Welcome"
NO  → "Closed"
```

---

# 2.3 Logical AND `&&`

Use `&&` when you want to render something **only when a condition is true**.

```jsx
function App({ isAdmin }) {
  return (
    <div>
      <h1>Dashboard</h1>

      {isAdmin && (
        <button>Delete User</button>
      )}
    </div>
  );
}
```

If:

```javascript
isAdmin = true;
```

Output:

```text
Dashboard
Delete User
```

If:

```javascript
isAdmin = false;
```

Output:

```text
Dashboard
```

---

## ⚠️ Important `&&` Pitfall

Consider:

```jsx
{items.length && <List />}
```

If `items.length` is `0`, React may render the number `0`.

So prefer:

```jsx
{items.length > 0 && <List />}
```

This is safer because the condition produces a boolean.

---

# 2.4 Multiple Conditions

```jsx
function Status({ status }) {
  if (status === "loading") {
    return <p>Loading...</p>;
  }

  if (status === "error") {
    return <p>Something went wrong.</p>;
  }

  if (status === "success") {
    return <p>Data loaded successfully.</p>;
  }

  return null;
}
```

---

# 2.5 Conditional Classes

JSX expressions can also be used to conditionally assign classes.

```jsx
function Button({ isActive }) {
  return (
    <button
      className={isActive ? "active" : "inactive"}
    >
      Button
    </button>
  );
}
```

---

## Real-Life Example

Think of a traffic signal:

```text
Condition
   ↓
Red    → STOP
Yellow → WAIT
Green  → GO
```

Conditional rendering follows the same logic:

```text
Condition
   ↓
true  → UI A
false → UI B
```

---

# 3. Lists

## Definition

**List rendering** means dynamically generating multiple JSX elements from an array of data.

The most common method is:

```javascript
array.map()
```

---

## Basic Example

```jsx
function Users() {
  const users = [
    "Aniket",
    "Rahul",
    "Priya"
  ];

  return (
    <ul>
      {users.map(user => (
        <li key={user}>
          {user}
        </li>
      ))}
    </ul>
  );
}
```

Output:

```text
• Aniket
• Rahul
• Priya
```

---

## Real-Life Example

Imagine a restaurant has a list of dishes:

```text
[
  "Pizza",
  "Burger",
  "Pasta"
]
```

Instead of manually creating:

```jsx
<li>Pizza</li>
<li>Burger</li>
<li>Pasta</li>
```

you can generate them:

```jsx
{foods.map(food => (
  <li key={food}>{food}</li>
))}
```

---

## List of Objects

Real applications usually contain objects.

```jsx
function Users() {
  const users = [
    {
      id: 1,
      name: "Aniket",
      role: "Developer"
    },
    {
      id: 2,
      name: "Rahul",
      role: "Designer"
    }
  ];

  return (
    <div>
      {users.map(user => (
        <div key={user.id}>
          <h2>{user.name}</h2>
          <p>{user.role}</p>
        </div>
      ))}
    </div>
  );
}
```

---

## Rendering Components from a List

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

Then:

```jsx
function App() {
  const users = [
    {
      id: 1,
      name: "Aniket",
      role: "Developer"
    },
    {
      id: 2,
      name: "Rahul",
      role: "Designer"
    }
  ];

  return (
    <>
      {users.map(user => (
        <UserCard
          key={user.id}
          name={user.name}
          role={user.role}
        />
      ))}
    </>
  );
}
```

---

## Filtering Before Rendering

You can combine `filter()` and `map()`.

```jsx
function Users() {
  const users = [
    { id: 1, name: "Aniket", active: true },
    { id: 2, name: "Rahul", active: false },
    { id: 3, name: "Priya", active: true }
  ];

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

Output:

```text
• Aniket
• Priya
```

---

# 4. Keys

## Definition

A **key** is a special React attribute that gives each item in a list a stable identity.

Example:

```jsx
users.map(user => (
  <UserCard
    key={user.id}
    name={user.name}
  />
))
```

Keys help React identify which list items:

* Were added
* Were removed
* Changed
* Moved

---

## Real-Life Example

Imagine a classroom with student IDs:

```text
101 → Aniket
102 → Rahul
103 → Priya
```

If Rahul leaves, the teacher can identify exactly which student was removed because each student has a unique ID.

Without IDs:

```text
Student 1
Student 2
Student 3
```

It becomes harder to reliably identify each person after changes.

Keys provide similar identity for React list items.

---

## Good Key

Use a stable unique identifier:

```jsx
<li key={user.id}>
  {user.name}
</li>
```

Best:

```text
Database ID
UUID
Stable unique identifier
```

---

## Avoid Random Keys

❌ Don't do:

```jsx
<li key={Math.random()}>
  {user.name}
</li>
```

Why?

Because a new random key is generated every render.

React then treats items as new identities instead of preserving their identity.

---

## Array Index as Key

You may see:

```jsx
users.map((user, index) => (
  <li key={index}>
    {user.name}
  </li>
))
```

This can be acceptable when:

* The list is static.
* Items are never reordered.
* Items are never inserted/removed from the middle.
* Item identity does not matter.

But it can cause problems for dynamic lists.

Example:

```text
Before:
0 → Aniket
1 → Rahul
2 → Priya

After removing Aniket:
0 → Rahul
1 → Priya
```

The indices changed.

Stable IDs are generally preferable:

```jsx
key={user.id}
```

---

## Key Is Not a Normal Prop

This is important.

```jsx
<User key={user.id} />
```

The `key` is used by React and is **not automatically available inside `User` as a prop**.

This won't work:

```jsx
function User(props) {
  console.log(props.key); // ❌
}
```

If the component needs the ID, pass it separately:

```jsx
<User
  key={user.id}
  id={user.id}
/>
```

Then:

```jsx
function User({ id }) {
  console.log(id);
}
```

---

## Key Mental Model

```text
Array Data
   ↓
map()
   ↓
React Elements
   ↓
key identifies item
   ↓
React can track identity
```

---

# 5. Fragments

## Definition

A **Fragment** allows you to group multiple JSX elements without adding an extra DOM element.

Short syntax:

```jsx
<>
  <h1>Hello</h1>
  <p>Welcome</p>
</>
```

---

## Why Do We Need Fragments?

React components must return one JSX tree.

This is invalid:

```jsx
return (
  <h1>Hello</h1>
  <p>Welcome</p>
);
```

You could use a `<div>`:

```jsx
return (
  <div>
    <h1>Hello</h1>
    <p>Welcome</p>
  </div>
);
```

But this adds an unnecessary DOM element.

Instead:

```jsx
return (
  <>
    <h1>Hello</h1>
    <p>Welcome</p>
  </>
);
```

No extra DOM element is created for the Fragment.

---

## Real-Life Example

Imagine packing multiple objects together for transportation.

You need a way to group them logically, but you don't necessarily need an additional physical box.

Fragment:

```text
Logical Group
├── Element 1
├── Element 2
└── Element 3
```

without:

```text
Extra <div>
```

---

## Full Fragment Syntax

You can also write:

```jsx
import { Fragment } from "react";

function App() {
  return (
    <Fragment>
      <h1>Hello</h1>
      <p>Welcome</p>
    </Fragment>
  );
}
```

Short form:

```jsx
<>
  <h1>Hello</h1>
  <p>Welcome</p>
</>
```

---

## Fragment with Key

The short syntax cannot receive a `key`:

```jsx
// ❌ Cannot use key here
<>
  ...
</>
```

When a Fragment needs a key, use the explicit syntax:

```jsx
import { Fragment } from "react";

function App() {
  const users = [
    { id: 1, name: "Aniket" },
    { id: 2, name: "Rahul" }
  ];

  return (
    <>
      {users.map(user => (
        <Fragment key={user.id}>
          <h2>{user.name}</h2>
          <p>Developer</p>
        </Fragment>
      ))}
    </>
  );
}
```

---

# 6. JSX Rules

JSX has several important rules.

---

## Rule 1: Return One Root Element

❌ Incorrect:

```jsx
return (
  <h1>Hello</h1>
  <p>World</p>
);
```

✅ Correct:

```jsx
return (
  <div>
    <h1>Hello</h1>
    <p>World</p>
  </div>
);
```

Or:

```jsx
return (
  <>
    <h1>Hello</h1>
    <p>World</p>
  </>
);
```

---

# Rule 2: Close Every Tag

❌ Incorrect:

```jsx
<img src="profile.png">
```

✅ Correct:

```jsx
<img src="profile.png" />
```

Similarly:

```jsx
<input />
<br />
<hr />
<img />
```

Self-closing tags must be closed.

---

# Rule 3: Use `className`

HTML:

```html
<div class="container"></div>
```

JSX:

```jsx
<div className="container"></div>
```

---

# Rule 4: Use `htmlFor`

HTML:

```html
<label for="email">
  Email
</label>
```

JSX:

```jsx
<label htmlFor="email">
  Email
</label>
```

---

# Rule 5: JavaScript Uses `{}`

```jsx
const name = "Aniket";

return (
  <h1>
    Hello {name}
  </h1>
);
```

For attributes:

```jsx
<img src={imageUrl} />
```

---

# Rule 6: Use camelCase for Most JSX Event/DOM Props

HTML:

```html
<button onclick="handleClick()">
```

JSX:

```jsx
<button onClick={handleClick}>
```

Common examples:

```text
onClick
onChange
onSubmit
onMouseEnter
onKeyDown
```

---

# Rule 7: Inline Styles Use an Object

HTML:

```html
<div style="color: red; font-size: 20px;">
```

JSX:

```jsx
<div
  style={{
    color: "red",
    fontSize: "20px"
  }}
>
  Hello
</div>
```

Notice:

```text
CSS:
font-size

JSX:
fontSize
```

---

# Rule 8: JSX Comments

Inside JSX:

```jsx
return (
  <div>
    {/* This is a JSX comment */}
    <h1>Hello</h1>
  </div>
);
```

This is not valid JSX:

```jsx
// ❌
<div>
  // comment
</div>
```

---

# Rule 9: Boolean Props

If a prop is `true`, you can write:

```jsx
<Button disabled />
```

This is equivalent to:

```jsx
<Button disabled={true} />
```

For false:

```jsx
<Button disabled={false} />
```

---

# Rule 10: Don't Put Statements Directly Inside JSX

❌ Invalid:

```jsx
return (
  <div>
    {if (isLoggedIn) {
      <h1>Welcome</h1>
    }}
  </div>
);
```

Use:

```jsx
return (
  <div>
    {isLoggedIn && <h1>Welcome</h1>}
  </div>
);
```

Or:

```jsx
return (
  <div>
    {isLoggedIn
      ? <h1>Welcome</h1>
      : <h1>Please Login</h1>
    }
  </div>
);
```

---

# Rule 11: Component Names Start with Uppercase

```jsx
<UserCard />
```

React treats this as a component.

Lowercase names are interpreted as HTML/custom DOM tags:

```jsx
<userCard />
```

So use:

```jsx
function UserCard() {}
```

and:

```jsx
<UserCard />
```

---

# Rule 12: Use Keys When Rendering Lists

❌ Avoid:

```jsx
users.map(user => (
  <li>{user.name}</li>
))
```

✅ Use:

```jsx
users.map(user => (
  <li key={user.id}>
    {user.name}
  </li>
))
```

---

# 7. Common JSX Mistakes

## Mistake 1 — Using `class`

```jsx
// ❌
<div class="box">
```

Correct:

```jsx
<div className="box">
```

---

## Mistake 2 — Forgetting `key`

```jsx
// ❌
users.map(user => (
  <li>{user.name}</li>
))
```

Correct:

```jsx
users.map(user => (
  <li key={user.id}>
    {user.name}
  </li>
))
```

---

## Mistake 3 — Rendering Objects Directly

```jsx
const user = {
  name: "Aniket"
};

// ❌
<div>{user}</div>
```

Correct:

```jsx
<div>{user.name}</div>
```

---

## Mistake 4 — Using `if` Directly Inside JSX

```jsx
// ❌
<div>
  {if (loggedIn) ...}
</div>
```

Use:

```jsx
<div>
  {loggedIn && <Dashboard />}
</div>
```

---

## Mistake 5 — Forgetting to Close Tags

```jsx
// ❌
<input>
```

Correct:

```jsx
<input />
```

---

## Mistake 6 — Calling Event Handler Immediately

❌ Wrong:

```jsx
<button onClick={handleClick()}>
  Click
</button>
```

This calls the function while rendering.

Correct:

```jsx
<button onClick={handleClick}>
  Click
</button>
```

With arguments:

```jsx
<button onClick={() => handleClick(id)}>
  Delete
</button>
```

---

# 8. Quick Revision

## JSX Expressions

```jsx
<h1>{name}</h1>
```

Use `{}` to embed JavaScript expressions.

---

## Conditional Rendering

### `if`

```jsx
if (loading) {
  return <Loading />;
}
```

### Ternary

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

### AND

```jsx
{isAdmin && <AdminPanel />}
```

---

## Lists

```jsx
users.map(user => (
  <User key={user.id} />
))
```

---

## Keys

```jsx
key={user.id}
```

Used to provide stable identity to list items.

---

## Fragments

```jsx
<>
  <Header />
  <Main />
  <Footer />
</>
```

Groups elements without adding an extra DOM element.

---

## JSX Rules

```text
1. One root element
2. Close every tag
3. className instead of class
4. htmlFor instead of for
5. JavaScript → {}
6. Events → camelCase
7. Styles → JavaScript object
8. Comments → {/* ... */}
9. Use keys for lists
10. Component names → Uppercase
```

---

# 9. Interview Revision

### Q1. What is JSX?

JSX is a JavaScript syntax extension that allows developers to write HTML-like syntax to describe React UI.

---

### Q2. Can we use JavaScript inside JSX?

Yes. JavaScript expressions can be embedded using `{}`.

```jsx
<h1>{name}</h1>
```

---

### Q3. Can we use an `if` statement directly inside JSX?

No. Statements such as `if` cannot be directly placed inside JSX expressions.

Use:

```jsx
if (...) {
  return ...;
}
```

or:

```jsx
{condition && <Component />}
```

or:

```jsx
{condition ? <A /> : <B />}
```

---

### Q4. What is conditional rendering?

Conditional rendering means displaying different UI based on a condition.

---

### Q5. Why do we use `map()` in React?

`map()` is commonly used to transform an array of data into an array of React elements.

```jsx
users.map(user => (
  <User key={user.id} />
))
```

---

### Q6. Why are keys required in React lists?

Keys provide stable identity to list items, allowing React to correctly track items when the list changes.

---

### Q7. Can we use array indexes as keys?

Yes, but only when the list is stable and items are not inserted, deleted, or reordered in a way that changes their identity.

Stable unique IDs are generally preferred.

---

### Q8. Is `key` available inside component props?

No.

```jsx
<User key={user.id} />
```

`key` is a special React attribute and is not passed as a normal prop.

If needed:

```jsx
<User
  key={user.id}
  id={user.id}
/>
```

---

### Q9. What is a Fragment?

A Fragment groups multiple JSX elements without adding an extra DOM node.

```jsx
<>
  <h1>Hello</h1>
  <p>World</p>
</>
```

---

### Q10. Why use Fragment instead of `<div>`?

When you only need grouping and don't want an additional DOM element.

---

### Q11. Why can't we render an object directly?

React cannot render a plain JavaScript object as a React child.

```jsx
// ❌
<div>{user}</div>
```

Instead:

```jsx
<div>{user.name}</div>
```

---

### Q12. What is the difference between JSX and HTML?

| JSX                                  | HTML                                |
| ------------------------------------ | ----------------------------------- |
| JavaScript syntax extension          | Markup language                     |
| Uses `className`                     | Uses `class`                        |
| Uses `htmlFor`                       | Uses `for`                          |
| Uses camelCase events                | Uses HTML event attributes          |
| JavaScript expressions use `{}`      | No equivalent JSX expression syntax |
| Transformed before browser execution | Browser parses HTML directly        |

---

# 🧠 Final Mental Model

```text
                         JSX
                          │
        ┌─────────────────┼─────────────────┐
        ↓                 ↓                 ↓
   Expressions       Conditional          Lists
        │             Rendering             │
        │                 │                 ↓
        │          ┌──────┼──────┐        Keys
        │          ↓      ↓      ↓
        │         if   ternary   &&
        │
        └──────────────────────────────────┐
                                           ↓
                                      JSX Rules
                                           │
                              ┌────────────┼────────────┐
                              ↓            ↓            ↓
                           Fragments    Attributes   Events
                              │
                              ↓
                       Clean Component UI
```

## 🔥 Remember

```text
{}           → JavaScript expression
&&           → Render when true
? :          → Choose between two UI options
map()        → Render lists
key          → Identify list items
<>...</>     → Fragment
className    → CSS class in JSX
htmlFor      → label's for attribute
onClick      → Event handler
style={{}}   → Inline styles
```

> **Core idea:** JSX is the syntax React uses to describe UI. Once you understand expressions, conditions, list rendering, keys, fragments, and JSX rules, you can build most basic React component UIs confidently.
