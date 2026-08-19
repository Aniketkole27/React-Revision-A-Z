# React A→Z Revision

**Phase 12 — React Architecture ⭐**

> A practical revision guide for designing scalable React applications, organizing files, managing state, separating API logic, building reusable components, and implementing authentication.

---

# Contents

1. [React Architecture Overview](#1-react-architecture-overview)
2. [Component Architecture](#2-component-architecture)
3. [Folder Structure](#3-folder-structure)
4. [Reusable Components](#4-reusable-components)
5. [Feature-Based Architecture](#5-feature-based-architecture)
6. [State Architecture](#6-state-architecture)
7. [API Layer](#7-api-layer)
8. [Authentication](#8-authentication)
9. [Putting Everything Together](#9-putting-everything-together)
10. [Small vs Large React Application](#10-small-vs-large-react-application)
11. [Architecture Principles](#11-architecture-principles)
12. [Quick Revision](#12-quick-revision)
13. [Interview Questions](#13-interview-questions)
14. [Final Architecture Map](#14-final-architecture-map)

---

# 1. React Architecture Overview

## Definition

**React architecture** is the way a React application is structured, organized, and designed so that its components, state, API communication, authentication, and business logic remain maintainable and scalable.

A good architecture should make it easy to:

```text
Add features
Fix bugs
Reuse components
Manage state
Test code
Scale the application
Onboard developers
```

---

## Real-Life Example

Think about a large company.

A company does not put everything in one room:

```text
Company
├── HR
├── Finance
├── Engineering
├── Marketing
└── Sales
```

Each department has its own responsibility.

A React application should follow a similar principle:

```text
Application
├── Components
├── Features
├── API
├── State
├── Authentication
└── Utilities
```

---

## Poor Architecture

```text
src/
└── App.jsx
```

Everything is inside one file:

```text
UI
API
Authentication
State
Business Logic
Forms
Utilities
```

This becomes difficult to maintain.

---

## Better Architecture

```text
src/
├── components/
├── features/
├── hooks/
├── services/
├── store/
├── pages/
├── routes/
├── utils/
└── App.jsx
```

Each area has a clear responsibility.

---

# 2. Component Architecture

## Definition

**Component architecture** is the practice of breaking an application UI into smaller, reusable, and independent components.

---

## Real-Life Example

A car is made from smaller systems:

```text
Car
├── Engine
├── Wheels
├── Brakes
├── Steering
└── Dashboard
```

Similarly:

```text
Dashboard
├── Navbar
├── Sidebar
├── Statistics
├── TaskList
└── UserProfile
```

---

## Component Hierarchy

Example:

```text
App
│
├── Navbar
│   ├── Logo
│   └── UserMenu
│
├── Sidebar
│   ├── NavigationItem
│   └── NavigationItem
│
└── Dashboard
    ├── StatisticsCard
    ├── TaskList
    │   └── TaskCard
    └── ActivityList
```

---

## Good Component

A component should ideally have a clear responsibility.

```jsx
function UserCard({ user }) {
  return (
    <article>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </article>
  );
}
```

Its responsibility is:

```text
Display user information
```

---

## Avoid Giant Components

Avoid:

```jsx
function Dashboard() {
  // 1000+ lines
  // API calls
  // forms
  // authentication
  // filtering
  // modals
  // tables
  // business logic
}
```

Instead:

```text
Dashboard
├── DashboardHeader
├── Statistics
├── TaskTable
├── Filters
└── TaskModal
```

---

## Container vs Presentational Concept

A common architectural idea is separating:

### Container

Handles logic:

```text
Fetching
State
Business logic
```

### Presentational Component

Handles UI:

```text
Layout
Rendering
Styles
```

Example:

```jsx
function UserContainer() {
  const users = useUsers();

  return <UserList users={users} />;
}
```

```jsx
function UserList({ users }) {
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

Modern React does not require strict container/presentational separation, but the principle of separating responsibilities remains useful.

---

# 3. Folder Structure

## Definition

A **folder structure** organizes application files according to their responsibilities or features.

---

# Basic Structure

For a small application:

```text
src/
├── components/
├── pages/
├── hooks/
├── services/
├── utils/
├── assets/
├── App.jsx
└── main.jsx
```

---

## components/

Contains reusable UI components.

```text
components/
├── Button/
│   ├── Button.jsx
│   └── Button.css
├── Modal/
│   ├── Modal.jsx
│   └── Modal.css
└── Navbar/
    └── Navbar.jsx
```

---

## pages/

Contains route-level components.

```text
pages/
├── Home/
│   └── Home.jsx
├── Login/
│   └── Login.jsx
└── Dashboard/
    └── Dashboard.jsx
```

---

## hooks/

Contains reusable Custom Hooks.

```text
hooks/
├── useAuth.js
├── useFetch.js
├── useDebounce.js
└── useLocalStorage.js
```

---

## services/

Contains API-related logic.

```text
services/
├── api.js
├── authService.js
├── userService.js
└── taskService.js
```

---

## utils/

Contains reusable helper functions.

```text
utils/
├── formatDate.js
├── validation.js
└── constants.js
```

---

# Feature-Based Structure

For larger applications, a feature-based architecture is often more scalable.

```text
src/
├── app/
│   ├── App.jsx
│   ├── routes.jsx
│   └── providers.jsx
│
├── features/
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── authSlice.js
│   │   └── pages/
│   │
│   ├── users/
│   │   ├── components/
│   │   ├── services/
│   │   ├── hooks/
│   │   └── pages/
│   │
│   └── tasks/
│       ├── components/
│       ├── services/
│       ├── hooks/
│       └── pages/
│
├── components/
├── hooks/
├── services/
├── utils/
└── main.jsx
```

---

# 4. Reusable Components

## Definition

A **reusable component** is a component designed to work in multiple places without duplicating its implementation.

---

## Real-Life Example

A company uses the same type of ID card for:

```text
Employee
Student
Visitor
```

Instead of creating three completely different systems, you create one reusable template.

---

## Bad Example

```jsx
<button className="primary-button">
  Login
</button>
```

Then again:

```jsx
<button className="primary-button">
  Register
</button>
```

And again:

```jsx
<button className="primary-button">
  Submit
</button>
```

The styling and behavior may become duplicated.

---

## Reusable Button

```jsx
function Button({
  children,
  onClick,
  variant = "primary",
  disabled = false
}) {
  return (
    <button
      className={`button ${variant}`}
      onClick={onClick}
      disabled={disabled}
    >
      {children}
    </button>
  );
}
```

Usage:

```jsx
<Button onClick={handleLogin}>
  Login
</Button>

<Button onClick={handleRegister}>
  Register
</Button>

<Button variant="danger">
  Delete
</Button>
```

---

## Good Reusable Component Principles

A reusable component should:

```text
Have a clear responsibility
Accept configurable props
Avoid unnecessary application-specific logic
Have predictable behavior
Be easy to compose
```

---

## Avoid Over-Abstraction

Not everything needs to become a reusable component.

Bad abstraction:

```text
UniversalTextContainerWithConfigurableMode
```

when the UI is used once.

The goal is:

> **Reuse where it provides real value, not reuse for the sake of reuse.**

---

# 5. Feature-Based Architecture

## Definition

**Feature-based architecture** organizes code according to business features instead of technical categories.

---

## Traditional Structure

```text
components/
hooks/
services/
utils/
pages/
```

Suppose you have:

```text
auth
users
tasks
payments
```

Their files are spread across many folders.

---

## Feature-Based Structure

```text
features/
├── auth/
├── users/
├── tasks/
└── payments/
```

Each feature contains its own related code.

---

## Example

```text
features/
└── tasks/
    ├── components/
    │   ├── TaskCard.jsx
    │   ├── TaskForm.jsx
    │   └── TaskList.jsx
    │
    ├── hooks/
    │   └── useTasks.js
    │
    ├── services/
    │   └── taskService.js
    │
    ├── taskSlice.js
    │
    └── pages/
        └── TasksPage.jsx
```

Everything related to Tasks stays together.

---

## Real-Life Example

Instead of organizing a company like:

```text
All Managers
All Developers
All Designers
```

you organize teams by product:

```text
Product A
├── Developer
├── Designer
└── Manager

Product B
├── Developer
├── Designer
└── Manager
```

This keeps feature ownership clear.

---

## Advantages

```text
Easy to locate code
Better scalability
Clear ownership
Less coupling
Easier maintenance
Feature can evolve independently
```

---

# 6. State Architecture

## Definition

**State architecture** means deciding where application state should live, who owns it, and which state management technique should be used.

---

## Not All State Is the Same

A common mistake is putting everything into Redux or Context.

Instead, classify state.

---

# 1. Local UI State

Used by one component.

Examples:

```text
Modal open/close
Input value
Dropdown state
Active tab
```

Use:

```jsx
useState()
```

Example:

```jsx
const [isOpen, setIsOpen] = useState(false);
```

---

# 2. Shared State

Used by multiple components.

Examples:

```text
Theme
Authenticated user
Shopping cart
Global UI state
```

Possible solutions:

```text
Context
Redux Toolkit
Zustand
Other state libraries
```

---

# 3. Server State

Data coming from the backend.

Examples:

```text
Users
Products
Tasks
Orders
Posts
```

Server state has additional concerns:

```text
Caching
Fetching
Refetching
Synchronization
Pagination
Error handling
```

Tools such as TanStack Query can be useful here.

---

# 4. URL State

State represented by the URL.

Examples:

```text
?page=2
?search=react
/products/123
```

Usually managed through the router.

---

## State Decision Tree

```text
Who needs this state?
        │
        ├── One component
        │       ↓
        │    useState
        │
        ├── Nearby components
        │       ↓
        │   Lift state up
        │
        ├── Many components
        │       ↓
        │ Context / State Library
        │
        ├── Backend data
        │       ↓
        │ Server-State Library
        │
        └── URL-related
                ↓
             Router
```

---

## Example

For an e-commerce application:

```text
Cart
 ↓
Global State

Product List
 ↓
Server State

Search Query
 ↓
URL State

Modal
 ↓
Local State
```

---

# 7. API Layer

## Definition

The **API layer** is a dedicated part of the application responsible for communicating with backend services.

Instead of making API requests directly inside every component, centralize API communication.

---

## Bad Architecture

```jsx
function Users() {
  useEffect(() => {
    fetch("http://localhost:8000/api/users");
  }, []);
}
```

Then:

```jsx
function AdminUsers() {
  useEffect(() => {
    fetch("http://localhost:8000/api/users");
  }, []);
}
```

And:

```jsx
function UserDropdown() {
  useEffect(() => {
    fetch("http://localhost:8000/api/users");
  }, []);
}
```

API logic becomes duplicated.

---

# Better Architecture

Create:

```text
services/
└── userService.js
```

```js
import api from "./api";

export async function getUsers() {
  const response = await api.get("/users");

  return response.data;
}
```

Component:

```jsx
import { getUsers } from "../services/userService";

useEffect(() => {
  async function loadUsers() {
    const users = await getUsers();

    setUsers(users);
  }

  loadUsers();
}, []);
```

---

## Axios API Instance

```js
import axios from "axios";

const api = axios.create({
  baseURL: "http://localhost:8000/api",
  withCredentials: true
});

export default api;
```

Now:

```js
api.get("/users");
api.post("/users");
api.put("/users/123");
api.delete("/users/123");
```

---

## API Layer Structure

```text
services/
├── api.js
├── authService.js
├── userService.js
├── taskService.js
└── paymentService.js
```

Example:

```js
// taskService.js

import api from "./api";

export const getTasks = () =>
  api.get("/tasks");

export const createTask = task =>
  api.post("/tasks", task);

export const deleteTask = id =>
  api.delete(`/tasks/${id}`);
```

---

## Why API Layer?

It provides:

```text
Centralized API logic
Reusable requests
Cleaner components
Easier testing
Easy API URL changes
Consistent error handling
Authentication integration
```

---

# 8. Authentication

## Definition

**Authentication** verifies **who the user is**.

Example:

```text
Email
Password
     ↓
Backend
     ↓
Valid?
     ↓
Authenticated User
```

---

## Authentication vs Authorization

Very important interview concept.

### Authentication

> "Who are you?"

```text
Login
Identity
Session
Token
```

### Authorization

> "What are you allowed to do?"

```text
Admin → Delete users
Teacher → Create sessions
Student → Take tests
```

---

# Authentication Flow

A typical token-based architecture:

```text
User
 ↓
Login Form
 ↓
POST /login
 ↓
Backend validates credentials
 ↓
Session / Token established
 ↓
Authenticated requests
 ↓
Protected resources
```

---

## React Authentication Context

A simple example:

```jsx
import {
  createContext,
  useContext,
  useState
} from "react";

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = userData => {
    setUser(userData);
  };

  const logout = () => {
    setUser(null);
  };

  return (
    <AuthContext.Provider
      value={{
        user,
        login,
        logout
      }}
    >
      {children}
    </AuthContext.Provider>
  );
}
```

Custom hook:

```jsx
export function useAuth() {
  return useContext(AuthContext);
}
```

Usage:

```jsx
function Navbar() {
  const { user, logout } = useAuth();

  return (
    <nav>
      {user ? (
        <>
          <span>{user.name}</span>

          <button onClick={logout}>
            Logout
          </button>
        </>
      ) : (
        <a href="/login">Login</a>
      )}
    </nav>
  );
}
```

---

# Protected Routes

Authentication state can be used to protect routes.

```jsx
function ProtectedRoute({ children }) {
  const { user } = useAuth();

  if (!user) {
    return <Navigate to="/login" replace />;
  }

  return children;
}
```

Usage:

```jsx
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  }
/>
```

---

# Role-Based Authorization

Authentication:

```text
User is logged in
```

Authorization:

```text
User role = admin
```

Example:

```jsx
function AdminRoute({ children }) {
  const { user } = useAuth();

  if (!user) {
    return <Navigate to="/login" replace />;
  }

  if (user.role !== "admin") {
    return <Navigate to="/unauthorized" replace />;
  }

  return children;
}
```

---

# Authentication Architecture

A typical structure:

```text
features/
└── auth/
    ├── components/
    │   ├── LoginForm.jsx
    │   └── RegisterForm.jsx
    │
    ├── hooks/
    │   └── useAuth.js
    │
    ├── services/
    │   └── authService.js
    │
    └── AuthProvider.jsx
```

---

# Token-Based Authentication

A common architecture uses access tokens.

```text
Login
 ↓
Backend
 ↓
Access Token
 ↓
Authenticated Request
 ↓
Authorization
```

Example request:

```http
Authorization: Bearer <token>
```

However, exact token storage and refresh strategy depends on the application's security requirements.

For browser applications, authentication should be designed carefully to reduce risks such as XSS and CSRF.

---

# 9. Putting Everything Together

Consider a **Task Management Application**.

Requirements:

```text
Login
Dashboard
Projects
Tasks
Users
Admin
```

A scalable structure could be:

```text
src/
│
├── app/
│   ├── App.jsx
│   ├── routes.jsx
│   └── providers.jsx
│
├── components/
│   ├── Button/
│   ├── Modal/
│   ├── Input/
│   └── Spinner/
│
├── features/
│   │
│   ├── auth/
│   │   ├── components/
│   │   │   ├── LoginForm.jsx
│   │   │   └── RegisterForm.jsx
│   │   │
│   │   ├── hooks/
│   │   │   └── useAuth.js
│   │   │
│   │   ├── services/
│   │   │   └── authService.js
│   │   │
│   │   └── AuthProvider.jsx
│   │
│   ├── tasks/
│   │   ├── components/
│   │   │   ├── TaskCard.jsx
│   │   │   ├── TaskForm.jsx
│   │   │   └── TaskList.jsx
│   │   │
│   │   ├── hooks/
│   │   │   └── useTasks.js
│   │   │
│   │   ├── services/
│   │   │   └── taskService.js
│   │   │
│   │   └── pages/
│   │       └── TasksPage.jsx
│   │
│   ├── projects/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   └── pages/
│   │
│   └── users/
│       ├── components/
│       ├── hooks/
│       ├── services/
│       └── pages/
│
├── hooks/
│   ├── useDebounce.js
│   └── useLocalStorage.js
│
├── services/
│   └── api.js
│
├── utils/
│   ├── formatDate.js
│   └── validation.js
│
└── main.jsx
```

---

# Request Flow

When the user loads Tasks:

```text
TasksPage
    ↓
useTasks()
    ↓
taskService.getTasks()
    ↓
api.get("/tasks")
    ↓
Backend
    ↓
Database
```

Response:

```text
Database
    ↓
Backend
    ↓
API
    ↓
taskService
    ↓
useTasks()
    ↓
TasksPage
    ↓
TaskList
    ↓
TaskCard
```

This creates clear separation between:

```text
UI
Logic
API
State
Database
```

---

# Authentication Flow

```text
LoginForm
    ↓
authService.login()
    ↓
API
    ↓
Backend
    ↓
Authentication
    ↓
Session / Token
    ↓
AuthProvider
    ↓
Application
```

Protected page:

```text
User
 ↓
Dashboard
 ↓
Auth Check
 ↓
Authenticated?
 ├── Yes → Dashboard
 └── No  → Login
```

---

# 10. Small vs Large React Application

## Small Application

You don't need a complex architecture.

```text
src/
├── components/
├── pages/
├── hooks/
├── services/
├── utils/
└── App.jsx
```

Simple is better.

---

## Medium Application

```text
src/
├── components/
├── features/
├── hooks/
├── services/
├── store/
├── pages/
└── utils/
```

Feature-based organization becomes useful.

---

## Large Application

```text
src/
├── app/
├── features/
├── components/
├── hooks/
├── services/
├── store/
├── routes/
├── utils/
└── types/
```

You may also introduce:

```text
Testing
Design system
Feature boundaries
Error handling
Observability
Performance monitoring
```

---

# 11. Architecture Principles

## 1. Single Responsibility

A component should have a clear purpose.

Bad:

```text
Dashboard
├── API
├── Authentication
├── Form
├── Table
├── Modal
└── Business Logic
```

Better:

```text
Dashboard
├── DashboardHeader
├── Statistics
├── TaskTable
└── TaskModal
```

---

## 2. Separation of Concerns

Separate:

```text
UI
Logic
API
State
Authentication
Utilities
```

---

## 3. Don't Duplicate Logic

If the same logic appears in:

```text
Component A
Component B
Component C
```

consider extracting:

```text
Custom Hook
Utility
Service
Reusable Component
```

---

## 4. Keep Components Focused

Prefer:

```jsx
<UserList />
```

over:

```jsx
<UserListWithAuthenticationAndFetchingAndSortingAndFilteringAndPagination />
```

---

## 5. Don't Over-Engineer

Avoid adding:

```text
Redux
Context
Custom Hook
HOC
Service
Repository
Adapter
Factory
```

just because they exist.

Use architecture according to actual application complexity.

---

## 6. Keep Dependencies Flowing in One Direction

A useful conceptual flow:

```text
UI
 ↓
Hooks / State
 ↓
Services
 ↓
API
 ↓
Backend
```

Avoid unnecessary circular dependencies.

---

# Architecture Golden Rules

```text
1. Keep components small.
2. Keep business logic reusable.
3. Centralize API communication.
4. Keep state close to where it is used.
5. Separate server state from client state.
6. Organize large applications by features.
7. Avoid unnecessary global state.
8. Reuse components when reuse is meaningful.
9. Keep authentication logic centralized.
10. Prefer simple architecture until complexity requires more.
```

---

# 12. Quick Revision

| Concept                    | One-Line Definition                                     |
| -------------------------- | ------------------------------------------------------- |
| Component Architecture     | Organizing UI into focused reusable components          |
| Folder Structure           | Organizing files according to responsibilities/features |
| Reusable Components        | Components designed to work in multiple places          |
| Feature-Based Architecture | Organizing code around business features                |
| State Architecture         | Deciding where state should live and how it is managed  |
| API Layer                  | Centralized system for backend communication            |
| Authentication             | Verifying the identity of a user                        |
| Authorization              | Determining what an authenticated user can access       |

---

# State Classification

```text
                  APPLICATION STATE
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
   Local State      Shared State     Server State
        │                │                │
     useState       Context/Store    Query/Cache
        │
        ↓
     UI State
```

And:

```text
URL State
   ↓
React Router
```

---

# 13. Interview Questions

## Basic

1. What is React architecture?
2. What makes a good React component?
3. What is component composition?
4. How should you structure a React project?
5. What is feature-based architecture?
6. What is a reusable component?
7. What is an API layer?
8. What is authentication?
9. What is authorization?
10. Authentication vs authorization?

---

## Intermediate

11. Folder-based vs feature-based architecture?
12. Where should API calls live?
13. Where should authentication logic live?
14. When should state be local?
15. When should state be global?
16. What is server state?
17. Why shouldn't every state be stored in Redux?
18. How would you structure a large React application?
19. How would you prevent components from becoming too large?
20. How would you share business logic between components?
21. How would you organize API services?
22. How would you implement protected routes?

---

## Advanced

23. Design the architecture of a large e-commerce React application.
24. How would you structure a multi-role application with Admin, User, and Manager?
25. How would you separate server state from client state?
26. How would you design an authentication architecture?
27. How would you handle access-token expiration and refresh?
28. How would you prevent API logic from leaking into UI components?
29. How would you decide between Context and Redux?
30. How would you design a scalable feature-based architecture?
31. How would you organize a React application with 50+ features?
32. How would you migrate a monolithic React component into a scalable architecture?
33. What are the signs that a React project is becoming over-engineered?
34. How would you design architecture for a frontend team with multiple developers?

---

# 14. Final Architecture Map

```text
                         REACT APPLICATION
                                │
                 ┌──────────────┼──────────────┐
                 ↓              ↓              ↓
              APP/UI         FEATURES       SHARED
                 │              │              │
                 │         ┌────┼────┐         │
                 │         ↓    ↓    ↓         │
                 │       Auth Tasks Users       │
                 │                             │
                 ↓                             ↓
            Components                    Components
                 │
                 ↓
              Hooks
                 │
        ┌────────┴────────┐
        ↓                 ↓
     Client State      Server State
        │                 │
        ↓                 ↓
   Context/Store     Query/Cache
        │                 │
        └────────┬────────┘
                 ↓
             API Layer
                 ↓
              Backend
                 ↓
             Database
```

---

# Complete Mental Model

When designing a React application, ask these questions in order:

```text
1. What are my features?
        ↓
2. What components does each feature need?
        ↓
3. Which components should be reusable?
        ↓
4. Where should each piece of state live?
        ↓
5. Which data comes from the server?
        ↓
6. Where should API communication happen?
        ↓
7. How will authentication work?
        ↓
8. Which routes require authorization?
        ↓
9. How will the application scale?
        ↓
10. Can another developer easily understand this structure?
```

> **The goal of React architecture is not to create the most complicated folder structure. The goal is to create clear boundaries between UI, reusable components, state, business logic, API communication, and authentication so the application can grow without becoming difficult to maintain.**
