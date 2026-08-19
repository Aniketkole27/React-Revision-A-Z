# React A→Z Revision

**Phase 9 — API & Async ⭐**

> Complete revision notes for handling APIs, asynchronous operations, data fetching, caching, pagination, infinite scrolling, debouncing, polling, and request cancellation in React.

---

## Contents

1. [API & Async Overview](#1-api--async-overview)
2. [fetch](#2-fetch)
3. [Axios](#3-axios)
4. [Loading State](#4-loading-state)
5. [Error State](#5-error-state)
6. [Caching](#6-caching)
7. [Pagination](#7-pagination)
8. [Infinite Scroll](#8-infinite-scroll)
9. [Debouncing](#9-debouncing)
10. [Polling](#10-polling)
11. [Request Cancellation](#11-request-cancellation)
12. [Complete API Pattern](#12-complete-api-pattern)
13. [Quick Revision](#13-quick-revision)
14. [Interview Questions](#14-interview-questions)

---

# 1. API & Async Overview

## Definition

**API (Application Programming Interface)** allows the React frontend to communicate with a backend server and exchange data.

**Async operations** are operations that take time to complete, such as:

* Fetching data from a server
* Sending form data
* Uploading files
* Authentication
* Database operations

### Real-Life Example

Think of a restaurant:

```text
You → Waiter → Kitchen → Waiter → You
```

In an application:

```text
React → API Request → Backend → Database
React ← API Response ← Backend ← Database
```

React does not directly access the database.

```text
React
  ↓
API
  ↓
Backend
  ↓
Database
```

---

## Basic API Flow

```js
async function getUsers() {
  const response = await fetch("/api/users");
  const data = await response.json();

  console.log(data);
}
```

### Important

API requests are asynchronous because the response does not arrive immediately.

---

# 2. fetch

## Definition

`fetch()` is a built-in JavaScript API used to make HTTP requests.

It returns a **Promise**.

```js
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

---

## Real-Life Example

Imagine ordering something online.

```text
Place Order
    ↓
Wait
    ↓
Server processes order
    ↓
Response arrives
```

`fetch()` represents the process of placing the request and waiting for the response.

---

## GET Request

```js
async function getUsers() {
  const response = await fetch("/api/users");

  const data = await response.json();

  console.log(data);
}
```

---

## POST Request

```js
async function createUser() {
  const response = await fetch("/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      name: "Aniket",
      email: "aniket@example.com"
    })
  });

  const data = await response.json();

  console.log(data);
}
```

---

## Fetch in React

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function getUsers() {
      const response = await fetch("/api/users");
      const data = await response.json();

      setUsers(data);
    }

    getUsers();
  }, []);

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## Important Fetch Point

`fetch()` does **not automatically reject the Promise for HTTP errors such as 404 or 500**.

Therefore:

```js
const response = await fetch("/api/users");

if (!response.ok) {
  throw new Error("Request failed");
}
```

Better:

```js
async function getUsers() {
  const response = await fetch("/api/users");

  if (!response.ok) {
    throw new Error("Failed to fetch users");
  }

  return response.json();
}
```

---

# 3. Axios

## Definition

**Axios** is a third-party HTTP client used to make API requests.

Install:

```bash
npm install axios
```

---

## fetch vs Axios

| Feature               | fetch           | Axios               |
| --------------------- | --------------- | ------------------- |
| Built-in              | Yes             | No                  |
| JSON parsing          | Manual          | Automatic           |
| HTTP error handling   | Manual          | Automatic rejection |
| Request cancellation  | AbortController | AbortController     |
| Interceptors          | No              | Yes                 |
| Request configuration | Basic           | Rich                |
| Instances             | Manual          | Built-in            |

---

## GET Request

```js
import axios from "axios";

async function getUsers() {
  const response = await axios.get("/api/users");

  console.log(response.data);
}
```

---

## POST Request

```js
await axios.post("/api/users", {
  name: "Aniket",
  email: "aniket@example.com"
});
```

---

## Axios Instance

Useful when every request uses the same base URL.

```js
import axios from "axios";

const api = axios.create({
  baseURL: "http://localhost:8000/api",
  withCredentials: true
});

export default api;
```

Then:

```js
const response = await api.get("/users");
```

Instead of:

```js
axios.get("http://localhost:8000/api/users");
```

---

## Axios Interceptor

Interceptors allow you to modify requests or responses globally.

```js
api.interceptors.request.use(config => {
  console.log("Request sent");

  return config;
});
```

Common use cases:

* Authentication tokens
* Logging
* Global error handling
* Refresh tokens

---

# 4. Loading State

## Definition

A **loading state** represents the period when an API request is currently running.

---

## Real-Life Example

When ordering food:

```text
Order placed
    ↓
Preparing...
    ↓
Delivered
```

Similarly:

```text
API request
    ↓
Loading...
    ↓
Data received
```

---

## React Example

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchUsers() {
      try {
        const response = await fetch("/api/users");
        const data = await response.json();

        setUsers(data);
      } finally {
        setLoading(false);
      }
    }

    fetchUsers();
  }, []);

  if (loading) {
    return <p>Loading users...</p>;
  }

  return (
    <div>
      {users.map(user => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

---

## Loading State Pattern

```js
const [loading, setLoading] = useState(false);

setLoading(true);

try {
  // API request
} finally {
  setLoading(false);
}
```

### Why `finally`?

Because loading should stop whether the request succeeds or fails.

---

# 5. Error State

## Definition

An **error state** stores information about a failed API operation.

---

## Real-Life Example

Online payment:

```text
Payment processing
       ↓
   Failed ❌
       ↓
"Payment failed. Try again."
```

Similarly:

```text
API request
    ↓
Error
    ↓
Display error message
```

---

## React Example

```jsx
function Users() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchUsers() {
      try {
        setError(null);

        const response = await fetch("/api/users");

        if (!response.ok) {
          throw new Error("Failed to fetch users");
        }

        const data = await response.json();

        setUsers(data);
      } catch (error) {
        setError(error.message);
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
    return <p>Error: {error}</p>;
  }

  return (
    <div>
      {users.map(user => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

---

## Standard API State

A common pattern is:

```js
const [data, setData] = useState(null);
const [loading, setLoading] = useState(false);
const [error, setError] = useState(null);
```

Think:

```text
data
loading
error
```

These three states cover most basic API scenarios.

---

# 6. Caching

## Definition

**Caching** means storing previously fetched data so that the application can reuse it instead of making the same API request repeatedly.

---

## Real-Life Example

You frequently visit the same website.

Instead of downloading everything again every time, your browser can reuse cached resources.

Similarly:

```text
First request
React → Server → Data
             ↓
           Cache

Second request
React → Cache → Data
```

---

## Simple React Cache

```jsx
const cache = new Map();

async function getUsers() {
  if (cache.has("users")) {
    return cache.get("users");
  }

  const response = await fetch("/api/users");
  const data = await response.json();

  cache.set("users", data);

  return data;
}
```

---

## Why Cache?

Without caching:

```text
Search "React"
→ API request

Search "JavaScript"
→ API request

Search "React"
→ API request again
```

With caching:

```text
Search "React"
→ API request
→ Store result

Search "JavaScript"
→ API request

Search "React"
→ Use cached result
```

---

## Cache with Expiration

A simple cache can store a timestamp.

```js
const cache = new Map();

async function getUsers() {
  const cached = cache.get("users");

  if (cached && Date.now() - cached.timestamp < 60_000) {
    return cached.data;
  }

  const response = await fetch("/api/users");
  const data = await response.json();

  cache.set("users", {
    data,
    timestamp: Date.now()
  });

  return data;
}
```

Here the cache is valid for:

```text
60,000 ms = 60 seconds
```

---

## Production Caching

For larger React applications, libraries such as:

* TanStack Query
* SWR
* RTK Query

provide advanced caching features.

They can handle:

```text
Caching
Refetching
Retries
Deduplication
Loading states
Error states
Pagination
Infinite queries
```

---

# 7. Pagination

## Definition

**Pagination** divides a large dataset into smaller pages instead of loading everything at once.

---

## Real-Life Example

Imagine an online shopping website with:

```text
10,000 products
```

Instead of loading all 10,000:

```text
Page 1 → Products 1–20
Page 2 → Products 21–40
Page 3 → Products 41–60
```

---

## Common API

```text
GET /users?page=1&limit=10
```

Response:

```json
{
  "users": [],
  "page": 1,
  "totalPages": 10
}
```

---

## React Pagination Example

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);
  const [page, setPage] = useState(1);

  useEffect(() => {
    async function fetchUsers() {
      const response = await fetch(
        `/api/users?page=${page}&limit=10`
      );

      const data = await response.json();

      setUsers(data.users);
    }

    fetchUsers();
  }, [page]);

  return (
    <>
      {users.map(user => (
        <p key={user.id}>{user.name}</p>
      ))}

      <button
        disabled={page === 1}
        onClick={() => setPage(page - 1)}
      >
        Previous
      </button>

      <span> Page {page} </span>

      <button onClick={() => setPage(page + 1)}>
        Next
      </button>
    </>
  );
}
```

---

## Types of Pagination

### Offset-Based

```text
?page=2&limit=10
```

or:

```text
?offset=10&limit=10
```

Easy to implement but can become inefficient for very large datasets.

### Cursor-Based

```text
?cursor=abc123&limit=10
```

The server returns a cursor for the next set of records.

Better for:

* Large datasets
* Feeds
* Infinite scrolling
* Frequently changing data

---

# 8. Infinite Scroll

## Definition

**Infinite scrolling** automatically loads more data when the user reaches near the bottom of the page.

---

## Real-Life Example

Social media feeds:

```text
Post
Post
Post
Post
↓
More posts automatically load
↓
Post
Post
Post
```

---

## Basic Flow

```text
Load first page
      ↓
User scrolls
      ↓
Near bottom?
      ↓
Yes
      ↓
Fetch next page
      ↓
Append new data
```

---

## Using IntersectionObserver

`IntersectionObserver` detects when an element becomes visible in the viewport.

```jsx
import { useEffect, useRef, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);
  const [page, setPage] = useState(1);
  const loaderRef = useRef(null);

  useEffect(() => {
    async function fetchUsers() {
      const response = await fetch(
        `/api/users?page=${page}&limit=10`
      );

      const data = await response.json();

      setUsers(prev => [...prev, ...data.users]);
    }

    fetchUsers();
  }, [page]);

  useEffect(() => {
    const observer = new IntersectionObserver(entries => {
      if (entries[0].isIntersecting) {
        setPage(prev => prev + 1);
      }
    });

    if (loaderRef.current) {
      observer.observe(loaderRef.current);
    }

    return () => observer.disconnect();
  }, []);

  return (
    <>
      {users.map(user => (
        <p key={user.id}>{user.name}</p>
      ))}

      <div ref={loaderRef}>
        Loading more...
      </div>
    </>
  );
}
```

---

## Important

Infinite scrolling should also consider:

```text
loading
hasMore
error
request cancellation
duplicate requests
```

Otherwise multiple requests can accidentally be triggered.

---

# 9. Debouncing

## Definition

**Debouncing** delays executing a function until a specified amount of time has passed since the last call.

---

## Real-Life Example

Search box:

```text
User types:

R
Re
Rea
Reac
React
```

Without debounce:

```text
R     → API
Re    → API
Rea   → API
Reac  → API
React → API
```

Five API requests.

With debounce:

```text
R
Re
Rea
Reac
React
     ↓
Wait 500ms
     ↓
API request
```

Only one request.

---

## Debounce Function

```js
function debounce(callback, delay) {
  let timer;

  return (...args) => {
    clearTimeout(timer);

    timer = setTimeout(() => {
      callback(...args);
    }, delay);
  };
}
```

---

## React Search Example

```jsx
import { useEffect, useState } from "react";

function Search() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);

  useEffect(() => {
    if (!query.trim()) {
      setResults([]);
      return;
    }

    const timer = setTimeout(async () => {
      const response = await fetch(
        `/api/search?q=${encodeURIComponent(query)}`
      );

      const data = await response.json();

      setResults(data);
    }, 500);

    return () => clearTimeout(timer);
  }, [query]);

  return (
    <>
      <input
        value={query}
        onChange={event => setQuery(event.target.value)}
        placeholder="Search..."
      />

      {results.map(result => (
        <p key={result.id}>{result.name}</p>
      ))}
    </>
  );
}
```

---

## Debouncing vs Throttling

### Debouncing

Runs **after activity stops**.

```text
Typing → Typing → Typing → STOP
                         ↓
                       Run
```

Best for:

* Search
* Validation
* Autocomplete
* API requests

### Throttling

Runs at **most once during a specified interval**.

```text
Event → Run
Event
Event
Event → Run
```

Best for:

* Scroll events
* Mouse movement
* Resize events

---

# 10. Polling

## Definition

**Polling** means repeatedly making API requests at a fixed interval to check for updated data.

---

## Real-Life Example

Imagine repeatedly asking:

> "Is my order ready?"

```text
Check
 ↓
Wait 5 seconds
 ↓
Check
 ↓
Wait 5 seconds
 ↓
Check
```

That is polling.

---

## React Polling Example

```jsx
import { useEffect, useState } from "react";

function OrderStatus() {
  const [status, setStatus] = useState("pending");

  useEffect(() => {
    async function checkStatus() {
      const response = await fetch("/api/order/status");

      const data = await response.json();

      setStatus(data.status);
    }

    checkStatus();

    const interval = setInterval(checkStatus, 5000);

    return () => clearInterval(interval);
  }, []);

  return <p>Status: {status}</p>;
}
```

The API is checked every:

```text
5000ms = 5 seconds
```

---

## Why Cleanup Matters

Without:

```js
return () => clearInterval(interval);
```

the interval may continue running after the component unmounts.

This can cause:

* Memory leaks
* Unnecessary API requests
* Multiple intervals
* Performance problems

---

## Polling Use Cases

Common examples:

* Order status
* Payment status
* Notifications
* Job processing
* Dashboard metrics
* Background task status

---

## Polling vs WebSocket

### Polling

```text
Client → Request
Client ← Response

Wait

Client → Request
Client ← Response
```

### WebSocket

```text
Client ←→ Server

Persistent connection
```

WebSockets are better when the server needs to push real-time updates frequently.

---

# 11. Request Cancellation

## Definition

**Request cancellation** means stopping an API request that is no longer needed.

---

## Real-Life Example

Imagine searching:

```text
React
```

Then immediately searching:

```text
React Router
```

The first request may still be running.

If its result arrives later, it could overwrite the newer search result.

Therefore:

```text
Cancel old request
      ↓
Send new request
```

---

## AbortController with fetch

```jsx
useEffect(() => {
  const controller = new AbortController();

  async function fetchUsers() {
    try {
      const response = await fetch("/api/users", {
        signal: controller.signal
      });

      const data = await response.json();

      setUsers(data);
    } catch (error) {
      if (error.name === "AbortError") {
        return;
      }

      console.error(error);
    }
  }

  fetchUsers();

  return () => {
    controller.abort();
  };
}, []);
```

---

## Why Cancel Requests?

Useful when:

* Component unmounts
* Search query changes
* User navigates away
* New request makes old request irrelevant
* Large requests are still processing

---

## Request Race Condition

Suppose:

```text
Request A → "React"
Request B → "React Router"
```

If:

```text
B finishes first
A finishes later
```

The UI might incorrectly show results from A.

Cancellation helps prevent stale requests from updating the UI.

---

## Axios Cancellation

Modern Axios also supports `AbortController`.

```js
const controller = new AbortController();

axios.get("/api/users", {
  signal: controller.signal
});

controller.abort();
```

---

# 12. Complete API Pattern

A good React API component usually manages:

```text
Data
Loading
Error
```

Example:

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    async function fetchUsers() {
      try {
        setLoading(true);
        setError(null);

        const response = await fetch("/api/users", {
          signal: controller.signal
        });

        if (!response.ok) {
          throw new Error("Failed to fetch users");
        }

        const result = await response.json();

        setData(result);
      } catch (error) {
        if (error.name === "AbortError") {
          return;
        }

        setError(error.message);
      } finally {
        setLoading(false);
      }
    }

    fetchUsers();

    return () => {
      controller.abort();
    };
  }, []);

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error}</p>;
  }

  return (
    <div>
      {data.map(user => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

---

# 13. Quick Revision

| Concept              | One-Line Definition                                |
| -------------------- | -------------------------------------------------- |
| `fetch()`            | Built-in API for making HTTP requests              |
| Axios                | Third-party HTTP client for API communication      |
| Loading State        | Represents an API request currently in progress    |
| Error State          | Stores information about a failed operation        |
| Caching              | Reuses previously fetched data                     |
| Pagination           | Divides large data into smaller pages              |
| Infinite Scroll      | Automatically loads more data while scrolling      |
| Debouncing           | Executes after activity stops for a specified time |
| Polling              | Repeatedly requests the server at intervals        |
| Request Cancellation | Stops an unnecessary API request                   |

---

# API Request Lifecycle

The overall lifecycle can be remembered as:

```text
User Action
     ↓
API Request
     ↓
Loading = true
     ↓
┌───────────────┐
│ API Response  │
└───────────────┘
     ↓
 ┌───────┬───────┐
 ↓       ↓       ↓
Success Error Cancel
 ↓       ↓       ↓
Data    Error   Ignore
 ↓       ↓
Render  Message
```

---

# Important API Optimization Techniques

## 1. Debouncing

Reduce unnecessary requests.

```text
Typing → wait → request
```

## 2. Caching

Avoid duplicate requests.

```text
Request → Cache → Reuse
```

## 3. Pagination

Avoid downloading huge datasets.

```text
10000 records
↓
20 records per page
```

## 4. Request Cancellation

Stop obsolete requests.

```text
Old request → Cancel
New request → Continue
```

## 5. Polling

Keep data periodically updated.

```text
Request → wait → Request → wait → Request
```

## 6. Infinite Scroll

Load data progressively.

```text
Page 1
 ↓
Page 2
 ↓
Page 3
```

---

# 14. Interview Questions

### Basic

1. What is an API?
2. What is asynchronous JavaScript?
3. What is `fetch()`?
4. What is Axios?
5. What is the difference between `fetch()` and Axios?
6. Why do we need loading states?
7. Why do we need error states?

### Intermediate

8. How do you fetch data in React?
9. Why is API fetching commonly performed inside `useEffect()`?
10. How does caching improve application performance?
11. What is pagination?
12. What is infinite scrolling?
13. What is debouncing?
14. What is the difference between debouncing and throttling?
15. What is polling?
16. Why should an interval be cleaned up in `useEffect()`?
17. What is request cancellation?
18. What is `AbortController`?

### Advanced

19. How would you prevent duplicate API requests?
20. How would you handle race conditions between API requests?
21. How would you implement search with debounce and cancellation?
22. How would you design infinite scrolling for a large dataset?
23. Offset pagination vs cursor pagination?
24. How would you cache API responses in React?
25. How would you implement retry logic?
26. How would you handle token expiration during API requests?
27. How would Axios interceptors help with authentication?
28. How would you prevent stale API responses from updating the UI?
29. When would you choose polling over WebSockets?
30. How would you optimize a React application making many API requests?

---

# Final Mental Map

```text
                    API & ASYNC
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
        fetch           Axios       API State
                                       │
                                ┌──────┼──────┐
                                ↓      ↓      ↓
                              Data   Loading  Error
                                │
                ┌───────────────┼───────────────┐
                ↓               ↓               ↓
             Caching        Pagination       Infinite
                                               Scroll
                │
        ┌───────┴────────┐
        ↓                ↓
   Debouncing         Polling
                           │
                           ↓
                  Request Cancellation
```

## Remember

> **Good API handling in React is not just about fetching data. It is about controlling the complete request lifecycle: loading, success, error, caching, pagination, optimization, cancellation, and synchronization.**
