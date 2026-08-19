# React A→Z Revision

**Phase 6 — Events & Forms 📝**

> React applications are highly interactive. Events allow components to respond to user actions, while forms collect, validate, and submit user data.

---

## 📚 Contents

1. [Event Handling](#1-event-handling)
2. [Synthetic Events](#2-synthetic-events)
3. [Controlled Inputs](#3-controlled-inputs)
4. [Uncontrolled Inputs](#4-uncontrolled-inputs)
5. [Form Validation](#5-form-validation)
6. [Form Submission](#6-form-submission)
7. [Controlled vs Uncontrolled Forms](#7-controlled-vs-uncontrolled-forms)
8. [Complete Form Example](#8-complete-form-example)
9. [Common Mistakes](#9-common-mistakes)
10. [Quick Revision](#10-quick-revision)
11. [Interview Revision](#11-interview-revision)

---

# 1. Event Handling

## Definition

**Event handling** in React is the process of responding to user interactions such as:

* Clicks
* Typing
* Form submission
* Mouse movement
* Keyboard input
* Focus/blur
* Change events

React uses event handler props such as:

```jsx
onClick
onChange
onSubmit
onFocus
onBlur
onKeyDown
```

---

## Real-Life Example

Think about a **doorbell**.

```text
Person presses button
        ↓
Doorbell detects event
        ↓
Bell rings
```

React works similarly:

```text
User clicks button
        ↓
onClick detects event
        ↓
Handler function executes
        ↓
UI/state may change
```

---

## Basic `onClick`

```jsx
function Button() {
  function handleClick() {
    console.log("Button clicked");
  }

  return (
    <button onClick={handleClick}>
      Click Me
    </button>
  );
}
```

---

## Important: Pass Function, Don't Call It

❌ Wrong:

```jsx
<button onClick={handleClick()}>
  Click
</button>
```

This calls `handleClick()` while rendering.

✅ Correct:

```jsx
<button onClick={handleClick}>
  Click
</button>
```

With arguments:

```jsx
<button onClick={() => handleClick(10)}>
  Click
</button>
```

---

## Common React Events

| Event           | Purpose                |
| --------------- | ---------------------- |
| `onClick`       | Mouse click            |
| `onChange`      | Input value change     |
| `onSubmit`      | Form submission        |
| `onFocus`       | Element receives focus |
| `onBlur`        | Element loses focus    |
| `onMouseEnter`  | Pointer enters element |
| `onMouseLeave`  | Pointer leaves element |
| `onKeyDown`     | Keyboard key pressed   |
| `onKeyUp`       | Keyboard key released  |
| `onInput`       | Input value changes    |
| `onDoubleClick` | Double click           |

---

## Passing Event Object

React passes an event object to the handler.

```jsx
function Button() {
  function handleClick(event) {
    console.log(event);
  }

  return (
    <button onClick={handleClick}>
      Click
    </button>
  );
}
```

---

## Event + State

Events are commonly used to update state.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(prev => prev + 1);
  }

  return (
    <>
      <h1>{count}</h1>

      <button onClick={handleClick}>
        Increment
      </button>
    </>
  );
}
```

Flow:

```text
User click
   ↓
onClick
   ↓
handleClick()
   ↓
setCount()
   ↓
State update
   ↓
React render
   ↓
UI update
```

---

# 2. Synthetic Events

## Definition

A **SyntheticEvent** is React's normalized event object that provides a consistent interface for browser events across supported environments.

Example:

```jsx
function Button() {
  function handleClick(event) {
    console.log(event.type);
    console.log(event.target);
  }

  return (
    <button onClick={handleClick}>
      Click
    </button>
  );
}
```

---

## Why Does React Use Synthetic Events?

Different browsers historically had differences in how DOM events behaved.

React provides a consistent event interface:

```text
Browser Event
     ↓
React Event System
     ↓
SyntheticEvent
     ↓
Your Handler
```

---

## Common Properties

```jsx
function handleClick(event) {
  console.log(event.type);
  console.log(event.target);
  console.log(event.currentTarget);
}
```

### `event.type`

Returns the event type.

```text
click
change
submit
keydown
```

---

### `event.target`

The element that originally triggered the event.

```jsx
function handleClick(event) {
  console.log(event.target);
}
```

---

### `event.currentTarget`

The element whose event handler is currently executing.

This distinction becomes important with event propagation.

---

## Example

```jsx
function App() {
  function handleClick(event) {
    console.log("Target:", event.target);
    console.log(
      "Current Target:",
      event.currentTarget
    );
  }

  return (
    <button onClick={handleClick}>
      <span>Click Me</span>
    </button>
  );
}
```

If you click the `<span>`:

```text
event.target
→ span

event.currentTarget
→ button
```

---

## `preventDefault()`

Used when you want to prevent the browser's default behavior.

Example:

```jsx
function Form() {
  function handleSubmit(event) {
    event.preventDefault();

    console.log("Form submitted");
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```

Without `preventDefault()`, a normal browser form submission may navigate/reload the document depending on the form's action and configuration.

---

## `stopPropagation()`

Stops the event from propagating further through the event propagation path.

```jsx
function App() {
  function handleParentClick() {
    console.log("Parent clicked");
  }

  function handleChildClick(event) {
    event.stopPropagation();

    console.log("Child clicked");
  }

  return (
    <div onClick={handleParentClick}>
      <button onClick={handleChildClick}>
        Click
      </button>
    </div>
  );
}
```

Clicking the button:

```text
Child clicked
```

The parent handler does not run because propagation was stopped.

---

## Real-Life Example

Think of an event as a message traveling through an organization:

```text
Employee
   ↓
Manager
   ↓
Director
```

`stopPropagation()` is like saying:

> "Don't pass this message further."

---

## Important Note

Modern React no longer uses the old event pooling behavior that required developers to call `event.persist()` in React 16 and earlier.

You generally do **not** need:

```jsx
event.persist();
```

in modern React.

---

# 3. Controlled Inputs

## Definition

A **controlled input** is an input whose current value is controlled by React state.

```text
React State
     ↓
Input value
     ↓
User types
     ↓
onChange
     ↓
State updates
     ↓
Input receives new value
```

---

## Basic Example

```jsx
import { useState } from "react";

function Form() {
  const [name, setName] = useState("");

  return (
    <input
      value={name}
      onChange={event =>
        setName(event.target.value)
      }
    />
  );
}
```

Here:

```jsx
value={name}
```

means React owns the input's current value.

---

## Real-Life Example

Think of a **digital banking form** where every change is immediately recorded by the application.

```text
User types
   ↓
Application receives value
   ↓
Application updates stored value
   ↓
Input displays current value
```

React state acts as the source of truth.

---

## Multiple Controlled Inputs

```jsx
function RegistrationForm() {
  const [form, setForm] = useState({
    name: "",
    email: "",
    password: ""
  });

  function handleChange(event) {
    const { name, value } = event.target;

    setForm(prev => ({
      ...prev,
      [name]: value
    }));
  }

  return (
    <form>
      <input
        name="name"
        value={form.name}
        onChange={handleChange}
      />

      <input
        name="email"
        value={form.email}
        onChange={handleChange}
      />

      <input
        name="password"
        type="password"
        value={form.password}
        onChange={handleChange}
      />
    </form>
  );
}
```

---

## Why Use `name`?

The `name` attribute allows one handler to update different fields.

```jsx
const { name, value } = event.target;
```

Then:

```jsx
setForm(prev => ({
  ...prev,
  [name]: value
}));
```

If the user changes:

```text
name → Aniket
```

React performs:

```text
form.name = "Aniket"
```

If the user changes:

```text
email → aniket@example.com
```

React performs:

```text
form.email = "aniket@example.com"
```

---

## Controlled Checkbox

Checkboxes use `checked`, not `value`, to represent their checked state.

```jsx
function Terms() {
  const [accepted, setAccepted] =
    useState(false);

  return (
    <label>
      <input
        type="checkbox"
        checked={accepted}
        onChange={e =>
          setAccepted(e.target.checked)
        }
      />

      Accept Terms
    </label>
  );
}
```

---

## Controlled Select

```jsx
function RoleSelect() {
  const [role, setRole] = useState("");

  return (
    <select
      value={role}
      onChange={e =>
        setRole(e.target.value)
      }
    >
      <option value="">
        Select Role
      </option>

      <option value="developer">
        Developer
      </option>

      <option value="designer">
        Designer
      </option>
    </select>
  );
}
```

---

# 4. Uncontrolled Inputs

## Definition

An **uncontrolled input** stores its current value in the DOM rather than React state.

React can access the value using a `ref`.

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

## Basic Example

```jsx
import { useRef } from "react";

function Form() {
  const nameRef = useRef(null);

  function handleSubmit(event) {
    event.preventDefault();

    console.log(
      nameRef.current.value
    );
  }

  return (
    <form onSubmit={handleSubmit}>
      <input ref={nameRef} />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```

---

## `defaultValue`

Uncontrolled inputs can use `defaultValue` for their initial value.

```jsx
<input
  defaultValue="Aniket"
  ref={nameRef}
/>
```

Unlike:

```jsx
value="Aniket"
```

`defaultValue` does not make React continuously control the input.

---

## Controlled vs Uncontrolled

| Controlled                 | Uncontrolled                           |
| -------------------------- | -------------------------------------- |
| React state controls value | DOM controls value                     |
| Uses `value` / `checked`   | Uses `defaultValue` / `defaultChecked` |
| Usually uses `onChange`    | Ref can read value                     |
| Easy live validation       | Simpler for some basic forms           |
| More explicit              | More DOM-driven                        |
| More React state updates   | Fewer React state updates              |

---

## When to Use Controlled Inputs

Good when you need:

```text
Live validation
Conditional UI
Formatting
Dynamic field behavior
Real-time values
Disable/enable buttons
```

---

## When to Use Uncontrolled Inputs

Useful when:

```text
Simple forms
DOM-centric integrations
You only need values at submission
Using APIs/libraries that work with refs
```

---

# 5. Form Validation

## Definition

**Form validation** checks whether user-provided data satisfies the required rules before accepting or submitting it.

Example rules:

```text
Name → Required
Email → Valid format
Password → Minimum 8 characters
Age → Must be >= 18
```

---

## Real-Life Example

Think about an airport security checkpoint.

Before allowing a passenger through:

```text
Check ticket
   ↓
Check ID
   ↓
Check requirements
   ↓
Allow / Reject
```

Form validation works similarly:

```text
User Input
   ↓
Validation
   ↓
Valid?
 ┌─┴─┐
Yes  No
 ↓    ↓
Submit Error
```

---

## Basic Validation

```jsx
import { useState } from "react";

function SignupForm() {
  const [email, setEmail] = useState("");
  const [error, setError] = useState("");

  function handleSubmit(event) {
    event.preventDefault();

    if (!email) {
      setError("Email is required");
      return;
    }

    setError("");

    console.log("Form submitted");
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={e =>
          setEmail(e.target.value)
        }
      />

      {error && (
        <p>{error}</p>
      )}

      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```

---

## Multiple Validation Rules

```jsx
function validate(form) {
  const errors = {};

  if (!form.name.trim()) {
    errors.name = "Name is required";
  }

  if (!form.email.trim()) {
    errors.email = "Email is required";
  } else if (
    !form.email.includes("@")
  ) {
    errors.email = "Invalid email";
  }

  if (form.password.length < 8) {
    errors.password =
      "Password must contain at least 8 characters";
  }

  return errors;
}
```

Use:

```jsx
function handleSubmit(event) {
  event.preventDefault();

  const errors = validate(form);

  if (Object.keys(errors).length > 0) {
    setErrors(errors);
    return;
  }

  // Submit
}
```

---

## Client-Side vs Server-Side Validation

### Client-Side

Runs in the browser:

```text
Browser
 ↓
Validate
 ↓
Submit
```

Benefits:

* Immediate feedback
* Better user experience
* Avoid unnecessary requests

But client-side validation **cannot be trusted for security**.

---

### Server-Side

Runs on the server:

```text
Browser
 ↓
Request
 ↓
Server
 ↓
Validate
 ↓
Accept / Reject
```

Server-side validation is essential because users can bypass or manipulate client-side JavaScript.

---

## Best Practice

Use both:

```text
Client Validation
       ↓
Better UX

Server Validation
       ↓
Security + Data Integrity
```

---

# 6. Form Submission

## Definition

**Form submission** is the process of collecting form data, validating it, and sending it to the appropriate destination.

In React, forms are commonly handled using the form's `onSubmit` event.

---

## Basic Form Submission

```jsx
function LoginForm() {
  function handleSubmit(event) {
    event.preventDefault();

    console.log("Submitting form...");
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="email" />

      <input type="password" />

      <button type="submit">
        Login
      </button>
    </form>
  );
}
```

---

## Why `preventDefault()`?

A traditional browser form submission can cause a document navigation/reload.

React applications commonly prevent that default behavior:

```jsx
event.preventDefault();
```

Then React can handle the submission itself.

---

## Complete API Submission

```jsx
import { useState } from "react";

function LoginForm() {
  const [form, setForm] = useState({
    email: "",
    password: ""
  });

  const [loading, setLoading] =
    useState(false);

  const [error, setError] =
    useState("");

  function handleChange(event) {
    const {
      name,
      value
    } = event.target;

    setForm(prev => ({
      ...prev,
      [name]: value
    }));
  }

  async function handleSubmit(event) {
    event.preventDefault();

    setLoading(true);
    setError("");

    try {
      const response = await fetch(
        "/api/login",
        {
          method: "POST",

          headers: {
            "Content-Type":
              "application/json"
          },

          body: JSON.stringify(form)
        }
      );

      if (!response.ok) {
        throw new Error(
          "Login failed"
        );
      }

      const data =
        await response.json();

      console.log(data);
    } catch (error) {
      setError(
        error.message
      );
    } finally {
      setLoading(false);
    }
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="email"
        type="email"
        value={form.email}
        onChange={handleChange}
      />

      <input
        name="password"
        type="password"
        value={form.password}
        onChange={handleChange}
      />

      {error && (
        <p>{error}</p>
      )}

      <button
        type="submit"
        disabled={loading}
      >
        {loading
          ? "Logging in..."
          : "Login"}
      </button>
    </form>
  );
}
```

---

## Form Submission Flow

```text
User fills form
      ↓
Input state updates
      ↓
User clicks Submit
      ↓
onSubmit
      ↓
preventDefault()
      ↓
Validate
      ↓
Valid?
  ┌───┴───┐
 No       Yes
 ↓         ↓
Errors    API Request
            ↓
       Loading State
            ↓
       Server Response
          /       \
       Success    Error
         ↓          ↓
       Update     Show Error
         UI
```

---

# 7. Controlled vs Uncontrolled Forms

## Controlled Form

```jsx
function Form() {
  const [email, setEmail] = useState("");

  return (
    <form>
      <input
        value={email}
        onChange={e =>
          setEmail(e.target.value)
        }
      />
    </form>
  );
}
```

Flow:

```text
Input
 ↓
onChange
 ↓
React State
 ↓
value
 ↓
Input
```

---

## Uncontrolled Form

```jsx
function Form() {
  const emailRef = useRef(null);

  function handleSubmit(event) {
    event.preventDefault();

    console.log(
      emailRef.current.value
    );
  }

  return (
    <form onSubmit={handleSubmit}>
      <input ref={emailRef} />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```

Flow:

```text
Input
 ↓
DOM
 ↓
ref
 ↓
Read on submit
```

---

# 8. Complete Form Example

This example combines:

* Controlled inputs
* Validation
* Form submission
* Loading state
* Error handling

```jsx
import { useState } from "react";

function RegistrationForm() {
  const [form, setForm] = useState({
    name: "",
    email: "",
    password: ""
  });

  const [errors, setErrors] =
    useState({});

  const [loading, setLoading] =
    useState(false);

  function handleChange(event) {
    const {
      name,
      value
    } = event.target;

    setForm(prev => ({
      ...prev,
      [name]: value
    }));
  }

  function validate() {
    const errors = {};

    if (!form.name.trim()) {
      errors.name =
        "Name is required";
    }

    if (!form.email.trim()) {
      errors.email =
        "Email is required";
    } else if (
      !/^\S+@\S+\.\S+$/.test(
        form.email
      )
    ) {
      errors.email =
        "Enter a valid email";
    }

    if (form.password.length < 8) {
      errors.password =
        "Password must contain at least 8 characters";
    }

    return errors;
  }

  async function handleSubmit(event) {
    event.preventDefault();

    const validationErrors =
      validate();

    if (
      Object.keys(
        validationErrors
      ).length > 0
    ) {
      setErrors(
        validationErrors
      );

      return;
    }

    setErrors({});
    setLoading(true);

    try {
      const response =
        await fetch(
          "/api/register",
          {
            method: "POST",

            headers: {
              "Content-Type":
                "application/json"
            },

            body: JSON.stringify(form)
          }
        );

      if (!response.ok) {
        throw new Error(
          "Registration failed"
        );
      }

      console.log(
        "Registration successful"
      );

      setForm({
        name: "",
        email: "",
        password: ""
      });
    } catch (error) {
      console.error(error);
    } finally {
      setLoading(false);
    }
  }

  return (
    <form
      onSubmit={handleSubmit}
      noValidate
    >
      <div>
        <label htmlFor="name">
          Name
        </label>

        <input
          id="name"
          name="name"
          value={form.name}
          onChange={handleChange}
        />

        {errors.name && (
          <p>{errors.name}</p>
        )}
      </div>

      <div>
        <label htmlFor="email">
          Email
        </label>

        <input
          id="email"
          name="email"
          type="email"
          value={form.email}
          onChange={handleChange}
        />

        {errors.email && (
          <p>{errors.email}</p>
        )}
      </div>

      <div>
        <label htmlFor="password">
          Password
        </label>

        <input
          id="password"
          name="password"
          type="password"
          value={form.password}
          onChange={handleChange}
        />

        {errors.password && (
          <p>{errors.password}</p>
        )}
      </div>

      <button
        type="submit"
        disabled={loading}
      >
        {loading
          ? "Creating..."
          : "Create Account"}
      </button>
    </form>
  );
}
```

---

# 9. Common Mistakes

## Mistake 1 — Calling Event Handler Immediately

❌

```jsx
<button onClick={handleClick()}>
```

✅

```jsx
<button onClick={handleClick}>
```

---

## Mistake 2 — Forgetting `preventDefault()`

For a React-controlled form:

❌

```jsx
function handleSubmit(event) {
  console.log("Submit");
}
```

Usually:

```jsx
function handleSubmit(event) {
  event.preventDefault();

  // custom submission logic
}
```

---

## Mistake 3 — Using `value` Without `onChange`

This creates a controlled input that cannot be edited normally.

❌

```jsx
<input value={name} />
```

If you intend it to be controlled, provide an update path:

```jsx
<input
  value={name}
  onChange={e =>
    setName(e.target.value)
  }
/>
```

---

## Mistake 4 — Mixing Controlled and Uncontrolled Modes

Avoid switching an input between:

```text
controlled → uncontrolled
```

or:

```text
uncontrolled → controlled
```

during its lifetime.

For example:

❌

```jsx
<input value={user.name} />
```

if `user.name` starts as `undefined` and later becomes a string.

Prefer initializing it consistently:

```jsx
const [name, setName] = useState("");
```

---

## Mistake 5 — Mutating Form State

❌

```jsx
form.email = "test@example.com";
```

Use:

```jsx
setForm(prev => ({
  ...prev,
  email: "test@example.com"
}));
```

---

## Mistake 6 — Only Validating on the Client

Client validation improves UX but is **not a security boundary**.

Always validate important data on the server as well.

---

## Mistake 7 — Not Handling Loading State

For asynchronous submission:

```text
Submit
 ↓
Request
 ↓
???
```

Users may click multiple times.

Use:

```jsx
<button disabled={loading}>
  {loading ? "Submitting..." : "Submit"}
</button>
```

---

# 10. Quick Revision

## Event Handling

```jsx
<button onClick={handleClick}>
```

React responds to user events through event handler props.

---

## Synthetic Events

```text
Browser Event
     ↓
React Event System
     ↓
SyntheticEvent
     ↓
Handler
```

Provides a consistent event interface.

---

## Controlled Input

```jsx
<input
  value={value}
  onChange={handleChange}
/>
```

```text
React State ↔ Input
```

---

## Uncontrolled Input

```jsx
<input ref={inputRef} />
```

```text
DOM → Input Value
       ↓
      Ref
```

---

## Validation

```text
Input
 ↓
Validate
 ↓
Valid?
 ├── No → Show Error
 └── Yes → Submit
```

---

## Form Submission

```jsx
<form onSubmit={handleSubmit}>
```

```text
Submit
 ↓
preventDefault()
 ↓
Validate
 ↓
API Request
 ↓
Handle response
```

---

# 11. Interview Revision

### Q1. How are events handled in React?

React handles events using camelCase event props such as:

```jsx
<button onClick={handleClick}>
```

Event handlers are functions that execute when the corresponding event occurs.

---

### Q2. What is a SyntheticEvent?

A SyntheticEvent is React's normalized event object that provides a consistent interface for browser events.

---

### Q3. What is the difference between `target` and `currentTarget`?

```text
target
→ Element that initiated the event.

currentTarget
→ Element whose handler is currently executing.
```

Example:

```jsx
function handleClick(event) {
  console.log(event.target);
  console.log(event.currentTarget);
}
```

---

### Q4. What does `preventDefault()` do?

It prevents the browser's default action for an event.

For forms, it is commonly used to prevent the browser from performing a normal document submission/navigation while React handles the submission.

---

### Q5. What does `stopPropagation()` do?

It prevents an event from continuing through its propagation path.

---

### Q6. What is a controlled component?

A controlled input gets its current value from React state and updates that state through an event handler.

```jsx
<input
  value={name}
  onChange={e =>
    setName(e.target.value)
  }
/>
```

---

### Q7. What is an uncontrolled component?

An uncontrolled input stores its value in the DOM, and React accesses it when necessary, commonly through a ref.

---

### Q8. What is the difference between `value` and `defaultValue`?

```text
value
→ Used to control the current input value.

defaultValue
→ Sets the initial value of an uncontrolled input.
```

---

### Q9. Why is `onChange` used with controlled inputs?

It allows React to receive the latest input value and update the corresponding state.

---

### Q10. How do you handle multiple controlled inputs?

Give each input a `name` and use a common handler.

```jsx
function handleChange(event) {
  const {
    name,
    value
  } = event.target;

  setForm(prev => ({
    ...prev,
    [name]: value
  }));
}
```

---

### Q11. Where should form validation happen?

Ideally:

```text
Client
→ User experience

Server
→ Security and data integrity
```

Never rely exclusively on client-side validation for security.

---

### Q12. Why do we use `onSubmit` instead of only handling button clicks?

Because `onSubmit` represents the form submission itself and also works with other submission mechanisms, such as pressing Enter in an appropriate form field.

---

### Q13. What is the purpose of `type="submit"`?

It identifies a button as a form submission button.

```jsx
<button type="submit">
  Submit
</button>
```

---

### Q14. Why should a submit button sometimes be disabled during an API request?

To prevent duplicate submissions while the request is in progress.

```jsx
<button disabled={loading}>
  {loading ? "Submitting..." : "Submit"}
</button>
```

---

# 🧠 Final Mental Model

```text
                         EVENTS & FORMS
                                │
                ┌───────────────┼───────────────┐
                ↓               ↓               ↓
             Events          Inputs          Forms
                │               │               │
                ↓               ↓               ↓
           onClick          Controlled       onSubmit
           onChange         Uncontrolled        │
           onKeyDown            │               ↓
           onFocus              │           preventDefault
           onBlur               │               │
                │               │               ↓
                ↓               ↓           Validation
          Event Handler     React State         │
                │               │               ↓
                └───────────────┼─────────── API Request
                                │
                                ↓
                               UI
```

---

# 🔥 One-Line Cheat Sheet

```text
Event Handling
→ Respond to user interactions using React event handlers.

SyntheticEvent
→ React's normalized event interface.

onClick
→ Handles click events.

onChange
→ Handles changes to form controls.

onSubmit
→ Handles form submission.

preventDefault()
→ Prevents the browser's default event behavior.

stopPropagation()
→ Stops event propagation.

Controlled Input
→ React state controls the input value.

Uncontrolled Input
→ DOM controls the input value.

Validation
→ Checks whether input satisfies required rules.

Client Validation
→ Immediate feedback in the browser.

Server Validation
→ Trusted validation on the server.

Form Submission
→ Validate and send form data to the appropriate destination.
```

---

# ⭐ Interview Flow to Remember

```text
USER INTERACTION
       ↓
    DOM EVENT
       ↓
React Event Handler
       ↓
   Event Object
       ↓
  ┌────┴────┐
  ↓         ↓
State     Form
Update    Handling
  │         │
  │       Validate
  │         │
  │      API Request
  │         │
  └────┬────┘
       ↓
   React Render
       ↓
    Updated UI
```

> **Core idea:** React events connect user actions to application logic. For forms, the key decision is whether React or the DOM should own the input value. Controlled inputs provide maximum control and validation, while uncontrolled inputs can be simpler when you only need the value at specific points such as form submission.
