# React A→Z Revision

**Phase 8 — React Router 🧭**

> React Router provides routing capabilities for React applications, allowing different URLs to render different UI while maintaining a client-side application experience.

---

## 📚 Contents

1. [What is React Router?](#1-what-is-react-router)
2. [BrowserRouter](#2-browserrouter)
3. [Routes](#3-routes)
4. [Route](#4-route)
5. [Link](#5-link)
6. [NavLink](#6-navlink)
7. [useNavigate](#7-usenavigate)
8. [useParams](#8-useparams)
9. [useLocation](#9-uselocation)
10. [useSearchParams](#10-usesearchparams)
11. [Nested Routes](#11-nested-routes)
12. [Outlet](#12-outlet)
13. [Protected Routes](#13-protected-routes)
14. [Lazy Loading](#14-lazy-loading)
15. [404 Routes](#15-404-routes)
16. [Complete Router Example](#16-complete-router-example)
17. [Common Mistakes](#17-common-mistakes)
18. [Quick Revision](#18-quick-revision)
19. [Interview Revision](#19-interview-revision)

---

# 1. What is React Router?

## Definition

**React Router** is a routing library for React applications that maps URLs to React UI and provides navigation APIs for client-side applications.

For example:

```text
/              → Home
/about         → About
/products      → Products
/products/101  → Product Details
/login         → Login
```

---

## Why Do We Need Routing?

Without routing:

```text
React App
   ↓
One UI
```

With routing:

```text
                 React App
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
         /         /about    /products
          ↓          ↓          ↓
        Home       About     Products
```

---

## Real-Life Example

Think about a **building with different rooms**.

```text
Building
│
├── Reception
├── Office
├── Meeting Room
└── Storage
```

The URL acts like the room number:

```text
/            → Reception
/about       → Office
/contact     → Meeting Room
```

React Router decides which component should be displayed for the current URL.

---

## Install

For a typical web application:

```bash
npm install react-router-dom
```

---

# 2. BrowserRouter

## Definition

`BrowserRouter` provides routing context to a React application using the browser's History API.

Example:

```jsx
import {
  BrowserRouter
} from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <MyRoutes />
    </BrowserRouter>
  );
}
```

---

## Real-Life Example

Think of `BrowserRouter` as the **building's navigation system**.

```text
BrowserRouter
      ↓
Understands current URL
      ↓
Provides routing context
      ↓
Routes can determine UI
```

---

## Basic Structure

```jsx
import {
  BrowserRouter,
  Routes,
  Route
} from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route
          path="/"
          element={<Home />}
        />

        <Route
          path="/about"
          element={<About />}
        />
      </Routes>
    </BrowserRouter>
  );
}
```

---

## Important

Components such as:

```text
Routes
Link
NavLink
useNavigate
useParams
useLocation
useSearchParams
```

need to be used within the appropriate router context.

Otherwise, React Router will report that the router context is missing.

---

# 3. Routes

## Definition

`Routes` is the container that evaluates the available `Route` definitions and renders the best matching route.

```jsx
<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />
</Routes>
```

---

## Real-Life Example

Think of a **receptionist**.

You say:

```text
"I want to go to the accounts department."
```

The receptionist checks the available departments and directs you to the correct one.

Similarly:

```text
URL
 ↓
Routes
 ↓
Matching Route
 ↓
Component
```

---

# 4. Route

## Definition

`Route` defines the relationship between a URL pattern and the React element to render.

Syntax:

```jsx
<Route
  path="/about"
  element={<About />}
/>
```

---

## Basic Routes

```jsx
<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />

  <Route
    path="/contact"
    element={<Contact />}
  />
</Routes>
```

Mapping:

```text
/          → Home
/about     → About
/contact   → Contact
```

---

## Dynamic Route

You can define dynamic URL segments:

```jsx
<Route
  path="/users/:id"
  element={<User />}
/>
```

URLs:

```text
/users/101
/users/102
/users/500
```

The `:id` part is dynamic.

```text
/users/:id
       ↑
   URL parameter
```

You can read it using `useParams()`.

---

# 5. Link

## Definition

`Link` is used to navigate between routes without relying on a normal full-page browser navigation.

```jsx
import { Link } from "react-router-dom";

<Link to="/about">
  About
</Link>
```

---

## Real-Life Example

Think about an **internal corridor** inside a building.

Instead of leaving the building:

```text
Building
 ↓
Outside
 ↓
Enter again
```

you simply move through an internal corridor:

```text
Room A
  ↓
Corridor
  ↓
Room B
```

`Link` provides client-side navigation between routes.

---

## Example

```jsx
function Navbar() {
  return (
    <nav>
      <Link to="/">
        Home
      </Link>

      <Link to="/about">
        About
      </Link>

      <Link to="/contact">
        Contact
      </Link>
    </nav>
  );
}
```

---

## Link with Dynamic Route

```jsx
<Link to="/users/101">
  View User
</Link>
```

---

## Link vs `<a>`

### React Router

```jsx
<Link to="/about">
  About
</Link>
```

### Normal HTML

```html
<a href="/about">
  About
</a>
```

For internal React Router navigation, `Link` is generally preferred because it uses the router's client-side navigation.

A normal anchor is appropriate when you intentionally want normal browser navigation, such as navigating to an external site.

---

# 6. NavLink

## Definition

`NavLink` is similar to `Link`, but it provides information about whether the link matches the current route, making it useful for navigation menus and active styling.

---

## Example

```jsx
import {
  NavLink
} from "react-router-dom";

<NavLink to="/about">
  About
</NavLink>
```

---

## Active Class

```jsx
<NavLink
  to="/about"
  className={({ isActive }) =>
    isActive
      ? "active"
      : ""
  }
>
  About
</NavLink>
```

When the current URL is:

```text
/about
```

then:

```text
isActive = true
```

---

## Real-Life Example

Think about a **building directory**.

```text
Home       ← You are here
About
Products
Contact
```

The current location is highlighted.

`NavLink` provides this active-route information.

---

## `Link` vs `NavLink`

| Link                           | NavLink                     |
| ------------------------------ | --------------------------- |
| Navigation                     | Navigation                  |
| No built-in active-route state | Provides active-route state |
| Good for normal links          | Good for navigation menus   |

---

# 7. useNavigate

## Definition

`useNavigate` returns a function that allows programmatic navigation.

```jsx
const navigate = useNavigate();
```

Then:

```jsx
navigate("/dashboard");
```

---

## Real-Life Example

A user completes login:

```text
Login successful
      ↓
Navigate to dashboard
```

There may not be a visible link for this action.

So instead of:

```jsx
<Link to="/dashboard">
```

you can navigate programmatically.

---

## Example

```jsx
import {
  useNavigate
} from "react-router-dom";

function Login() {
  const navigate = useNavigate();

  function handleLogin() {
    // Login logic

    navigate("/dashboard");
  }

  return (
    <button onClick={handleLogin}>
      Login
    </button>
  );
}
```

---

## Navigate Back

```jsx
navigate(-1);
```

This goes back one entry in the browser history.

Forward:

```jsx
navigate(1);
```

---

## Replace History Entry

```jsx
navigate("/dashboard", {
  replace: true
});
```

This replaces the current history entry instead of adding a new one.

Useful after flows such as login:

```text
/login
   ↓
replace → /dashboard
```

Then pressing Back won't necessarily return to the login page.

---

## Link vs useNavigate

Use:

```jsx
<Link to="/about">
```

when the user is clicking a navigation link.

Use:

```jsx
navigate("/dashboard");
```

when navigation should happen as part of application logic.

---

# 8. useParams

## Definition

`useParams` reads dynamic parameters from the current URL.

Route:

```jsx
<Route
  path="/users/:id"
  element={<User />}
/>
```

URL:

```text
/users/101
```

Component:

```jsx
import {
  useParams
} from "react-router-dom";

function User() {
  const { id } = useParams();

  return <h1>User ID: {id}</h1>;
}
```

Output:

```text
User ID: 101
```

---

## Real-Life Example

Think about a **product shelf**.

```text
Product Shelf
      ↓
Product ID
      ↓
P-101
```

URL:

```text
/products/P-101
```

The component reads:

```text
id = P-101
```

and can use it to fetch the corresponding product.

---

## API Example

```jsx
function ProductDetails() {
  const { id } = useParams();

  useEffect(() => {
    fetch(`/api/products/${id}`)
      .then(response => response.json())
      .then(data => {
        console.log(data);
      });
  }, [id]);

  return <h1>Product {id}</h1>;
}
```

---

# 9. useLocation

## Definition

`useLocation` returns information about the current URL/location.

```jsx
const location = useLocation();
```

Example:

```jsx
function Page() {
  const location = useLocation();

  console.log(location);
}
```

A location object can include information such as:

```text
pathname
search
hash
state
key
```

---

## Example

If the URL is:

```text
/products?page=2#reviews
```

you can access:

```jsx
location.pathname
```

```text
/products
```

```jsx
location.search
```

```text
?page=2
```

```jsx
location.hash
```

```text
#reviews
```

---

## Real-Life Example

Think about a **GPS system**.

It tells you:

```text
Current location
Destination
Route information
```

`useLocation()` gives your application information about the current URL.

---

## Example: Track Page Changes

```jsx
function PageTracker() {
  const location = useLocation();

  useEffect(() => {
    console.log(
      "Current path:",
      location.pathname
    );
  }, [location]);

  return null;
}
```

---

# 10. useSearchParams

## Definition

`useSearchParams` is used to read and update the query string of the current URL.

Example URL:

```text
/products?category=shoes&page=2
```

Query parameters:

```text
category = shoes
page = 2
```

---

## Basic Example

```jsx
import {
  useSearchParams
} from "react-router-dom";

function Products() {
  const [
    searchParams,
    setSearchParams
  ] = useSearchParams();

  const category =
    searchParams.get("category");

  const page =
    searchParams.get("page");

  return (
    <div>
      <p>Category: {category}</p>
      <p>Page: {page}</p>
    </div>
  );
}
```

---

## Update Search Params

```jsx
setSearchParams({
  category: "laptops",
  page: "2"
});
```

URL becomes:

```text
/products?category=laptops&page=2
```

---

## Real-Life Example

Think about an online shopping filter.

```text
/products
```

User selects:

```text
Category → Shoes
Page → 2
Sort → Price
```

URL:

```text
/products?category=shoes&page=2&sort=price
```

The URL now represents the current filter state.

---

## `useParams` vs `useSearchParams`

### URL Parameter

```text
/users/101
```

```jsx
const { id } = useParams();
```

Use for identifying a resource.

---

### Query Parameter

```text
/users?page=2&sort=name
```

```jsx
const [params] =
  useSearchParams();
```

Use for filters, sorting, pagination, search, etc.

---

# 11. Nested Routes

## Definition

**Nested routes** allow routes to be structured inside other routes.

Example:

```text
/dashboard
/dashboard/profile
/dashboard/settings
/dashboard/orders
```

---

## Real-Life Example

Think about a **dashboard building**:

```text
Dashboard
│
├── Profile
├── Settings
└── Orders
```

The dashboard layout remains visible while the child page changes.

---

## Route Structure

```jsx
<Routes>
  <Route
    path="/dashboard"
    element={<Dashboard />}
  >
    <Route
      path="profile"
      element={<Profile />}
    />

    <Route
      path="settings"
      element={<Settings />}
    />
  </Route>
</Routes>
```

Notice:

```jsx
path="profile"
```

not:

```jsx
path="/dashboard/profile"
```

because it is nested under `/dashboard`.

---

# 12. Outlet

## Definition

`Outlet` is the placeholder where a matching child route renders inside its parent route.

---

## Example

Parent layout:

```jsx
import {
  Outlet
} from "react-router-dom";

function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>

      <nav>
        {/* Navigation */}
      </nav>

      <Outlet />
    </div>
  );
}
```

Routes:

```jsx
<Routes>
  <Route
    path="/dashboard"
    element={<Dashboard />}
  >
    <Route
      path="profile"
      element={<Profile />}
    />

    <Route
      path="settings"
      element={<Settings />}
    />
  </Route>
</Routes>
```

When URL is:

```text
/dashboard/profile
```

React renders:

```text
Dashboard
│
├── Dashboard heading
├── Navigation
│
└── <Outlet />
       ↓
    Profile
```

---

## Real-Life Example

Think about a **picture frame**.

```text
Dashboard Frame
┌─────────────────────────┐
│ Header                  │
│ Navigation              │
│                         │
│    OUTLET AREA          │
│                         │
└─────────────────────────┘
```

The child page is placed inside the outlet.

---

## Nested Route Flow

```text
/dashboard/profile
        ↓
Dashboard Route
        ↓
Dashboard Component
        ↓
<Outlet />
        ↓
Profile Component
```

---

# 13. Protected Routes

## Definition

A **protected route** is a route that only authorized users can access.

Examples:

```text
/dashboard
/admin
/settings
/profile
```

---

## Real-Life Example

Think about an office building.

```text
Office Entrance
      ↓
Check ID
      ↓
Authorized?
   ┌──┴──┐
  Yes    No
   ↓      ↓
Enter   Reject
```

Protected routes work similarly:

```text
Request route
     ↓
Authenticated?
  ┌──┴──┐
 Yes    No
  ↓      ↓
Page   Login
```

---

## Basic Protected Route

```jsx
import {
  Navigate
} from "react-router-dom";

function ProtectedRoute({
  children,
  isAuthenticated
}) {
  if (!isAuthenticated) {
    return (
      <Navigate
        to="/login"
        replace
      />
    );
  }

  return children;
}
```

Use:

```jsx
<Route
  path="/dashboard"
  element={
    <ProtectedRoute
      isAuthenticated={isAuthenticated}
    >
      <Dashboard />
    </ProtectedRoute>
  }
/>
```

---

## Better Pattern with Layout Route

```jsx
function ProtectedLayout() {
  const isAuthenticated =
    useAuth();

  if (!isAuthenticated) {
    return (
      <Navigate
        to="/login"
        replace
      />
    );
  }

  return <Outlet />;
}
```

Routes:

```jsx
<Routes>
  <Route
    element={<ProtectedLayout />}
  >
    <Route
      path="/dashboard"
      element={<Dashboard />}
    />

    <Route
      path="/profile"
      element={<Profile />}
    />
  </Route>

  <Route
    path="/login"
    element={<Login />}
  />
</Routes>
```

---

## Role-Based Route Protection

Suppose:

```text
Roles:
admin
teacher
student
```

You can restrict routes:

```jsx
function AdminRoute() {
  const user = useAuth();

  if (!user) {
    return (
      <Navigate
        to="/login"
        replace
      />
    );
  }

  if (user.role !== "admin") {
    return (
      <Navigate
        to="/unauthorized"
        replace
      />
    );
  }

  return <Outlet />;
}
```

---

## Important Security Note

Client-side protected routes are primarily a **UI/navigation control**.

They are not sufficient security.

A user can still directly call your backend API.

You must enforce authorization on the server:

```text
Frontend protection
→ Better UX

Backend authorization
→ Actual security boundary
```

---

# 14. Lazy Loading

## Definition

**Lazy loading** means loading a component's code only when it is needed instead of including every route's JavaScript in the initial application bundle.

React provides:

```jsx
lazy()
```

and:

```jsx
<Suspense>
```

for component-level code splitting.

---

## Why Lazy Loading?

Imagine your application has:

```text
Home
Dashboard
Admin
Analytics
Settings
Editor
Reports
```

Without code splitting:

```text
Initial Load
 ↓
Download everything
 ↓
Large JavaScript bundle
```

With lazy loading:

```text
Initial Load
 ↓
Load Home
 ↓
User visits Dashboard
 ↓
Load Dashboard code
```

---

## Real-Life Example

Think about a **library**.

You don't carry every book with you.

You request a book when you need it.

```text
Need book?
   ↓
Request book
   ↓
Load book
```

Lazy loading works similarly.

---

## Example

```jsx
import {
  lazy,
  Suspense
} from "react";

const Dashboard = lazy(
  () => import("./Dashboard")
);

const Settings = lazy(
  () => import("./Settings")
);
```

Routes:

```jsx
<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/dashboard"
    element={
      <Suspense fallback={<p>Loading...</p>}>
        <Dashboard />
      </Suspense>
    }
  />

  <Route
    path="/settings"
    element={
      <Suspense fallback={<p>Loading...</p>}>
        <Settings />
      </Suspense>
    }
  />
</Routes>
```

---

## Better Suspense Structure

You can wrap the route tree:

```jsx
<Suspense fallback={<p>Loading...</p>}>
  <Routes>
    <Route
      path="/"
      element={<Home />}
    />

    <Route
      path="/dashboard"
      element={<Dashboard />}
    />

    <Route
      path="/settings"
      element={<Settings />}
    />
  </Routes>
</Suspense>
```

Now any lazy-loaded route beneath it can use the same fallback.

---

## Lazy Loading Flow

```text
User visits /dashboard
        ↓
Router matches route
        ↓
Dashboard chunk requested
        ↓
Suspense fallback
        ↓
Chunk loaded
        ↓
Dashboard rendered
```

---

# 15. 404 Routes

## Definition

A **404 route** handles URLs that don't match any defined route.

React Router uses a wildcard:

```jsx
path="*"
```

---

## Example

```jsx
<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />

  <Route
    path="*"
    element={<NotFound />}
  />
</Routes>
```

---

## Real-Life Example

Think about a building directory.

Someone asks:

```text
"Where is Room 999?"
```

but the building has no such room.

The receptionist responds:

```text
"Room not found."
```

Similarly:

```text
Unknown URL
    ↓
*
    ↓
404 Page
```

---

## 404 Component

```jsx
function NotFound() {
  return (
    <div>
      <h1>404</h1>

      <p>
        Page not found.
      </p>

      <Link to="/">
        Go Home
      </Link>
    </div>
  );
}
```

---

# 16. Complete Router Example

Here is a practical application structure:

```text
App
│
├── Navbar
│
├── Public Routes
│   ├── /
│   ├── /login
│   └── /about
│
├── Protected Routes
│   ├── /dashboard
│   ├── /dashboard/profile
│   └── /dashboard/settings
│
└── 404
```

---

## Routes

```jsx
import {
  BrowserRouter,
  Routes,
  Route,
  Navigate
} from "react-router-dom";

import {
  lazy,
  Suspense
} from "react";

const Dashboard = lazy(
  () => import("./pages/Dashboard")
);

const Profile = lazy(
  () => import("./pages/Profile")
);

function ProtectedLayout() {
  const isAuthenticated = true;

  if (!isAuthenticated) {
    return (
      <Navigate
        to="/login"
        replace
      />
    );
  }

  return <Outlet />;
}

function App() {
  return (
    <BrowserRouter>
      <Navbar />

      <Suspense
        fallback={
          <p>Loading page...</p>
        }
      >
        <Routes>
          {/* Public Routes */}

          <Route
            path="/"
            element={<Home />}
          />

          <Route
            path="/about"
            element={<About />}
          />

          <Route
            path="/login"
            element={<Login />}
          />

          {/* Protected Routes */}

          <Route
            element={<ProtectedLayout />}
          >
            <Route
              path="/dashboard"
              element={<Dashboard />}
            />

            <Route
              path="/dashboard/profile"
              element={<Profile />}
            />
          </Route>

          {/* 404 */}

          <Route
            path="*"
            element={<NotFound />}
          />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

---

# 17. Common Mistakes

## Mistake 1 — Using `<a>` for Internal Navigation

❌

```jsx
<a href="/dashboard">
  Dashboard
</a>
```

For React Router internal navigation, prefer:

```jsx
<Link to="/dashboard">
  Dashboard
</Link>
```

---

## Mistake 2 — Calling `useNavigate` Outside Router Context

❌

```jsx
function App() {
  const navigate = useNavigate();

  return <div />;
}

createRoot(root).render(
  <App />
);
```

If `App` isn't under a router, the Hook has no router context.

Correct:

```jsx
createRoot(root).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

---

## Mistake 3 — Forgetting `<Outlet />`

Nested routes:

```jsx
<Route
  path="/dashboard"
  element={<Dashboard />}
>
  <Route
    path="profile"
    element={<Profile />}
  />
</Route>
```

But `Dashboard` has:

```jsx
function Dashboard() {
  return <h1>Dashboard</h1>;
}
```

`Profile` has nowhere to render.

Add:

```jsx
function Dashboard() {
  return (
    <>
      <h1>Dashboard</h1>
      <Outlet />
    </>
  );
}
```

---

## Mistake 4 — Wrong Nested Path

Inside:

```jsx
<Route path="/dashboard">
```

prefer:

```jsx
<Route path="profile">
```

instead of:

```jsx
<Route path="/dashboard/profile">
```

when defining a relative child route.

---

## Mistake 5 — Using `useParams` Without a Dynamic Route

This:

```jsx
const { id } = useParams();
```

expects a matching route such as:

```jsx
<Route
  path="/users/:id"
  element={<User />}
/>
```

---

## Mistake 6 — Confusing URL Parameters and Query Parameters

Dynamic:

```text
/users/101
```

```jsx
useParams()
```

Query:

```text
/users?page=2
```

```jsx
useSearchParams()
```

---

## Mistake 7 — Treating Client Route Protection as Security

This is not enough:

```jsx
if (!user) {
  return <Navigate to="/login" />;
}
```

The backend must also verify:

```text
Authentication
Authorization
Permissions
Roles
```

---

## Mistake 8 — Forgetting Server Configuration for SPA Deployment

A browser request directly to:

```text
/dashboard
```

may reach your server before React Router does.

Your hosting server needs to serve the application's entry document for client-side routes when appropriate.

Otherwise, you may see:

```text
/dashboard
    ↓
Server
    ↓
404
```

even though the React route exists.

---

# 18. Quick Revision

## BrowserRouter

```jsx
<BrowserRouter>
  <App />
</BrowserRouter>
```

> Provides routing context using browser history.

---

## Routes

```jsx
<Routes>
  ...
</Routes>
```

> Matches the current location against route definitions.

---

## Route

```jsx
<Route
  path="/about"
  element={<About />}
/>
```

> Maps a URL pattern to UI.

---

## Link

```jsx
<Link to="/about">
  About
</Link>
```

> Performs client-side navigation.

---

## NavLink

```jsx
<NavLink to="/about">
  About
</NavLink>
```

> Navigation link that can detect active route state.

---

## useNavigate

```jsx
const navigate = useNavigate();

navigate("/dashboard");
```

> Performs programmatic navigation.

---

## useParams

```jsx
const { id } = useParams();
```

> Reads dynamic URL parameters.

---

## useLocation

```jsx
const location = useLocation();
```

> Reads the current location object.

---

## useSearchParams

```jsx
const [
  params,
  setParams
] = useSearchParams();
```

> Reads and updates URL query parameters.

---

## Nested Routes

```text
/dashboard
/dashboard/profile
/dashboard/settings
```

> Routes can be organized hierarchically.

---

## Outlet

```jsx
<Outlet />
```

> Renders the matching child route inside a parent route.

---

## Protected Routes

```text
Authenticated?
 ┌────┴────┐
Yes        No
 ↓          ↓
Page      Login
```

> Restrict UI access based on authentication/authorization state.

---

## Lazy Loading

```jsx
const Dashboard = lazy(
  () => import("./Dashboard")
);
```

> Loads component code when needed.

---

## 404

```jsx
<Route
  path="*"
  element={<NotFound />}
/>
```

> Handles unmatched routes.

---

# 19. Interview Revision

### Q1. What is React Router?

React Router is a routing library for React applications that maps URL locations to UI and provides client-side navigation APIs.

---

### Q2. What is `BrowserRouter`?

`BrowserRouter` provides router context to a web application using the browser's History API.

---

### Q3. Difference between `Routes` and `Route`?

```text
Routes
→ Container that performs route matching.

Route
→ Defines one route pattern and its UI.
```

---

### Q4. Difference between `Link` and `NavLink`?

```text
Link
→ Navigation link.

NavLink
→ Navigation link + active route information.
```

---

### Q5. When would you use `useNavigate()`?

When navigation needs to happen programmatically as part of application logic.

Examples:

```text
After login
After form submission
After logout
After creating a resource
```

---

### Q6. What is `useParams()`?

It reads dynamic URL parameters.

Route:

```jsx
<Route
  path="/products/:id"
  element={<Product />}
/>
```

URL:

```text
/products/101
```

Then:

```jsx
const { id } = useParams();
```

returns:

```text
101
```

---

### Q7. What is `useLocation()`?

It provides information about the current router location, such as:

```text
pathname
search
hash
state
```

---

### Q8. What is `useSearchParams()`?

It provides an API for reading and updating query parameters.

```text
/products?page=2&sort=price
```

---

### Q9. Difference between `useParams` and `useSearchParams`?

```text
useParams
→ Dynamic route segments

/products/101
         ↑
         id

useSearchParams
→ Query string

/products?page=2
            ↑
           page
```

---

### Q10. What are nested routes?

Nested routes represent hierarchical URL/UI structures.

```text
/dashboard
/dashboard/profile
/dashboard/settings
```

They allow child routes to render inside a parent layout.

---

### Q11. What is `Outlet`?

`Outlet` is the placeholder where the matching child route renders.

```jsx
function Dashboard() {
  return (
    <>
      <Navbar />
      <Outlet />
    </>
  );
}
```

---

### Q12. How do protected routes work?

A route checks authentication or authorization state:

```text
Authenticated?
   ↓
Yes → Render page
No  → Redirect to login
```

But actual authorization must also be enforced by the backend.

---

### Q13. What is lazy loading?

Lazy loading loads component code only when it is needed.

```jsx
const Dashboard = lazy(
  () => import("./Dashboard")
);
```

It can reduce the amount of JavaScript required for the initial page.

---

### Q14. Why do we use `Suspense` with lazy-loaded components?

The lazy component may not be available immediately.

`Suspense` provides fallback UI while the required code is loading.

```jsx
<Suspense fallback={<Loading />}>
  <Dashboard />
</Suspense>
```

---

### Q15. How do you create a 404 route?

Use a wildcard route:

```jsx
<Route
  path="*"
  element={<NotFound />}
/>
```

---

### Q16. What is the difference between `navigate("/home")` and `navigate("/home", { replace: true })`?

Normal navigation adds a new history entry.

```jsx
navigate("/home");
```

`replace: true` replaces the current history entry:

```jsx
navigate("/home", {
  replace: true
});
```

---

# 🧠 Final Mental Model

```text
                         REACT ROUTER
                              │
               ┌──────────────┼──────────────┐
               ↓              ↓              ↓
          BrowserRouter     Routes         Navigation
               │              │              │
               │              ↓              ├── Link
               │            Route            └── NavLink
               │              │
               │              ↓
               │           Element
               │
               ├─────────────────────────────────┐
               ↓                                 ↓
          Dynamic URLs                       Query URLs
               │                                 │
               ↓                                 ↓
          useParams()                    useSearchParams()
               │
               ↓
        Nested Routes
               │
               ↓
            Outlet
               │
       ┌───────┴────────┐
       ↓                ↓
 Protected          Lazy Loading
 Routes             + Suspense
       │
       ↓
 Authentication
       │
       ↓
  Authorized?
   ┌───┴───┐
  Yes      No
   ↓        ↓
 Page      Login
               │
               ↓
          No Match
               │
               ↓
          404 Route
```

---

# 🔥 React Router Decision Guide

```text
Need to define a URL → UI mapping?
        ↓
      Route


Need to group routes?
        ↓
      Routes


Need browser routing context?
        ↓
  BrowserRouter


Need normal internal navigation?
        ↓
      Link


Need active navigation styling?
        ↓
     NavLink


Need navigation from code?
        ↓
   useNavigate


Need /users/:id?
        ↓
    useParams


Need current URL information?
        ↓
   useLocation


Need ?page=2&sort=name?
        ↓
 useSearchParams


Need /dashboard/profile?
        ↓
   Nested Routes


Need child UI inside a layout?
        ↓
     Outlet


Need authentication-based access?
        ↓
 Protected Route


Need route-level code splitting?
        ↓
 lazy() + Suspense


Need unknown URL handling?
        ↓
    path="*"
```

---

# ⭐ One-Line Cheat Sheet

```text
BrowserRouter
→ Provides browser-based routing context.

Routes
→ Matches the current location against route definitions.

Route
→ Maps a URL pattern to React UI.

Link
→ Performs client-side navigation.

NavLink
→ Link with active-route state.

useNavigate
→ Navigate programmatically.

useParams
→ Read dynamic URL parameters.

useLocation
→ Read the current URL/location object.

useSearchParams
→ Read and update query parameters.

Nested Routes
→ Create hierarchical routes and layouts.

Outlet
→ Render the matched child route inside its parent.

Protected Routes
→ Restrict UI routes based on authentication/authorization.

lazy()
→ Load component code on demand.

Suspense
→ Display fallback UI while lazy content is loading.

404 Route
→ Handle unmatched URLs.
```

> **Core idea:** React Router connects **URL → Route → Component → UI**. For interviews, focus especially on the difference between **Link vs useNavigate**, **useParams vs useSearchParams**, **nested routes + Outlet**, **protected routes**, and **re-rendering vs navigation**.
