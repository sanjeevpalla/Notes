# 🧩 FastAPI — Complete Course: Beginner to Advanced, with Blog API Capstone Project

## Table of Contents

1. [Session Overview](#session-overview)
2. [Learning Objectives](#learning-objectives)
3. [Detailed Notes](#detailed-notes)
   1. [What is an API and Why FastAPI](#1-what-is-an-api-and-why-fastapi)
   2. [Environment Setup and Your First FastAPI App](#2-environment-setup-and-your-first-fastapi-app)
   3. [Routes, GET Requests, and Swagger UI](#3-routes-get-requests-and-swagger-ui)
   4. [Path Parameters (Dynamic Routes)](#4-path-parameters-dynamic-routes)
   5. [Query Parameters](#5-query-parameters)
   6. [Request Body, POST, and the Problem Pydantic Solves](#6-request-body-post-and-the-problem-pydantic-solves)
   7. [Pydantic Models and Nested Models](#7-pydantic-models-and-nested-models)
   8. [CRUD Fundamentals with an In-Memory Store](#8-crud-fundamentals-with-an-in-memory-store)
   9. [Combining Path, Query, and Body in One API](#9-combining-path-query-and-body-in-one-api)
   10. [Response Models](#10-response-models)
   11. [Status Codes and Basic Error Handling](#11-status-codes-and-basic-error-handling)
   12. [Advanced Exception Handling (Custom and Global)](#12-advanced-exception-handling-custom-and-global)
   13. [Dependency Injection](#13-dependency-injection)
   14. [Middleware](#14-middleware)
   15. [SQLite: Your First Real Database](#15-sqlite-your-first-real-database)
   16. [SQLAlchemy ORM Setup](#16-sqlalchemy-orm-setup)
   17. [CRUD with SQLAlchemy](#17-crud-with-sqlalchemy)
   18. [Asynchronous Programming (async/await)](#18-asynchronous-programming-asyncawait)
   19. [Basic JWT Authentication](#19-basic-jwt-authentication)
   20. [Production Authentication: OAuth2 + JWT + Password Hashing](#20-production-authentication-oauth2--jwt--password-hashing)
   21. [File Upload and Static Files](#21-file-upload-and-static-files)
   22. [CORS (Cross-Origin Resource Sharing)](#22-cors-cross-origin-resource-sharing)
   23. [Environment Variables and Config Management](#23-environment-variables-and-config-management)
   24. [Testing FastAPI Apps with pytest](#24-testing-fastapi-apps-with-pytest)
   25. [Third-Party API Integration](#25-third-party-api-integration)
   26. [Web Scraping with BeautifulSoup](#26-web-scraping-with-beautifulsoup)
   27. [Pagination](#27-pagination)
   28. [Caching](#28-caching)
   29. [Rate Limiting](#29-rate-limiting)
   30. [Deployment to Render](#30-deployment-to-render)
   31. [Capstone Project: The Blog API](#31-capstone-project-the-blog-api)
4. [Glossary](#glossary)
5. [Revision Notes (One-Minute Summary)](#revision-notes-one-minute-summary)
6. [Cheat Sheet](#cheat-sheet)
7. [Interview Questions and Answers](#interview-questions-and-answers)
8. [Scenario-Based Questions](#scenario-based-questions)
9. [Hands-on Exercises](#hands-on-exercises)
10. [Practice Assignment](#practice-assignment)
11. [Additional Resources](#additional-resources)
12. [Final Revision Sheet](#final-revision-sheet)

---

## Session Overview

This course takes you from "What is an API?" all the way to deploying a production-style, database-backed, JWT-authenticated REST API — using **FastAPI**, a modern, high-performance Python web framework.

The course is organized (as taught) into roughly these arcs:

1. **Fundamentals** — what an API/route/request is, installing FastAPI, GET routes, Swagger UI.
2. **Input handling** — path parameters, query parameters, request bodies, and Pydantic validation (including nested models).
3. **CRUD design** — building Create/Read/Update/Delete endpoints first against an in-memory list, then against a real database.
4. **Professional API behavior** — response models (hiding sensitive fields), status codes, custom responses, exception handling (both basic and global).
5. **Reusable architecture** — dependency injection and middleware.
6. **Persistence** — SQLite basics, then SQLAlchemy ORM (the industry-standard approach), and full CRUD rebuilt on top of it.
7. **Performance concepts** — synchronous vs. asynchronous execution.
8. **Security** — basic JWT authentication, then the production combination of OAuth2 + JWT + password hashing (bcrypt).
9. **Practical backend features** — file upload & static files, CORS for frontend integration, environment variables & config management, automated testing with pytest.
10. **Integration and backend utility skills** — calling third-party APIs, web scraping, pagination, caching, and rate limiting.
11. **Shipping it** — deploying to Render.com with Git/GitHub.
12. **Capstone** — a full "Blog API" project built on **PostgreSQL**, consolidating almost every concept from the course: SQLAlchemy models/schemas, complete CRUD, JWT-protected routes, pagination + search, and a GitHub push.

Throughout, the instructor repeatedly uses the same teaching pattern: explain the concept with a real-world analogy (e-commerce, Instagram, Amazon, Google search) → live-code the example in VS Code on a dedicated Git branch → test it via Swagger UI (`/docs`) and/or Postman/Thunder Client → close with a short set of instructor-flagged interview questions. This guide preserves that same rhythm inside each topic.

> 💡 **Why this course matters for interviews:** FastAPI is now one of the most commonly asked-about Python backend frameworks in interviews, specifically because it touches so many transferable backend concepts in one place: validation, dependency injection, async programming, ORMs, auth, and API design. Nearly every concept in this guide has a direct "tell me about X" interview angle, which is why Interview Q&A call-outs are attached to almost every section.

---

## Learning Objectives

By the end of this guide, you should be able to:

- Explain what an API is, what a route is, and how HTTP methods (GET/POST/PUT/DELETE) map to CRUD operations.
- Set up a FastAPI project correctly, with or without a virtual environment, and run it with Uvicorn.
- Build and validate path parameters, query parameters, and request bodies — using Pydantic models, including nested models.
- Design full CRUD APIs, first against an in-memory store, then against a real database.
- Use Response Models to control exactly what data leaves your API (and why that matters for security).
- Apply proper HTTP status codes, custom JSON responses, and both basic (`HTTPException`) and advanced (custom + global) exception handling.
- Explain and implement Dependency Injection (`Depends`) and Middleware, and clearly distinguish the two.
- Connect FastAPI to a database using both raw SQLite and the SQLAlchemy ORM, and justify why SQLAlchemy is the industry-preferred approach for larger applications.
- Explain synchronous vs. asynchronous execution and know when `async`/`await` actually helps.
- Implement authentication with JWT, and upgrade it to a production-style OAuth2 + JWT + bcrypt-hashed-password flow.
- Handle file uploads and serve static files.
- Diagnose and fix CORS errors between a frontend and a FastAPI backend.
- Manage secrets and configuration using `.env` files and a centralized `Settings`/config pattern.
- Write automated tests for FastAPI endpoints using `pytest` and `TestClient`.
- Integrate third-party APIs, scrape web data responsibly, and implement pagination, caching, and rate limiting.
- Deploy a FastAPI application to Render.com using Git/GitHub.
- Build a complete, real-world, PostgreSQL-backed "Blog API" that combines ORM models, Pydantic schemas, full CRUD, JWT-protected routes, and pagination + search — and be able to describe, debug, and extend a project exactly like it in an interview setting.

---

## Detailed Notes

### 1. What is an API and Why FastAPI

#### 📖 Concept: What is an API

An **API (Application Programming Interface)** is the bridge between a frontend (a website, mobile app, or any UI) and a backend. The frontend sends a **request**; the backend processes it (often pulling data from a database) and sends back a **response** — almost always in **JSON** format, because JSON is lightweight and easy for any frontend technology to consume.

```mermaid
flowchart LR
    A[Client / Frontend - React, mobile app, any UI] -->|HTTP Request| B[REST API - built with FastAPI]
    B -->|Query| C[(Database - MySQL, PostgreSQL, SQLite...)]
    C -->|Data| B
    B -->|HTTP Response - JSON| A
```

- GET, POST, PUT, and DELETE APIs are all written on the backend. Data flows up from the database, through the API, and down to the frontend for display.
- **Real-world examples**: Google search results, an Instagram feed, Amazon product listings, and e-commerce login/checkout flows are all backed by APIs like this.

#### 📖 Concept: What is FastAPI

FastAPI is a **modern Python web framework** purpose-built for creating fast, high-performance REST APIs. If you need a backend for a frontend, mobile app, login system, or e-commerce product catalog, FastAPI is a strong default choice.

#### 🔍 Deep Dive: FastAPI vs. Flask vs. Django

| Aspect | FastAPI | Django | Flask |
|---|---|---|---|
| Category | API-specialized framework | Full-stack framework | Micro framework |
| Relative speed | Fastest | Fast | Fast, but behind FastAPI in this course's framing |
| Async support (as taught) | Yes, built-in | Not emphasized in this course | Not emphasized in this course |
| Auto-generated interactive docs | Yes (Swagger-style `/docs`) | No, by default | No, by default |
| Built-in data validation | Yes (via Pydantic) | Manual | Manual |
| Best for | High-performance APIs, microservices | Large full-stack apps (admin panel, templating, ORM baked in) | Lightweight apps |

> ⚠ **Nuance/fact-check (added beyond the transcript):** the instructor's claim that "Django and Flask don't support async" is a simplification for beginners. Django has supported async views since version 3.1 (released 2020), and Flask has had some async view support since Flask 2.0. The core teaching point — that FastAPI was designed async-first and makes async easiest to use correctly — still holds and is the real interview-relevant takeaway.

**Two features the instructor explicitly flags as needing clear understanding:**
1. **Validation** — FastAPI automatically validates incoming data. If a client sends the wrong data type, FastAPI auto-generates a clear error — no manual `if/else` checks needed.
2. **Automatic API docs** — Visiting the `/docs` URL gives you an interactive, Swagger-style UI with input fields and live response viewing. You often don't need Postman at all for quick testing.

#### 🪜 Course roadmap (as announced by the instructor)

1. Basics (so everyone has the same foundation).
2. CRUD APIs (Create, Read, Update, Delete).
3. Connecting to a database.
4. Authentication (via JWT).
5. File upload.
6. Deployment.
7. A production-level capstone project.

**Prerequisites:** basic Python knowledge is enough — no heavy prerequisites.

#### 🎯 Key Takeaways
- An API is a structured bridge between frontend and backend, almost always exchanging JSON.
- FastAPI's three headline advantages, repeated throughout the course, are: **speed**, **automatic validation**, and **automatic interactive documentation**.
- FastAPI is API-specialized; Django is full-stack; Flask is minimal. Choose based on project shape, not hype.

---

### 2. Environment Setup and Your First FastAPI App

#### 🪜 Step-by-step: Checking your Python version

- Windows: `python --version`
- Mac/Linux: `python3 --version` (plain `python` is often not mapped on macOS)
- **Requirement:** Python **3.8+** is needed for solid FastAPI compatibility.
- Recommended editor: VS Code.

#### 📖 Concept: Virtual environments (`venv`)

A virtual environment isolates a project's dependencies so different projects can use different package versions without clashing. The instructor states that **almost 99.9% of real-world projects are built inside a virtual environment** — it is industry standard, even though some demo videos skip it for speed/simplicity.

#### 🛠 Method 1 — Without a virtual environment (global install, fine for quick learning only)

```bash
pip install fastapi
pip install uvicorn
pip list     # confirms fastapi & uvicorn are installed
```

> ⚠ **Warning:** a global install is fine for a five-minute demo, but not for real projects — different projects' dependency versions will clash. Industry projects are built inside a venv.

**First "Hello World" app:**

```python
# main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"message": "Hello without virtual environment"}
```

Run it:

```bash
uvicorn main:app --reload
```

- `uvicorn` = the ASGI server that actually runs your app.
- `main` = the filename (`main.py`).
- `app` = the `FastAPI()` instance variable inside that file.
- `--reload` = automatically restarts the server whenever you save code changes.

> 💡 **Memory Trick:** `uvicorn main:app --reload` reads almost like a sentence: "uvicorn, run `app` from `main`, and reload on change."

#### 🛠 Method 2 — With a virtual environment (recommended)

```bash
# Create
python -m venv venv        # Windows
python3 -m venv venv       # Mac/Linux

# Activate
venv\Scripts\activate          # Windows
source venv/bin/activate       # Mac/Linux
```

- A successful activation shows a `(venv)` prefix in your terminal prompt.
- Inside the activated venv, install the same packages (`pip install fastapi uvicorn`) — `pip list` will now show *only* what this project needs, proving isolation.

#### 🏗 Project structure (preview, built up as the course progresses)

A typical FastAPI project eventually contains: a `static/` folder (CSS/JS), a templates folder, one or more database files, a main application file, and sometimes a dedicated logging file. (We will see the real shape of this in the Blog API capstone, Section 31.)

#### ❌ Common Mistakes
- Forgetting to activate the venv before installing packages (installs land globally instead).
- Using bare `python` on macOS instead of `python3`.
- Forgetting `--reload`, then wondering why code changes aren't taking effect.

#### 🚀 Best Practices
- Always use a virtual environment for anything beyond a five-minute demo.
- Pin your Python version to 3.8+ for FastAPI.
- Keep `uvicorn main:app --reload` as your go-to dev-run command.

#### 🎯 Key Takeaways
- `pip install fastapi uvicorn` are your two foundational packages.
- `uvicorn main:app --reload` is the standard way to run a FastAPI app in development.
- Visiting `/docs` on a running app gives you the interactive Swagger UI "for free."

---

### 3. Routes, GET Requests, and Swagger UI

#### 📖 Definition: Route

A **route** is a URL path (e.g., `/`, `/about`, `/users`) attached to a specific function. When that path is requested, FastAPI runs the corresponding function.

#### 📖 Definition: GET Request

A **GET** request *fetches* data — it is read-only and never changes server-side data.

> 💡 **Memory Trick:** "GET = only fetching data, not changing it." Think Google search results, Instagram feed scrolling, or browsing Amazon listings — all GET requests under the hood.

#### 💻 Code Example: multiple simple routes

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"message": "Welcome to FastAPI"}

@app.get("/about")
def about():
    return {"message": "This is about page"}

@app.get("/users")
def users():
    return {"users": ["Mohit", "Rohit", "Amit"]}
```

- Run with `uvicorn main:app --reload`; since `--reload` is on, you don't need to restart the server after each edit.
- Visit `/`, `/users`, `/about` directly in the browser to see each route's own JSON.
- Visit `/docs` to see all three routes listed in Swagger UI, each with a "Try it out" + "Execute" button — no Postman required for quick manual testing.

> ⚠ **Important clarification:** viewing raw JSON in a browser is *not* the same as viewing a rendered webpage. It's just raw data meant to be consumed and rendered into HTML by a frontend (like React). The browser/Swagger views here are for **backend testing only**.

#### 🔥 Interview Q&A (as explicitly flagged by the instructor)
1. **What is an API?** — A bridge connecting frontend and backend; frontend sends a request, backend sends a response, almost always as JSON.
2. **What is a Route?** — A URL path where a specific function executes.
3. **What is a GET request?** — Fetches/reads data; it cannot modify data.
4. **Why does FastAPI return JSON by default?** — Because the core purpose of an API is data exchange, and JSON is lightweight and frontend-friendly.
5. **What is Swagger UI?** — Auto-generated interactive API documentation where you can test endpoints without Postman.

#### 🎯 Key Takeaways
- A route maps a URL path to a handler function.
- GET is for reading data only.
- `/docs` (Swagger UI) is your fastest manual-testing tool during development.

---

### 4. Path Parameters (Dynamic Routes)

#### 📖 Concept: Dynamic Routes / Path Parameters

Instead of hardcoding a separate route per record, a single route template can serve many different records based on a changing value embedded directly in the URL path — e.g., `/users/1`, `/users/2`.

> 💡 **Real-world analogy:** an e-commerce product detail page. The route template is the same for every product; only the product ID in the URL changes what's shown.

#### 💻 Code Example

```python
@app.get("/user/{user_id}")
def get_user(user_id):
    return user_id
```

- `{user_id}` is a placeholder; FastAPI passes the matching URL segment into the `user_id` function parameter.
- Visiting `/users` (no ID given) → a "not found"-style error, since no value was supplied.
- Visiting `/user/1001` → returns `1001`. Change the number, and the output changes — this is **dynamic routing**.

#### ⚙ How It Works: type validation on path parameters

```python
@app.get("/user/{user_id}")
def get_user(user_id: int):
    ...
```

- `/user/200` → works (valid integer).
- `/user/200abc` → **automatic validation error**: *"Input should be a valid integer, unable to parse string as an integer."* This happens identically whether you hit the raw URL or use Swagger's "Try it out."
- Changing the type hint to `str` makes previously-invalid values pass through without error — the type hint *is* the validation rule.

> 🎯 **Key teaching point:** FastAPI performs type validation on path parameters automatically, directly from your Python type hints — no manual checks required.

#### 🔥 Interview Q&A
1. What is a path parameter?
2. What's the difference between path parameters and query parameters? *(Answered fully in the next section.)*
3. How does type validation work on a path parameter? — Via the Python type hint on the function parameter.
4. What happens if you pass the wrong type? — FastAPI automatically raises a validation error.
5. Real-world use case? — Fetching one specific record by ID (a user, a product, an order).

#### 🎯 Key Takeaways
- Path parameters identify *which specific resource* you want.
- Type hints on path parameters double as automatic validation rules.

---

### 5. Query Parameters

#### 📖 Definition: Query Parameters

Extra data appended to a URL after a `?`, in `key=value` form (and chained with `&` for multiple values). Used primarily for **filtering, searching, and sorting**.

> 💡 **Real-world examples:** Amazon's price filters, Instagram's user search by name — both query-parameter-driven.

#### 💻 Code: basic (required) query parameter

```python
@app.get("/users")
def get_users(name):
    return {"name": name}
```

- `/users?name=mohit` → `{"name": "mohit"}`.
- `/users` (no `name`) → **error**: field required — FastAPI auto-validates that a required query param is present.

#### 💻 Code: optional query parameter with a default

```python
@app.get("/users")
def get_users(name: str = None):
    return {"name": name}
```

- Giving the parameter a default (`= None`) makes it optional — no more "field required" error.

#### 💻 Code: default values & multiple query parameters

```python
@app.get("/products")
def get_products(limit: int = 10):
    return {"limit": limit}

@app.get("/items")
def get_items(name: str = None, price: int = 0):
    return {"name": name, "price": price}
```

- `/products` → `limit = 10` (default used).
- `/products?limit=200` → `limit = 200` (override).
- `/items?name=laptop&price=2000` → both values captured; multiple filters combined with `&`.

#### ⚠ Common Mistakes & Fixes

| Problem | Cause | Solution |
|---|---|---|
| "Field required" error on a query param | No default value set | Give it a default (`= None` or a sensible value) |
| Query param silently ignored | Forgot `&` between multiple params in the URL | Join multiple `key=value` pairs with `&` |

#### 🔥 Interview Q&A
1. What are query parameters, and how do they differ from path parameters? — Path identifies *which* resource; query parameters filter/sort/paginate *within or around* that request.
2. How do you make a query parameter optional? — Give it a default value with a type hint.
3. How do you combine multiple query parameters? — Declare multiple parameters in the function signature; join with `&` in the URL.
4. Real-world use cases? — Search, filtering, pagination (e.g., e-commerce price filters, category filters).

#### 🎯 Key Takeaways
- Query parameters live after `?` in the URL and are for optional, filter-like inputs.
- Always give optional query parameters sensible defaults to avoid "field required" errors.

---

### 6. Request Body, POST, and the Problem Pydantic Solves

#### 📖 Definition: Request Body

Data the **client sends to the backend** — e.g., a sign-up form's name/email/password — almost always sent as **JSON**.

#### 📖 Definition: POST Request

**GET fetches; POST creates.** Used for registration, adding a product, creating a blog post, etc.

> 💡 **Memory Trick:** "GET means fetch, POST means create." Also important: **GET APIs can be tested directly in the browser URL bar; POST/PUT/DELETE cannot** — you need Swagger UI (`/docs`) or a tool like Postman/Thunder Client.

#### 💻 Code: POST using query-style parameters (works, but limited)

```python
@app.post("/create-user")
def create_user(name: str, age: int):
    return {"name": name, "age": age}
```

#### 💻 Code: POST using a raw `dict` as the request body

```python
@app.post("/create-user")
def create_user(user: dict):
    return {"message": "User created", "data": user}
```

- Tested via Swagger: the request body becomes a free-form, editable JSON textbox.
- **Important detail:** unlike with query params, the *requested URL itself doesn't change* here — the data travels in the body, not the URL.

#### ❌ The problem with raw `dict` (motivates Pydantic)

With a plain `dict`, **whatever the client sends is simply accepted — there is no validation at all**. Any key/value typed in gets echoed back; nothing enforces structure or type.

> 🎯 The instructor calls **Pydantic** "FastAPI's killer feature" — it's introduced explicitly to solve exactly this problem: it lets you **validate data** and **define a schema**.

#### 💻 Code: first Pydantic model

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

@app.post("/create-user")
def create_user(user: User):
    return {"name": user.name, "age": user.age}
```

- Entering a non-integer string for `age` (e.g., `"abc"`) triggers an **automatic validation error**: *"Input should be a valid integer."*

**Benefits of Pydantic (explicitly summarized by the instructor):**
1. Automatic error validation.
2. Auto-generated, accurate docs (the schema shows up in Swagger).
3. Clean, structured, readable code.

#### 🔥 Interview Q&A
1. What is a request body?
2. How do you use POST requests?
3. What is Pydantic?
4. `dict` vs. Pydantic model for request validation — `dict` is flexible but has zero validation (manual checks needed); Pydantic gives structured, automatic validation and automatic error handling "for free."

#### 🎯 Key Takeaways
- POST sends data to create something; it cannot be tested via a plain browser URL bar.
- A raw `dict` request body accepts anything — no safety net.
- Pydantic's `BaseModel` turns a plain class into both a schema *and* a validator.

---

### 7. Pydantic Models and Nested Models

#### 📖 Concept: Pydantic model as schema

> "A Pydantic model is a schema structure that defines what format the data will be in." — e.g., a `User` model predefines that a user must have exactly `name`, `age`, `email`; nothing outside the declared fields is part of the expected schema.

#### 💻 Code: a fuller Pydantic model

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    age: int
    email: str

@app.post("/create-user")
def create_user(user: User):
    return {"message": "User created", "data": user}
```

- Swagger shows the generated schema for `User` with all three typed fields — "clean structure and readable code."
- Deliberately sending `age` as a string triggers an automatic validation error, illustrating that **Pydantic catches mistakes before they reach your business logic**.

#### 📖 Concept: Nested Models

Described by the instructor as **"the most common and most famous"** real-world Pydantic pattern — modeling data that itself contains structured sub-data (nested JSON).

#### 💻 Code: nested models

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Address(BaseModel):
    city: str
    pin_code: int

class User(BaseModel):
    name: str
    age: int
    address: Address      # nested model

@app.post("/users")
def create_user(user: User):
    print(user)
    return {"message": "User created", "data": user}
```

- Swagger's schema view shows `name`, `age`, and then `address` as a nested curly-brace object containing `city` and `pin_code` — exactly mirroring the nested-JSON shape.
- Tested with `age=25`, `city="Delhi"`, `pin_code=200101` → output JSON mirrors the same nested structure, validated end-to-end.

```mermaid
classDiagram
    class User {
      +str name
      +int age
      +Address address
    }
    class Address {
      +str city
      +int pin_code
    }
    User --> Address : nested model
```

#### 🔥 Interview Q&A
1. What is Pydantic? What is `BaseModel`?
2. What is a schema, and how does Pydantic enforce one?
3. How do you validate nested/complex data with Pydantic?
4. What is a nested model, and what's a real-world use case? — Any object that logically contains another object: a `User` with an `Address`, an `Order` with `LineItems`, a `Blog` with an `Author` profile.
5. `dict` vs. Pydantic — same comparison as Section 6, reinforced here.

#### 🎯 Key Takeaways
- Nested Pydantic models let you validate deeply structured JSON in one declarative pass.
- FastAPI's biggest practical advantage, repeated throughout the course: **you stop writing manual `if/else` validation** — Pydantic does it declaratively.

---

### 8. CRUD Fundamentals with an In-Memory Store

#### 📖 Concept: CRUD

**C**reate, **R**ead, **U**pdate, **D**elete — the four basic data operations every backend needs. Taught first against a simple in-memory Python list (no real database yet) so the *shape* of CRUD is clear before adding database complexity.

> 🚀 **Best practice called out by the instructor:** keep the **same URL** for all CRUD operations on one resource (e.g., always `/todos`), differentiated only by HTTP method. This avoids route-naming confusion.

#### 💻 Setup

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()
todos = []   # in-memory "database"

class Todo(BaseModel):
    id: int
    title: str
    completed: bool
```

#### 💻 Create (POST)

```python
@app.post("/todos")
def create_todo(todo: Todo):
    todos.append(todo)
    return {"message": "Todo added", "data": todo}
```

> ⚠ **Common mistake explicitly demonstrated live:** giving two different todos the *same* ID. **IDs must always be unique**, because every later lookup, update, and delete depends on ID-based comparison.

Testing tools demonstrated: Swagger UI's "Try it out," and **Thunder Client** (a VS Code REST-client extension with a Postman-like UI) — the instructor notes that while Swagger is convenient, learning Postman/Thunder Client matters because Postman is widely used industry-wide.

#### 💻 Read — all, and by ID

```python
@app.get("/todos")
def get_todos():
    return todos

@app.get("/todos/{todo_id}")
def get_todo(todo_id: int):
    for todo in todos:
        if todo.id == todo_id:
            return todo
    return {"error": "Todo not found"}
```

> 💡 **Real-world analogy:** clicking a specific product on an e-commerce site shows details for *that* product by ID — this is exactly the single-item GET-by-path-param pattern.

#### 💻 Update (PUT)

```python
@app.put("/todos/{todo_id}")
def update_todo(todo_id: int, updated_todo: Todo):
    for index, todo in enumerate(todos):
        if todo.id == todo_id:
            todos[index] = updated_todo
            return {"message": "Data updated", "data": updated_todo}
    return {"error": "Todo not found"}
```

> ⚠ **Live-debugged mistake (worth remembering):** the instructor initially forgot to actually reference `updated_todo` in the replacement line and the return line — a classic "wrote the right logic shape but forgot to wire the variable" bug. Always double-check that every variable you *intend* to use is the one actually referenced.

#### 💻 Delete (DELETE)

```python
@app.delete("/todos/{todo_id}")
def delete_todo(todo_id: int):
    for index, todo in enumerate(todos):
        if todo.id == todo_id:
            todos.pop(index)
            return {"message": "Data deleted"}
    return {"error": "Data not found"}
```

#### 🪜 Step-by-Step: the full CRUD request lifecycle (in-memory version)

```text
Client Request
     ↓
Route Matched (by URL + HTTP method)
     ↓
Function Executes (loops over in-memory list, matches by ID)
     ↓
List Mutated (append / replace / pop) — for Create/Update/Delete
     ↓
JSON Response Returned
```

#### 🔥 Interview Q&A
1. What is a CRUD operation?
2. Difference between POST, GET, PUT, and DELETE?
3. **Difference between PUT and PATCH?** — PUT replaces the *whole* object; PATCH partially updates specific fields. (This course only implements PUT, but the distinction is explicitly interview-flagged.)
4. What role does the ID field play, and why must it be unique?
5. How can you run CRUD without a database? — A temporary in-memory list gives the "look and feel" of a real database, but data resets whenever the server restarts.
6. Real-world examples of CRUD? — E-commerce systems, user management systems.

#### 🎯 Key Takeaways
- CRUD maps cleanly onto POST/GET/PUT/DELETE.
- Keep one URL per resource, differentiated by HTTP method.
- Unique IDs are the backbone of every update/delete/lookup operation.
- In-memory storage is great for *teaching* CRUD shape, but resets on every restart — hence the later move to SQLite/SQLAlchemy.

---

### 9. Combining Path, Query, and Body in One API

#### 📖 Concept

In real production APIs, path parameters, query parameters, and the request body are frequently used **together in a single endpoint**:

- **Path parameter** → identifies *which* resource ("Path matlab resource identify karna").
- **Query parameter** → supplies filters/options ("Query matlab filter karna").
- **Request body** → carries the actual data to send/update ("Data send karna hai to wo aap body me hi karoge").

```mermaid
flowchart TD
    A["PUT /users/1001?notify=true<br/>body: {name, age}"] --> B[Path param: user_id = 1001 - which user]
    A --> C[Query param: notify = true - option/flag]
    A --> D[Body: actual updated data]
```

#### 💻 Code

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    age: int

users = []

@app.post("/users")
def create_user(user: User):
    users.append(user)
    return {"message": "User created", "data": user}

@app.put("/users/{user_id}")
def update_user(user_id: int, notify: bool = False, user: User = ...):
    if user_id < len(users):
        users[user_id] = user
        return {"message": "User updated", "notify": notify, "data": user}
    return {"error": "User not found"}
```

> ⚠ **Important caveat the instructor flags explicitly:** this particular example uses the **list index** as a stand-in for a real ID (`users[user_id]`), not a true unique `id` field. For production-style ID matching, use the `enumerate` + `if todo.id == todo_id` pattern from Section 8 instead. This simplified version exists purely to demonstrate path+query+body working together in one function signature.

#### 🔥 Interview Q&A
1. Why are path, query, and body parameters often used together in real APIs?
2. How would you combine all three in a single endpoint?
3. What error is returned if the identifying path parameter doesn't match anything? — A custom "not found" error, via your own logic or `HTTPException`.

#### 🎯 Key Takeaways
- Path = *which* resource. Query = *filters/options*. Body = *actual payload*.
- Production endpoints routinely need all three at once — recognizing this pattern is a common interview/code-review talking point.

---

### 10. Response Models

#### 📖 Concept

A **Response Model** controls exactly what data your API sends back to the client — separate from what your backend internally has. You decide what to hide (e.g., passwords, tokens, secret keys) and what to expose.

> 💡 **Motivating example:** given `{"name": "Mohit", "age": 25, "password": "12345"}`, should the frontend ever receive the raw password? **No.** This is exactly what a Response Model is for.
>
> ⚠ **Security note:** passwords should never be sent in plaintext at all — they should be hashed/encrypted, or represented as a token instead. Response Models are a complementary control, not a substitute for hashing (see Section 20 for bcrypt).

#### 💻 Code

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    age: int
    password: str

class UserResponse(BaseModel):
    name: str
    age: int
    # password intentionally excluded

@app.get("/user", response_model=UserResponse)
def get_user():
    return {"name": "Mohit", "age": 24, "password": "123456"}
```

- The function *internally* returns a dict that includes `password`, but because `response_model=UserResponse` is set, FastAPI automatically strips out anything not in `UserResponse` — only `name` and `age` ever reach the client.
- **FastAPI also validates the *response*, not just the request** — giving `age` as a string internally (instead of int) triggers an Internal Server Error, because the returned data is checked against the response model's types too.

```mermaid
flowchart LR
    DB[(Backend data<br/>name, age, password)] --> RM[response_model = UserResponse]
    RM --> Client[Client receives only<br/>name, age]
```

#### 🔥 Interview Q&A
1. What is a Response Model, and why is it used?
2. How do you hide sensitive data from an API response?
3. How does FastAPI validate responses?
4. **Response Model vs. Request Model?** — Request Model defines the *input* shape (what the client sends); Response Model defines the *output* shape (what the client receives).

#### 🎯 Key Takeaways
- `response_model=` is a one-line way to guarantee sensitive internal fields never leak.
- Response validation is automatic too — a type mismatch against the response model is a real error, not a silent pass-through.

---

### 11. Status Codes and Basic Error Handling

#### 📖 Concept: HTTP Status Codes

| Code | Meaning |
|---|---|
| 200 | Success |
| 201 | Created (after a successful create operation) |
| 400 | Bad Request (invalid client input) |
| 401 | Unauthorized |
| 404 | Not Found |
| 429 | Too Many Requests (seen later with rate limiting) |
| 500 | Internal Server Error |

#### 💻 Setting a custom status code

```python
from fastapi import FastAPI, status

app = FastAPI()

@app.post("/create_user", status_code=status.HTTP_201_CREATED)
def create_user():
    return {"message": "User created"}
```

#### 💻 Sending a fully custom response shape

```python
@app.get("/user")
def get_user():
    return {
        "status": "success",
        "message": "User fetch",
        "data": {"name": "Mohit", "age": 24}
    }
```

> 💡 This `{status, message, data}` envelope pattern is explicitly described as how real-world backend-to-frontend data is commonly shaped at industrial scale.

#### 💻 Basic error handling with `HTTPException`

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

@app.get("/users/{user_id}")
def get_user(user_id: int):
    if user_id != 1:
        raise HTTPException(status_code=404, detail="User not found")
    return {"id": 1, "name": "Mohit"}
```

- `user_id=2` → `404`, detail `"User not found"`.
- `user_id=1` → `200`, returns the valid user.

#### 🔥 Interview Q&A
1. What is an HTTP status code?
2. Difference between status 200 and 201?
3. When does a 404 occur?
4. What is `HTTPException`? — FastAPI's built-in class for raising clean, custom HTTP errors.
5. Benefit of a custom response shape? — Gives a consistent API structure the frontend can integrate against predictably.

#### 🎯 Key Takeaways
- Use `status_code=` and `fastapi.status` constants instead of hardcoding numbers.
- `HTTPException(status_code=..., detail=...)` is the simplest way to return a clean, structured error.

---

### 12. Advanced Exception Handling (Custom and Global)

#### 📖 Concept: Why go beyond `HTTPException`

`HTTPException` is great for one-off errors, but repeating the same error-raising logic in every route doesn't scale. **Custom exceptions + a global exception handler** solve this: write the error-handling logic once, reuse it everywhere.

#### 💻 Step 1 — A custom exception class

```python
class UserNotFoundException(Exception):
    def __init__(self, name: str):
        self.name = name
```

#### 💻 Step 2 — Raising it in a route (incomplete — no handler yet)

```python
@app.get("/user/{name}")
def get_user(name: str):
    if name != "Mohit":
        raise UserNotFoundException(name=name)
    return {"name": name}
```

> ⚠ Without a registered handler, this produces a vague, unhelpful **generic 500 Internal Server Error** — exactly the problem that motivates the next step.

#### 💻 Step 3 — A global exception handler

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

app = FastAPI()

class UserNotFoundException(Exception):
    def __init__(self, name: str):
        self.name = name

@app.exception_handler(UserNotFoundException)
def user_not_found_handler(request: Request, exception: UserNotFoundException):
    return JSONResponse(
        status_code=404,
        content={"status": "error", "message": f"User {exception.name} not found"}
    )

@app.get("/user/{name}")
def get_user(name: str):
    if name != "Mohit":
        raise UserNotFoundException(name=name)
    return {"name": name}
```

- `name="Rohit"` → `404`, `{"status": "error", "message": "User Rohit not found"}`.
- `name="Mohit"` → `200`, `{"name": "Mohit"}`.

```mermaid
sequenceDiagram
    participant C as Client
    participant R as Route
    participant E as Custom Exception
    participant H as Global Handler
    C->>R: GET /user/Rohit
    R->>E: raise UserNotFoundException("Rohit")
    E->>H: caught by @app.exception_handler
    H-->>C: 404 JSONResponse with clean message
```

#### 🪜 Why Global Exception Handling (explicit rationale)

1. The same error type doesn't need to be re-raised and re-formatted in every single route.
2. One reusable function keeps the codebase clean and scalable.
3. It's typically kept in its own file and imported wherever needed.

#### 🔥 Interview Q&A
1. What is `HTTPException`, and how does it differ from a custom exception? — `HTTPException` is built-in and generic; a custom exception is user-defined for specific business-logic errors.
2. What is a Global Exception Handler, and what problem does it solve? — A centralized function that catches a specific exception type everywhere it's raised and returns a consistent response; avoids duplicated error-handling code.
3. Real-world use cases? — User not found, payment failure, unauthorized access.

#### 🎯 Key Takeaways
- Custom exceptions model *business-logic* errors, not just HTTP-layer errors.
- `@app.exception_handler(ExceptionType)` centralizes formatting for that error type, app-wide.

---

### 13. Dependency Injection

#### 📖 Concept

- **Dependency**: a function/logic another function needs.
- **Injection**: automatically supplying that logic.
- **Combined**: Dependency Injection = a function another function depends on, run and supplied automatically — rather than manually re-implemented every time.
- `Depends` is FastAPI's built-in helper for wiring dependencies into routes.

#### 💻 Minimal example

```python
from fastapi import FastAPI, Depends

app = FastAPI()

def common_logic():
    return {"message": "Common logic executed"}

@app.get("/home")
def home(data = Depends(common_logic)):
    return data
```

#### 💻 Reusable logic across multiple routes (the real use case)

```python
def get_current_user():
    return "Mohit"

@app.get("/profile")
def profile(user = Depends(get_current_user)):
    return user

@app.get("/dashboard")
def dashboard(user = Depends(get_current_user)):
    return user
```

> 🎯 **Key teaching point:** the *same* function is reused across multiple routes — this is the DRY (Don't Repeat Yourself) payoff of dependency injection. Without DI, you'd rewrite the same user-check logic in every route.

#### 💻 Real-world use case: token/auth verification via header

```python
from fastapi import FastAPI, Depends, Header, HTTPException

app = FastAPI()

def verify_token(token: str = Header(None)):
    if token != "my_secret_token":
        raise HTTPException(status_code=401, detail="Unauthorized Access")
    return "Authorized User"

@app.get("/secured-data")
def secure_data(user = Depends(verify_token)):
    return {"message": "Secure Data Access", "user": user}
```

- `token: str = Header(None)` reads the token from the **HTTP header** (not the JSON body — correct practice for auth tokens).
- Wrong/missing token → `401 Unauthorized`.
- Correct token → `200`, `"Authorized User"`.
- Testing with Postman/Thunder Client requires manually adding the token as a **request header**, not JSON body.

```mermaid
sequenceDiagram
    participant C as Client
    participant D as Dependency (verify_token)
    participant A as API Route
    C->>D: Request with header token
    alt token valid
        D->>A: Injected "Authorized User"
        A-->>C: 200 Secure Data Access
    else token invalid
        D-->>C: 401 Unauthorized Access
    end
```

#### 🔥 Interview Q&A
1. What is Dependency Injection? — A design pattern where a function's dependency is supplied from an external source automatically.
2. What does `Depends` do? — Automatically calls a function and injects its return value into the route.
3. Real use cases? — Auth/access checks, DB session management, reusing any shared logic.
4. Role of dependency in an auth system? — `Depends` verifies tokens and secures endpoints without duplicating logic.

#### 🎯 Key Takeaways
- Write shared logic once; inject it with `Depends(...)` anywhere it's needed.
- Dependencies can themselves raise `HTTPException` — a clean way to gate access before a route body even runs.

---

### 14. Middleware

#### 📖 Definition

> **"A function that sits between request and response is called middleware."**

Every request passes *through* middleware before reaching its route handler; every response passes back *through* middleware before reaching the client. Also described as a security/processing layer between frontend and backend.

```mermaid
sequenceDiagram
    participant C as Client
    participant M as Middleware
    participant R as Route Handler
    C->>M: Incoming Request
    M->>R: call_next(request)
    R-->>M: Response
    M-->>C: Response (after middleware post-processing)
```

#### 💻 Basic middleware

```python
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def my_middleware(request: Request, call_next):
    response = await call_next(request)
    print("Request received")
    print("Response sent")
    return response
```

- Middleware is **async** (`async def`) because it should be able to handle multiple requests simultaneously for high performance — explicitly flagged as a common interview question.
- `call_next` forwards the request onward to the actual handler, and carries the response back — the "pass it one step forward (and one step back)" mechanism.
- ⚠ **Gotcha:** `call_next` must be a plain parameter, not given a default value — adding `=` there causes a "default argument" error.

#### 💻 Real-world example: request-timing/logging middleware

```python
import time
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def log_middleware(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    process_time = time.time() - start_time
    print(f"Path: {request.url.path}, Time: {process_time}")
    return response
```

- Useful for performance tracking, debugging, and monitoring — printed per-request path and processing time regardless of which route was hit.

#### 🛠 Other real-world middleware use cases
- Authentication checks
- Request validation
- Rate limiting (see Section 29)
- Adding/removing response headers
- Running shared logic on *every* request, globally

#### 🔥 Interview Q&A — including the key distinction
1. What is middleware, and what's it used for?
2. What does `call_next` do? — Forwards the request to the actual handler and returns the resulting response.
3. Why is middleware asynchronous? — So the app stays performant and can handle multiple concurrent requests.
4. **Middleware vs. Dependency — what's the real difference?**

| | Middleware | Dependency |
|---|---|---|
| Scope | Global — runs on **every** request | Per-route — opt-in, route by route |
| Typical use | Logging, timing, CORS, global auth checks | Route-specific auth, DB session, reusable per-endpoint logic |

#### 🎯 Key Takeaways
- Middleware wraps *every* request/response globally; dependencies are opt-in per route.
- `call_next` is the pivot point that hands control to (and back from) the actual route handler.

---

### 15. SQLite: Your First Real Database

#### 📖 Concept

SQLite is a **lightweight, file-based database** — "Lite" signals that data lives in a single file, with no separate server process needed. Unlike an in-memory Python list, data **persists** across server restarts until explicitly deleted. SQLite comes **built into Python's standard library** (`sqlite3`) — no `pip install` needed.

#### 💻 Code: connect and create a table

```python
import sqlite3

connect = sqlite3.connect("test.db", check_same_thread=False)
cursor = connect.cursor()

cursor.execute('''
CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY,
    title TEXT,
    completed TEXT
)
''')
connect.commit()
```

- `check_same_thread=False` lets the DB connection be used safely across different threads — necessary for FastAPI's multi-request handling.
- The flow: **connect** (open the DB) → **cursor** (the object that runs SQL) → **execute** (run the query) → **commit** (persist the change to disk).
- A free VS Code extension ("SQLite3 Editor") was used purely for *viewing* the resulting table/schema, not for inserting data.

#### 🔍 Deep Dive: SQLite vs. SQLAlchemy

| | SQLite (raw) | SQLAlchemy (ORM) |
|---|---|---|
| Install | Built-in (`sqlite3`) | `pip install sqlalchemy` |
| You write | Raw SQL (`SELECT * FROM table_name`) | Python classes/objects (`db.query(Model).all()`) |
| Code at scale | Can get messy | Stays clean |
| Best for | Small applications | Large/enterprise applications |

> 🚀 **Rule of thumb (as given):** "If you want to write SQL yourself, use SQLite. If you want Python to handle the database for you, use SQLAlchemy."

#### 🔥 Interview Q&A
1. What is SQLite? — A lightweight, file-based database for small apps.
2. What is SQLAlchemy? — An ORM mapping Python objects to database tables.
3. What is an ORM? — A way to work with a database through Python code instead of raw SQL.
4. Which is "better"? — It depends: SQLite for small apps, SQLAlchemy for large/enterprise apps.

#### 🎯 Key Takeaways
- SQLite needs zero extra installation and persists data to a single file.
- The jump from SQLite to SQLAlchemy is a jump from "writing SQL by hand" to "modeling your database as Python classes" — the next section builds this up piece by piece.

---

### 16. SQLAlchemy ORM Setup

#### 📖 Concept

SQLAlchemy is an **ORM (Object Relational Mapper)** that lets you define database tables as Python classes and query them with Python method calls instead of raw SQL strings. Described as the **industry-standard approach**, used in roughly 90% of real-world FastAPI projects per the instructor.

#### 💻 Install

```bash
pip install sqlalchemy
```

#### 💻 Engine, Session, Base — the three core building blocks

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, declarative_base

DATABASE_URL = "sqlite:///./test.db"

engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})
SessionLocal = sessionmaker(bind=engine)
Base = declarative_base()
```

| Component | Role |
|---|---|
| `engine` | Connects to the actual database |
| `SessionLocal` | A session factory — lets you perform read/write operations |
| `Base` | The base class every ORM model (table) inherits from |

#### 💻 Defining a model (table)

```python
from sqlalchemy import Column, Integer, String

class Todo(Base):
    __tablename__ = "todos"

    id = Column(Integer, primary_key=True, index=True)
    title = Column(String)
    completed = Column(String)
```

```python
Base.metadata.create_all(bind=engine)   # actually creates the table in test.db
```

#### 💻 A per-request DB session dependency — the pattern you'll reuse everywhere

```python
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

- `get_db` is a **generator-based dependency**: it opens a fresh session, `yield`s it for the route to use, then closes it in a `finally` block — guaranteeing cleanup even if the route raises an error.
- This single function is the reusable dependency every DB-touching route will call via `Depends(get_db)`.

```python
from fastapi import FastAPI, Depends

app = FastAPI()

@app.get("/")
def home(db = Depends(get_db)):
    return {"message": "DB connected fine"}
```

```mermaid
flowchart LR
    Engine[create_engine] --> Session[SessionLocal / sessionmaker]
    Session --> GetDB[get_db generator dependency]
    GetDB -->|Depends| Route[Any route needing DB access]
    Base[declarative_base] --> Model[ORM Model e.g. Todo]
    Model -->|create_all| DBFile[(test.db)]
```

#### 🔥 Interview Q&A
1. What are the three core SQLAlchemy setup pieces, and what does each do? (engine, session, base)
2. Why use a generator dependency (`yield`) for the DB session instead of just opening/closing manually in each route?
3. What does `Base.metadata.create_all(bind=engine)` actually do?

#### 🎯 Key Takeaways
- Engine → connects. Session → operates. Base → defines table shape.
- `get_db()` with `yield` + `finally` is the idiomatic FastAPI+SQLAlchemy session-per-request pattern — memorize this exact shape, it recurs in every database-backed FastAPI project (including the capstone).

---

### 17. CRUD with SQLAlchemy

Now the same CRUD shape from Section 8 is rebuilt against a **real, persistent database** using the SQLAlchemy setup from Section 16.

#### 💻 Create (POST)

```python
@app.post("/todos")
def create_todo(title: str, db = Depends(get_db)):
    todo = Todo(title=title, completed=False)
    db.add(todo)
    db.commit()
    db.refresh(todo)
    return {"message": "Todo created", "data": todo}
```

- `db.add(todo)` stages the new object; `db.commit()` persists it; `db.refresh(todo)` reloads the object so auto-generated fields (like `id`) become available on the Python object.

#### 💻 Read — all and by ID

```python
@app.get("/todos")
def get_todos(db = Depends(get_db)):
    todos = db.query(Todo).all()
    return {"total": len(todos), "data": todos}

@app.get("/todos/{todo_id}")
def get_todo(todo_id: int, db = Depends(get_db)):
    todo = db.query(Todo).filter(Todo.id == todo_id).first()
    if not todo:
        raise HTTPException(status_code=404, detail="Todo not found")
    return todo
```

> 🎯 **The one query pattern to memorize:** `db.query(Model).filter(Model.field == value).first()`. This single idiom powers Read-by-ID, Update, and Delete across the entire course (and the capstone).

#### 💻 Update (PUT) — and the `db.refresh()` lesson

```python
@app.put("/todos/{todo_id}")
def update_todo(todo_id: int, title: str, db = Depends(get_db)):
    todo = db.query(Todo).filter(Todo.id == todo_id).first()
    if not todo:
        raise HTTPException(status_code=404, detail="Todo not found")
    todo.title = title
    db.commit()
    db.refresh(todo)
    return {"message": "Updated", "data": todo}
```

> ⚠ **Live-debugged gotcha (important, generalizable lesson):** immediately after `db.commit()`, the in-memory Python object can still show *stale* data until you call `db.refresh(todo)`. **Always `db.refresh(obj)` after a commit if you're about to return that same object**, or the client may see outdated values in the response even though the database itself is correct.

#### 💻 Delete (DELETE)

```python
@app.delete("/todos/{todo_id}")
def delete_todo(todo_id: int, db = Depends(get_db)):
    todo = db.query(Todo).filter(Todo.id == todo_id).first()
    if not todo:
        raise HTTPException(status_code=404, detail="Todo not found")
    db.delete(todo)
    db.commit()
    return {"message": "Todo deleted"}
```

#### 🪜 Full request lifecycle (SQLAlchemy-backed CRUD)

```text
Client Request
   ↓
Route + Depends(get_db) → fresh DB session opened
   ↓
db.query(Model).filter(...).first()  — fetch/check
   ↓
db.add() / mutate attribute / db.delete()
   ↓
db.commit()  — persist
   ↓
db.refresh(obj)  — reload fresh data (Create/Update)
   ↓
Response returned
   ↓
finally: db.close()  — session cleanup
```

#### 🔥 Interview Q&A
1. What's the end-to-end flow of a CRUD operation using SQLAlchemy?
2. Why is `.filter()` used, and what does `.first()` do?
3. **Difference between `commit` and `refresh`?** — `commit` persists the transaction to the database; `refresh` updates your in-memory Python object with the latest database state.
4. How does delete work in the ORM pattern?
5. What are `HTTPException`s used for in this flow?

#### 🎯 Key Takeaways
- SQLAlchemy lets you manipulate the database "like a Python object" — cleaner and more scalable than raw SQL at any real scale.
- The `filter(...).first()` → mutate/delete → `commit()` → `refresh()` sequence is the backbone pattern for every database-backed route you'll write, including the capstone's Blog API.

---

### 18. Asynchronous Programming (async/await)

#### 📖 Definitions

- **Synchronous (blocking):** tasks run strictly one after another; each must finish before the next starts. Slower under load.
- **Asynchronous (non-blocking):** multiple tasks can be *in flight* concurrently; no task is forced to sit and wait for another to finish. Faster and more scalable under load.
- Python's async programming uses **both** `async` and `await` together — `async def` defines an async function; `await` pauses *that* function (without blocking the whole program) until the awaited operation completes.

> ⚠ **Best practice / warning:** don't use `async` everywhere. Use it specifically for **I/O-bound** work — API calls, database calls, external service calls. For simple CPU-bound local calculations with no I/O wait, async adds nothing and shouldn't be used.

#### 💻 Synchronous vs. asynchronous — minimal comparison

```python
# Synchronous
import time

def task():
    time.sleep(3)
    return "Done"
```

```python
# Asynchronous
import asyncio

async def task():
    await asyncio.sleep(3)
    return "Done"
```

- `asyncio` is Python's own built-in async module — no `pip install` needed.

#### 💻 FastAPI route using async

```python
from fastapi import FastAPI
import asyncio

app = FastAPI()

@app.get("/")
async def home_function():
    await asyncio.sleep(3)
    return {"message": "Async API"}
```

> 🎯 **Why FastAPI leans on async:** so multiple requests can be handled concurrently — one slow request doesn't force every other request into a blocked, waiting state.

```mermaid
flowchart TB
    subgraph Synchronous
        S1[Request A runs fully] --> S2[Request B waits] --> S3[Request B runs]
    end
    subgraph Asynchronous
        A1[Request A starts] --> A3[Request A awaits I/O]
        A2[Request B starts while A awaits] --> A4[Request B awaits I/O]
        A3 --> A5[Both complete, interleaved]
        A4 --> A5
    end
```

#### 🔥 Interview Q&A
1. What is asynchronous programming? — A non-blocking approach letting multiple tasks progress without waiting on each other.
2. What do `async` and `await` each do?
3. Sync vs. async, in one line each? — Sync: blocking, sequential, simpler to reason about. Async: non-blocking, concurrent, better scalability under load.
4. Why does FastAPI use async? — Performance optimization: handling many simultaneous requests efficiently.

#### 🎯 Key Takeaways
- `async`/`await` are a matched pair — you rarely see one without the other in correct code.
- Reserve async for I/O-bound operations; don't reach for it on pure CPU-bound logic.

---

### 19. Basic JWT Authentication

#### 📖 Concept: Authentication and JWT

**Authentication** verifies who a user is and what they're allowed to access. **JWT (JSON Web Token)** is the token issued after a successful login, carrying the user's identity and metadata in a way the server can later verify without a database round-trip for every request.

#### 📖 JWT structure — three parts

| Part | Purpose |
|---|---|
| **Header** | Which hashing/signing algorithm is used |
| **Payload** | User-related claims (identity, expiry time, etc.) |
| **Signature** | The secret key + algorithm used to sign the token, so tampering can be detected |

```mermaid
sequenceDiagram
    participant U as User
    participant S as Server
    U->>S: Login (username + password)
    S->>S: Validate credentials
    S-->>U: JWT access_token (Header.Payload.Signature)
    U->>S: Subsequent request with token
    S->>S: Verify token (decode + check signature/expiry)
    S-->>U: Protected data (if valid) or 401 (if invalid)
```

#### 💻 Install and configure

```bash
pip install python-jose
```

> 📖 **JOSE** stands for *"JavaScript Object Signature and Encryption"* — a standard for securely signing/encrypting JSON data, which `python-jose` implements for Python.

```python
from fastapi import FastAPI, HTTPException, Depends, Header
from jose import jwt
from datetime import datetime, timedelta, timezone

app = FastAPI()

SECRET_KEY = "mysecret"
ALGORITHM = "HS256"
```

#### 🔍 Deep Dive: common JWT algorithms

| Algorithm | Notes |
|---|---|
| **HS256** | Simple shared secret key; fine for small apps (used in this course) |
| **HS512** | Shared secret key, stronger hashing |
| **RS256** | Public/private key pair; common in production |
| **ES256** | Advanced elliptic-curve cryptography; highly secure |

#### 💻 Creating a token

```python
def create_token(data: dict):
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + timedelta(minutes=30)
    to_encode.update({"exp": expire})
    token = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return token
```

> 🚀 **Best practice caught live by the instructor:** `datetime.utcnow()` is **deprecated** in modern Python. Use `datetime.now(timezone.utc)` instead. The instructor demonstrates pasting the deprecation warning into an AI assistant to get this exact fix — a sanctioned, explicitly-modeled debugging workflow.

#### 💻 Login (dummy/hardcoded credentials — demo only)

```python
@app.post("/login")
def login(username: str, password: str):
    if username != "admin" or password != "1234":
        raise HTTPException(status_code=401, detail="Invalid username and password")
    token = create_token({"sub": username})
    return {"access_token": token}
```

> ⚠ Explicitly flagged as **hardcoded/dummy credentials for demonstration only** — a real app checks against a database.

#### 💻 Verifying a token and protecting a route

```python
def verify_token(token: str = Header(None)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        return payload
    except Exception:
        raise HTTPException(status_code=401, detail="Invalid and Expired token")

@app.get("/secure")
def secure_data(user: dict = Depends(verify_token)):
    return {"message": "Secure data access", "user": user}
```

#### 🪜 End-to-end testing flow (Swagger UI)
1. Call `/login` with wrong credentials → `401`.
2. Call `/login` with correct credentials → receive `access_token`.
3. Copy the token; paste it into the `/secure` endpoint's token header field.
4. Execute → `200`, secure data + expiry info returned.
5. Tamper with the token (add garbage characters) → `401 "Invalid and Expired Token"`.

#### 🔥 Interview Q&A
1. What is authentication?
2. What is JWT, and how does token-based authentication work?
3. How many parts does a JWT have? — Header, Payload, Signature.
4. Why should a token have an expiry? — So it can't be misused indefinitely; short expiry windows limit the damage of a leaked token.

#### 🎯 Key Takeaways
- JWT = Header + Payload + Signature, created with `jwt.encode`, verified with `jwt.decode`.
- Always set an expiry claim (`exp`) on every token you issue.
- `datetime.now(timezone.utc)` is the modern, non-deprecated way to get the current UTC time for expiry calculations.

---

### 20. Production Authentication: OAuth2 + JWT + Password Hashing

#### 📖 Concept

This is the **production upgrade** to Section 19's basic JWT flow, combining three techniques:
1. **OAuth2** — a standard authorization *framework* (provides the token-based login/authorize flow, including Swagger's "Authorize" button support).
2. **JWT** — same token mechanics as before.
3. **Password hashing (bcrypt)** — so plaintext passwords are never stored or compared directly.

> ⚠ **Problem being solved:** storing plaintext passwords is a major security risk if the database ever leaks. Hashing + token-based auth is explicitly described as "fully secure," versus the earlier simple-JWT approach described as "a bit less secure."

#### 💻 Install

```bash
pip install python-jose
pip install "passlib[bcrypt]"
pip install python-multipart
```

> ⚠ **Syntax gotcha explicitly flagged:** `passlib[bcrypt]` must be written as **one bracketed dependency string**, not two separate package names — otherwise pip treats `passlib` and `bcrypt` as unrelated packages instead of `passlib`'s bcrypt "extra."
>
> `python-multipart` is required because `OAuth2PasswordRequestForm` reads form-encoded data, not JSON.

#### 💻 Setup block

```python
from fastapi import FastAPI, Depends, HTTPException
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from passlib.context import CryptContext
from jose import jwt, JWTError
from datetime import datetime, timedelta, timezone

app = FastAPI()

SECRET_KEY = "mysecret"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRY_MINUTES = 30

pwd_context = CryptContext(schemes=["bcrypt"])
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login")

fake_user_db = {
    "admin": {
        "username": "admin",
        "password": pwd_context.hash("1234")
    }
}
```

- `pwd_context = CryptContext(schemes=["bcrypt"])` — the conventional variable name; configures bcrypt as the hashing scheme.
- `OAuth2PasswordBearer(tokenUrl="login")` — tells FastAPI/Swagger that tokens are obtained at the `/login` endpoint, enabling the "Authorize" lock-icon flow.
- `fake_user_db` stands in for a real user table — explicitly flagged as a placeholder for teaching speed.

#### 💻 Hash & verify helpers

```python
def hash_password(password: str):
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str):
    return pwd_context.verify(plain_password, hashed_password)
```

#### 💻 Login using `OAuth2PasswordRequestForm`

```python
@app.post("/login")
def login(form_data: OAuth2PasswordRequestForm = Depends()):
    user = fake_user_db.get(form_data.username)
    if not user or not verify_password(form_data.password, user["password"]):
        raise HTTPException(status_code=401, detail="Invalid username or password")
    access_token = create_token({"sub": form_data.username})
    return {"access_token": access_token, "token_type": "bearer"}
```

> 🎯 **Key detail the instructor stresses:** you must include `"token_type": "bearer"` in the response — a standard requirement of the OAuth2 bearer-token flow, not optional decoration.

#### 💻 Verifying via `OAuth2PasswordBearer` (replaces the plain `Header` approach)

```python
def verify_token(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username = payload.get("sub")
        return username
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid or expired token")

@app.get("/protected")
def protected_route(username: str = Depends(verify_token)):
    return {"message": "Hello, you have access to this protected route", "username": username}
```

> 💡 With `Depends(oauth2_scheme)`, FastAPI/Swagger handles extracting the token from the request header automatically via the "Authorize" button — you no longer need a plain `Header(None)` parameter.

#### 🪜 Step-by-Step: production auth testing flow (Swagger UI)
1. `POST /login` with `admin` / `1234` (as **form fields**, not JSON — because of `OAuth2PasswordRequestForm`) → receive `access_token` + `"token_type": "bearer"`.
2. Try `GET /protected` **without authorizing first** → `"Not Authorized"`.
3. Click the **"Authorize"** lock icon in Swagger → enter `admin` / `1234` → "Authorize" → close the dialog.
4. `GET /protected` again → success message with the username.
5. Click **"Logout"** in the Authorize dialog → `GET /protected` again → access denied — confirming the whole gate actually works both ways.

```mermaid
flowchart LR
    A[POST /login<br/>OAuth2PasswordRequestForm] --> B{verify_password<br/>bcrypt check}
    B -- valid --> C[create_token - JWT]
    B -- invalid --> D[401 Invalid username or password]
    C --> E[access_token + token_type=bearer]
    E --> F[Client stores token]
    F --> G[GET /protected<br/>Depends oauth2_scheme]
    G --> H{jwt.decode valid?}
    H -- yes --> I[200 protected data]
    H -- no --> J[401 Invalid or expired token]
```

#### 🔥 Interview Q&A
1. What is OAuth2? — An authorization framework that provides a standard token-based authorization flow.
2. Why hash passwords? — So plaintext passwords are never stored or exposed, even in a data breach.
3. What's the role of JWT here? — Authenticating users and securing API access after login.
4. How do you validate a token? — `jwt.decode()` inside a try/except, checking signature and expiry.
5. What is `OAuth2PasswordBearer`, and why use it over a plain header? — It standardizes token extraction from the `Authorization` header and plugs directly into Swagger's "Authorize" UI.

#### 🎯 Key Takeaways
- Production auth = OAuth2 (flow/standardization) + JWT (token mechanics) + bcrypt (password hashing) — three complementary layers, not competing options.
- `passlib[bcrypt]` must be installed as one bracketed string.
- `jwt.decode(..., algorithms=[ALGORITHM])` takes a **list** of algorithms, even when you only use one — a subtle but real API detail worth remembering (see also the Capstone's flagged inconsistency in Section 31).

---

### 21. File Upload and Static Files

#### 📖 Concept

FastAPI can accept uploaded files from a client and also serve files back out as **static assets** (e.g., so an uploaded image can be viewed via a plain URL).

#### 💻 Imports and folder setup

```python
from fastapi import FastAPI, UploadFile, File, HTTPException
from fastapi.staticfiles import StaticFiles
import os
import shutil

app = FastAPI()

UPLOAD_DIRECTORY = "uploads"

if not os.path.exists(UPLOAD_DIRECTORY):
    os.makedirs(UPLOAD_DIRECTORY)

app.mount("/files", StaticFiles(directory=UPLOAD_DIRECTORY), name="files")
```

- `shutil` is Python's built-in module for file/folder operations (copy, move, delete).
- `app.mount(path, StaticFiles(directory=...), name=...)` publishes the upload folder's contents at a public URL path (`/files/<filename>`).

#### 💻 Upload endpoint

```python
@app.post("/upload")
def upload_file(file: UploadFile = File(...)):
    file_name = file.filename
    file_path = os.path.join(UPLOAD_DIRECTORY, file_name)

    if not file_name:
        raise HTTPException(status_code=400, detail="File not selected")

    with open(file_path, "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)

    return {
        "message": "File uploaded successfully",
        "file_name": file_name,
        "file_url": f"http://127.0.0.1:8000/files/{file_name}"
    }
```

- `UploadFile = File(...)` is the FastAPI idiom for accepting an uploaded file.
- Files are saved in **binary write mode** (`"wb"`); `shutil.copyfileobj(file.file, buffer)` streams the uploaded bytes to disk.

#### 💻 Retrieve-by-filename endpoint

```python
@app.get("/files/{file_name}")
def get_file(file_name: str):
    file_path = os.path.join(UPLOAD_DIRECTORY, file_name)
    if not os.path.exists(file_path):
        raise HTTPException(status_code=404, detail="File not found")
    return f"http://127.0.0.1:8000/files/{file_name}"
```

#### 🪜 Testing flow
1. `POST /upload` with a chosen file (e.g., `demo.png`) → get back a `file_url`.
2. Open that URL directly in the browser → confirms the uploaded image renders, validating the static mount.
3. `GET /files/demo.png` → returns the same URL, confirming the lookup path too.

#### 🔥 Interview Q&A
1. How do you upload a file in FastAPI?
2. What are static files? — Utility assets (images, CSS, JS) served directly from the server to the frontend.

#### 🎯 Key Takeaways
- `UploadFile = File(...)` + `shutil.copyfileobj` is the standard upload pattern.
- `app.mount(...)` with `StaticFiles` is how you turn a server-side folder into a public, browsable URL path.

---

### 22. CORS (Cross-Origin Resource Sharing)

#### 📖 Concept

**CORS** is a **browser security policy**: if a frontend and backend run on different *origins* (different host/port combinations — e.g., frontend on `localhost:5173`, backend on `localhost:8000`), the browser blocks the request by default until the backend explicitly grants permission.

> ⚠ Symptom: a "Blocked by CORS policy" error in the browser console, even though the backend itself works fine when tested directly (e.g., via Swagger).

#### 💻 Backend CORS configuration

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost:5173",  # frontend URL
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.get("/")
def home():
    return {"message": "CORS Enable API"}
```

| Parameter | Purpose |
|---|---|
| `allow_origins` | Which frontend URL(s) may call this backend |
| `allow_credentials=True` | Required to allow cookies/auth headers cross-origin |
| `allow_methods=["*"]` | Allow all HTTP methods (GET/POST/PUT/DELETE) |
| `allow_headers=["*"]` | Allow all request headers |

> 🚀 **Best practice:** in a real project, `allow_origins` normally lives in a config/`.env` file, not hardcoded — see Section 23.

#### 💻 Minimal React frontend making the cross-origin call

```jsx
import React, { useState, useEffect } from "react";

export default function App() {
  const [data, setData] = useState();

  useEffect(() => {
    fetch("http://localhost:8000/")
      .then((res) => res.json())
      .then((data) => setData(data))
      .catch((err) => console.log(err));
  }, []);

  return (
    <div style={{ padding: "20px" }}>
      <p>{data?.message}</p>
    </div>
  );
}
```

- Verified via browser DevTools → Network tab: the GET request to port 8000 succeeds while the frontend itself runs on port 5173.

#### 🔥 Interview Q&A (explicitly flagged by the instructor as common)
1. What is CORS, and why does the CORS error occur?
2. How do you enable CORS in FastAPI?
3. What does `allow_origins` control?

#### 🎯 Key Takeaways
- CORS errors are a **browser-enforced** security boundary, not a backend bug per se — but the backend must explicitly whitelist allowed origins.
- `CORSMiddleware` with `allow_origins`, `allow_credentials`, `allow_methods`, `allow_headers` is the full configuration surface you need for most frontend integrations.

---

### 23. Environment Variables and Config Management

#### 📖 Concept

Secrets (API keys, database URLs, JWT secret keys) should **never** be hardcoded directly in source files, because:
- Code gets pushed to GitHub (public or shared) — hardcoded secrets leak.
- Secret values differ between environments (dev vs. production).
- Centralizing secrets in one place makes rotation easy.
- Separating secrets from code keeps the codebase clean.

#### 💻 Install and basic usage

```bash
pip install python-dotenv
```

`.env` file:
```
ORIGINS=http://localhost:5173
SECRET_KEY=my secret
DB_URL=postgresql://...
```

```python
import os
from dotenv import load_dotenv

load_dotenv()

SECRET_KEY = os.getenv("SECRET_KEY")
DB_URL = os.getenv("DB_URL")
origins = [os.getenv("ORIGINS")]
```

- `load_dotenv()` loads the `.env` file's contents into the process's environment; `os.getenv("KEY")` reads individual values.

#### 💻 Scaling up: a `config.py` / `Settings` pattern for larger projects

```python
# config.py
import os
from dotenv import load_dotenv
load_dotenv()

class Settings:
    ORIGINS = os.getenv("ORIGINS")
    SECRET_KEY = os.getenv("SECRET_KEY")
    DB_URL = os.getenv("DB_URL")

settings = Settings()
```

```python
# main.py
from config import settings

origins = [settings.ORIGINS]
```

> 🚀 **Best practice:** always add `.env` to `.gitignore` so secrets never get pushed to GitHub. Knowing Git well enough to manage this is explicitly called out as important.

#### 🔥 Interview Q&A
1. What is an environment variable, and what is `.env` used for?
2. What does `os.getenv()` do?
3. Why are environment variables considered secure? — Because they live outside the codebase and are never shared via version control.
4. What is configuration management? — Centralizing app settings for readability, reuse, and safety.

#### 🎯 Key Takeaways
- `.env` + `python-dotenv` + `os.getenv()` is the minimum-viable secrets pattern.
- A `Settings` class in `config.py` is the natural next step once a project grows past a handful of secrets.
- `.gitignore` your `.env` file — always.

---

### 24. Testing FastAPI Apps with pytest

#### 📖 Why testing matters

Untested APIs can silently break, crash in production, or regress when new code is added. Automated tests give repeatable, environment-independent confidence that endpoints behave correctly.

#### 💻 Install

```bash
pip install pytest
pip install httpx   # sometimes needed as a TestClient dependency
```

#### 💻 Sample app (`main.py`)

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"message": "Hello Mohit"}

@app.get("/add")
def add(a: int, b: int):
    return {"result": a + b}
```

#### 💻 Tests (`test_main.py`)

```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_home_api():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "Hello Mohit"}

def test_add():
    response = client.get("/add?a=5&b=3")
    assert response.status_code == 200
    assert response.json() == {"result": 8}
```

Run with:

```bash
pytest
```

- `TestClient` simulates real HTTP calls against your app, in-process, without a running server.
- `assert` checks that the actual result matches the expected one; a mismatch fails the test with a clear diff.

> 🧪 **Demonstrated live:** deliberately setting an expected value wrong (8 → 7) and re-running `pytest` shows a clear failure report — a good way to build trust that your tests are actually checking something.

**Suggested next step (explicitly assigned by the instructor):** write equivalent tests for your full CRUD and authentication endpoints using this same pattern.

#### 🔥 Interview Q&A
1. Why is automated testing important for APIs?
2. What is `TestClient`, and how does it differ from manually hitting endpoints via Swagger?
3. What does `assert` do in a test function?

#### 🎯 Key Takeaways
- `TestClient(app)` + plain `assert` statements are enough to start testing any FastAPI app.
- Test both the success path (`status_code == 200`, correct body) and representative failure paths.

---

### 25. Third-Party API Integration

#### 📖 Concept

Your FastAPI backend can act as a **proxy/aggregator** in front of another API — fetching external data and reshaping, filtering, or error-handling it before returning it to your own client.

#### 💻 Install

```bash
pip install requests
```

#### 💻 Plain Python demo (JSONPlaceholder mock API)

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/posts")
data = response.json()
```

#### 💻 FastAPI integration — all data, and single item by path param

```python
from fastapi import FastAPI, HTTPException
import requests

app = FastAPI()

@app.get("/posts")
def get_posts():
    url = "https://jsonplaceholder.typicode.com/posts"
    response = requests.get(url)
    return response.json()

@app.get("/posts/{post_id}")
def get_post(post_id: int):
    url = f"https://jsonplaceholder.typicode.com/posts/{post_id}"
    response = requests.get(url)
    if response.status_code != 200:
        raise HTTPException(status_code=404, detail="page not found")
    return response.json()
```

#### 🔥 Interview Q&A
1. What is API integration? — When one API calls another API to fetch and reuse its data.
2. What role does the `requests` library play? — Sends outbound HTTP requests.
3. Why call `.json()` on the response? — Converts the raw HTTP response into a usable Python object.
4. Why use third-party APIs at all? — To leverage an existing service instead of building that capability yourself.

#### 🎯 Key Takeaways
- `requests.get(url).json()` is the baseline pattern for consuming any external REST API from inside FastAPI.
- Always check `response.status_code` before trusting the payload, and translate failures into clean `HTTPException`s for your own clients.

---

### 26. Web Scraping with BeautifulSoup

#### 📖 Concept

**Web scraping/crawling** extracts data directly from a website's HTML when no API is available.

> ⚠ **Explicit legal/ethical caveat from the instructor:** not every website permits scraping — check a site's terms/robots policy before scraping it. The instructor states he used a specific news site purely for teaching purposes and removed the resulting endpoint afterward.

#### 💻 Install

```bash
pip install beautifulsoup4   # import name: bs4
```

#### 💻 Minimal scraping demo

```python
import requests
from bs4 import BeautifulSoup

url = "http://example.com"
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")
print(soup.title.text)
```

#### 💻 FastAPI integration — scraping headlines

```python
from fastapi import FastAPI
import requests
from bs4 import BeautifulSoup

app = FastAPI()

@app.get("/news")
def get_news():
    url = "https://<news-site-url>"
    response = requests.get(url)
    soup = BeautifulSoup(response.text, "html.parser")
    title = []
    for item in soup.find_all("a", class_="top-block-news"):
        title.append(item.text)
    return {"news": title}
```

- Browser **DevTools → Inspect** is used to find the exact HTML tag and CSS class holding the target content.
- `soup.find_all(tag, class_=...)` returns every matching element; `.text` extracts the visible text from each.

#### 🔥 Interview Q&A
1. What is web crawling/scraping?
2. What does BeautifulSoup do? — Parses HTML so you can navigate/query it programmatically.
3. What's the role of `requests` here? — Fetches the raw HTML that BeautifulSoup then parses.

#### 🎯 Key Takeaways
- Scraping = `requests` (fetch HTML) + `BeautifulSoup` (parse/query HTML).
- Always confirm a site legally/ethically allows scraping before building on it — this is both a legal consideration and an interview-relevant professionalism point.

---

### 27. Pagination

#### 📖 Concept

If an endpoint has hundreds or thousands of records, returning them all at once is slow and wasteful. **Pagination** splits results into pages (e.g., 5 or 10 items at a time).

#### 💻 Core pagination logic

```python
@app.get("/news")
def get_news(page: int = 1, limit: int = 5):
    # ... assume `title` is the full list of items to paginate ...

    start = (page - 1) * limit
    end = start + limit

    return {
        "page": page,
        "limit": limit,
        "total": len(title),
        "data": title[start:end],
    }
```

> 🎯 **The formula to memorize:** `start = (page - 1) * limit`, `end = start + limit`. This same arithmetic reappears, slightly adapted for SQL, in the capstone's `offset(start).limit(limit)` (Section 31).

**Demonstrated behavior:**
- `page=1, limit=5` → first 5 items, `total` reflects the full dataset size.
- `page=2, limit=6` → a *different* slice of 6 items (not overlapping with page 1).

#### 🔥 Interview Q&A
1. What is pagination, and why is it used?
2. (Commonly asked to implement live): write the `start`/`end` offset math from scratch.

#### 🎯 Key Takeaways
- Pagination is a generic slicing pattern — it applies equally to in-memory lists, scraped data, or SQL query results.
- Always return `page`, `limit`, and `total` alongside the data slice so the frontend can build pagination controls.

---

### 28. Caching

#### 📖 Concept

Caching stores the result of an expensive operation (like a web scrape or slow external API call) so a repeated request for the same data returns almost instantly instead of re-running the expensive work.

#### 💻 Simple TTL-style in-memory cache

```python
import time

cache_data = []
last_fetch = 0

@app.get("/news")
def get_news():
    global cache_data, last_fetch
    start = time.time()

    if time.time() - last_fetch >= 60:
        # ... fetch fresh data (e.g., scrape or API call) ...
        cache_data = [...]
        last_fetch = time.time()
    else:
        pass  # reuse cache_data as-is

    end = time.time()
    return {"time_taken": round(end - start, 4), "data": cache_data[:5]}
```

- `last_fetch` tracks when data was last refreshed; `time.time() - last_fetch >= 60` is a simple **Time To Live (TTL)** check — refresh only if more than 60 seconds have elapsed.
- `global` is needed so these variables persist their updated values *across* separate requests (not just within one function call).

**Demonstrated performance difference:** first (fresh) call ≈ 4.8 seconds; a repeated call within the 60-second window ≈ 0.0001 seconds — several orders of magnitude faster.

#### 🔥 Interview Q&A
1. What is caching, and what problem does it solve?
2. What is **TTL (Time To Live)**? How do you configure it, and why use it? — TTL is the duration cached data stays valid before being refreshed; configured by comparing `time.time()` against a stored "last fetched at" timestamp; used to balance freshness against performance.

#### 🎯 Key Takeaways
- A cache is only as good as its invalidation strategy — TTL is the simplest one.
- `global` variables are a quick way to persist cache state across requests in a single-process demo; a production system would typically use Redis or a similar store instead.

---

### 29. Rate Limiting

#### 📖 Concept

**Rate limiting** protects an API from spam/abuse by capping how many times a client can call an endpoint in a given time window (e.g., 5 requests/minute).

#### 💻 Install

```bash
pip install slowapi
```

#### 💻 Setup and usage

```python
from fastapi import FastAPI, Request
from slowapi import Limiter
from slowapi.util import get_remote_address
from slowapi.errors import RateLimitExceeded
from fastapi.responses import JSONResponse

app = FastAPI()

limiter = Limiter(key_func=get_remote_address)
app.state.limiter = limiter

@app.exception_handler(RateLimitExceeded)
def rate_limit_handler(request: Request, exc: RateLimitExceeded):
    return JSONResponse(
        status_code=429,
        content={"detail": "Too Many Requests"},
    )

@app.get("/data")
@limiter.limit("5/minute")
def get_data(request: Request):
    return {"message": "success"}
```

- `get_remote_address` tracks limits **per client IP**, not globally across all users.
- The rate-limited route must accept `request: Request` as a parameter — a `slowapi` requirement.
- `429 Too Many Requests` is the conventional status code for exceeding a rate limit.

**Demonstrated behavior:** calls 1–5 within a minute succeed; call 6 within the same window returns the custom `429` error.

#### 🔥 Interview Q&A
1. What is rate limiting, and why should it be used?
2. What does the `429` status code mean?

#### 🎯 Key Takeaways
- `slowapi` + `Limiter(key_func=get_remote_address)` + `@limiter.limit("N/period")` is the standard production pattern for per-client rate limiting in FastAPI.
- A custom `RateLimitExceeded` handler lets you return a clean, consistent error body instead of a generic one.

---

### 30. Deployment to Render

#### 📖 Concept

**Render.com** is a free-tier-friendly hosting platform for deploying a FastAPI app straight from a GitHub repository.

#### 🪜 Step-by-step: preparing a project for deployment

1. **Virtual environment:** create and activate (`python3 -m venv venv`, then `source venv/bin/activate` / `venv\Scripts\activate`). Open a fresh terminal after activating.
2. **Install dependencies:** `pip install fastapi uvicorn` (plus whatever else the project needs — `python-dotenv`, auth libraries, etc.).
3. **Freeze dependencies:**
   ```bash
   pip freeze > requirements.txt
   ```
   This is required so Render (or any host) knows exactly what to install. Double-check this command's spelling.
4. **Create `.gitignore`**, excluding at minimum: `venv/`, `.env`, `__pycache__/`. Without this, secrets and bulky/irrelevant folders get pushed to GitHub.

#### 🪜 Step-by-step: Git + GitHub push

```bash
git init
git add .
deactivate              # make sure you're out of the venv before committing, if needed
git add .
git commit -m "updated code"
git remote add origin <repo-url>
git push origin main
```

- Confirm on GitHub that `requirements.txt`, `main.py`, `.gitignore` are present, and that `venv/`, `.env`, `__pycache__/` are correctly excluded.

#### 🪜 Step-by-step: deploying on Render.com

1. Sign up/log in → Dashboard → **New → Web Service**.
2. Choose **Public Git repository**, paste the GitHub URL, click **Connect**.
3. Render auto-detects Python 3 and the build command (installs from `requirements.txt`).
4. **Manually set the Start Command**, e.g.:
   ```
   uvicorn main:app --host 0.0.0.0 --port 10000
   ```
   (`0.0.0.0` so the app accepts external connections; pick any open port, e.g. `10000`.)
5. Add any needed **Environment Variables** on the pre-deploy page (don't forget this step).
6. Select the **Free** tier plan.
7. Click **Deploy Web Service** and wait through the "Preparing → Starting → Injecting environment variable" stages (slower on the free tier).
8. Once "Live," verify both the root route and `/docs` on the live Render URL.

```mermaid
flowchart LR
    A[Local Project] -->|pip freeze > requirements.txt| B[requirements.txt]
    A -->|git push| C[(GitHub Repo)]
    C -->|Connect| D[Render Web Service]
    D -->|Build: pip install -r requirements.txt| E[Build]
    E -->|Start Command: uvicorn main:app --host 0.0.0.0 --port PORT| F[Live App]
```

#### ⚠ Common Mistakes & Troubleshooting

| Problem | Cause | Solution |
|---|---|---|
| Secrets/`venv` pushed to GitHub | Missing or incomplete `.gitignore` | Add `venv/`, `.env`, `__pycache__/` *before* the first commit |
| Render build succeeds but app won't start | Missing/incorrect Start Command | Explicitly set `uvicorn main:app --host 0.0.0.0 --port <PORT>` |
| App works locally but not after deploy | Missing environment variables on Render | Add them on the deploy page before deploying |

#### 🔥 Interview Q&A
1. Why is `requirements.txt` needed for deployment?
2. Why must `.env` and `venv` be excluded from version control?
3. Why does the Render start command need `--host 0.0.0.0`? — To accept external connections, not just `localhost`.

#### 🎯 Key Takeaways
- `pip freeze > requirements.txt` + a correct `.gitignore` are the two things you must get right before any deploy.
- Hosting platforms like Render need an explicit Uvicorn start command — it isn't always auto-inferred.

---

### 31. Capstone Project: The Blog API

This is the course's final, consolidating project — a production-style **Blog API** built on **PostgreSQL**, pulling together nearly every concept from the course. It's structured in five parts, exactly as the instructor builds it.

```mermaid
flowchart TD
    P1[Part 1: PostgreSQL + FastAPI Setup] --> P2[Part 2: CRUD API]
    P2 --> P3[Part 3: JWT Authentication]
    P3 --> P4[Part 4: Pagination + Search]
    P4 --> P5[Part 5: Push to GitHub]
```

#### 31.1 Part 1 — PostgreSQL Setup and FastAPI Project Setup

**Why PostgreSQL here:** once you've set up *any* one real database (SQLite, MySQL, Postgres), the rest of the course transfers directly — the instructor picks Postgres specifically for this capstone to show a genuinely production-grade database.

**Installing PostgreSQL + pgAdmin (two paths):**
- **Official installer** (postgresql.org) — standard GUI installer, optionally set a password during setup.
- **Homebrew (Mac):** `brew services start postgresql` after installing via brew (version 16 in the demo); `brew services list` to confirm it's running.
- **pgAdmin** — a GUI tool for Postgres, used throughout to visually confirm tables/rows alongside the API's own behavior.

**Creating the database (two equivalent methods):**

```sql
-- Method 1: terminal (psql CLI)
-- $ psql postgres
CREATE DATABASE blog_db;
\l   -- list all databases to confirm
```

- **Method 2:** pgAdmin GUI → right-click **Databases** → Create → Database → name it `blog_db`.

**Project dependencies:**

```bash
pip install fastapi uvicorn sqlalchemy psycopg2-binary python-dotenv
```

- `psycopg2-binary` is the PostgreSQL driver SQLAlchemy needs under the hood (new vs. the SQLite examples earlier in the course).

**`database.py` — connection setup:**

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, declarative_base

DATABASE_URL = "postgresql://postgres:password@localhost/blog_db"

engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(bind=engine)
Base = declarative_base()
```

> ⚠ **Explicitly flagged anti-pattern:** hardcoding the real DB URL (with username/password) directly in source, as shown here, should **not** be pushed to GitHub in a real project — it belongs in `.env` (Section 23). The instructor does it this way only for teaching speed and explicitly disclaims it on screen.

**`models.py` — the `Blog` table:**

```python
from sqlalchemy import Column, Integer, String, Text
from database import Base

class Blog(Base):
    __tablename__ = "blogs"

    id = Column(Integer, primary_key=True, index=True)
    title = Column(String)
    content = Column(Text)
```

**`main.py` — app init, table creation, sanity route:**

```python
from fastapi import FastAPI
from database import engine
import models

models.Base.metadata.create_all(bind=engine)

app = FastAPI()

@app.get("/")
def home():
    return {"message": "Blog API started"}
```

- Verified in pgAdmin: `blog_db` → Schemas → `public` → Tables → `blogs`, with columns `id`, `title`, `content` exactly matching the model.

#### 31.2 Part 2 — CRUD API

**`schemas.py` — Pydantic input/output schemas:**

```python
from pydantic import BaseModel

class BlogCreate(BaseModel):
    title: str
    content: str

class BlogResponse(BaseModel):
    id: int
    title: str
    content: str

    class Config:
        from_attributes = True
```

> 📌 **Remember:** `from_attributes = True` is **Pydantic v2's** replacement for the older `orm_mode = True` — it lets a Pydantic model read values directly off a SQLAlchemy ORM object's attributes, which is essential for any `response_model` that wraps an ORM object.

**DB dependency (same `get_db` pattern as Section 16/17, reused without modification):**

```python
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.orm import Session
from database import engine, SessionLocal
import models, schemas

models.Base.metadata.create_all(bind=engine)
app = FastAPI()

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

**Create:**

```python
@app.post("/blogs", response_model=schemas.BlogResponse)
def create_blog(blog: schemas.BlogCreate, db: Session = Depends(get_db)):
    new_blog = models.Blog(title=blog.title, content=blog.content)
    db.add(new_blog)
    db.commit()
    return new_blog
```

> ⚠ **Flagged gap vs. idiomatic SQLAlchemy:** this version skips `db.refresh(new_blog)` before returning — the Section 17 lesson. It may still work depending on session configuration, but the idiomatic, safest version adds `db.refresh(new_blog)` right after `db.commit()`.

**Read all / Read one:**

```python
@app.get("/blogs", response_model=list[schemas.BlogResponse])
def get_blogs(db: Session = Depends(get_db)):
    return db.query(models.Blog).all()

@app.get("/blog/{id}", response_model=schemas.BlogResponse)
def get_blog(id: int, db: Session = Depends(get_db)):
    blog = db.query(models.Blog).filter(models.Blog.id == id).first()
    if not blog:
        raise HTTPException(status_code=404, detail="Blog not found")
    return blog
```

**Update:**

```python
@app.put("/blogs/{id}", response_model=schemas.BlogResponse)
def update_blog(id: int, blog: schemas.BlogCreate, db: Session = Depends(get_db)):
    existing_blog = db.query(models.Blog).filter(models.Blog.id == id).first()
    if not existing_blog:
        raise HTTPException(status_code=404, detail="Blog not found")

    existing_blog.title = blog.title
    existing_blog.content = blog.content
    db.commit()
    return existing_blog
```

**Delete:**

```python
@app.delete("/blogs/{id}")
def delete_blog(id: int, db: Session = Depends(get_db)):
    blog = db.query(models.Blog).filter(models.Blog.id == id).first()
    if not blog:
        raise HTTPException(status_code=404, detail="Blog not found")

    db.delete(blog)
    db.commit()
    return {"message": "Blog deleted successfully"}
```

- Testing mixed both Swagger UI ("Try it out") *and* direct row creation/inspection via pgAdmin's "View/Edit All Rows" — a good habit: verify at both the API layer and the database layer.

#### 31.3 Part 3 — JWT Authentication

**Dependencies:**

```bash
pip install "python-jose[cryptography]" "passlib[bcrypt]" python-multipart
```

**`auth.py` — all JWT logic in its own file (a clean separation-of-concerns habit worth copying):**

```python
from jose import jwt, JWTError
from datetime import datetime, timedelta, timezone
from fastapi import HTTPException, Depends
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm

SECRET_KEY = "mysecret"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRY_MINUTES = 30

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login")

def create_token(data: dict):
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + timedelta(minutes=ACCESS_TOKEN_EXPIRY_MINUTES)
    to_encode.update({"exp": expire})
    token = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return token

def verify_token(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        return payload
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")
```

**Login (simplified in this part of the capstone):**

```python
@app.post("/login")
def login(form_data: OAuth2PasswordRequestForm = Depends()):
    access_token = create_token({"sub": "user", "admin": True, "type": "bearer"})
    return {"access_token": access_token, "token_type": "bearer"}
```

> ⚠ **Explicitly flagged simplification:** this capstone login endpoint does **not** actually verify `form_data.username`/`form_data.password` against a real user table — it issues a token unconditionally. This is a deliberate teaching shortcut, not a production-ready login. A real version should look up the user (as in Section 20's `fake_user_db` pattern, or better, a real `users` table) and call `verify_password(...)` before issuing any token.

**Protecting routes:** add `user = Depends(verify_token)` as an extra parameter to any endpoint that needs auth. Applied in the capstone to **Create**, **Update**, and **Delete** — **Read endpoints were deliberately left public**.

```python
@app.post("/blogs", response_model=schemas.BlogResponse)
def create_blog(blog: schemas.BlogCreate, db: Session = Depends(get_db), user = Depends(verify_token)):
    ...
```

**Testing flow:** `POST /login` → copy token → Swagger "Authorize" → confirm Create/Delete succeed while authorized and fail with "Not Authenticated" while logged out; confirm GET endpoints keep working either way.

#### 31.4 Part 4 — Pagination and Search

```python
from fastapi import Query

@app.get("/blogs", response_model=list[schemas.BlogResponse])
def get_blogs(
    page: int = 1,
    limit: int = 5,
    search: str = Query(default=""),
    db: Session = Depends(get_db),
):
    query = db.query(models.Blog)

    if search:
        query = query.filter(models.Blog.title.ilike(f"%{search}%"))

    total = query.count()
    start = (page - 1) * limit
    blogs = query.offset(start).limit(limit).all()

    return {
        "page": page,
        "limit": limit,
        "total": total,
        "data": blogs,
    }
```

- **`ilike()`** = SQLAlchemy's **case-insensitive LIKE** — essential for user-facing search.
- The `%{search}%` wildcard wrapping means "contains," not "exactly equals."
- SQL-level pagination: `query.offset(start).limit(limit)` mirrors the same `start`/`limit` math from Section 27, now expressed as SQL rather than a Python list slice.

> ⚠ **Live-debugged mistake (explicitly called out, great real-world lesson):** the first version of this filter forgot the `%` wildcards around `search`, so `ilike("Ram")` only matched an *exact* (case-insensitive) title, not a title that merely *contains* "Ram" — silently returning zero results for a seemingly correct search term. Always double-check wildcard placement when building `LIKE`/`ILIKE` filters.

> ⚠ **Flagged code-correctness inconsistency (worth noticing in a code review or interview):** the route's decorator still says `response_model=list[schemas.BlogResponse]` (a bare list), but the function now actually returns a **dict** (`{page, limit, total, data}`). In a stricter FastAPI setup this mismatch would need fixing — either remove `response_model` here, or define a dedicated `PaginatedBlogResponse` schema that matches the real return shape.

#### 31.5 Part 5 — Pushing to GitHub

```bash
git add .
git commit -m "updated code"
git push origin blog-api-project
```

- Pushed to a **dedicated branch** (`blog-api-project`), separate from `main`, consistent with the course's "one branch per lecture/project" convention used throughout.

#### 🎯 Key Takeaways (Capstone)
- The capstone is best understood as "Section 16/17's SQLAlchemy CRUD pattern" + "Section 20's OAuth2/JWT pattern" + "Section 27's pagination pattern," all pointed at one real PostgreSQL-backed resource (`Blog`).
- Three explicitly-flagged rough edges are worth remembering for your own code (and for interviews, where "what would you fix in this code?" is a realistic question):
  1. Missing `db.refresh()` after `db.add()` + `db.commit()` in Create.
  2. A login endpoint that issues tokens without actually checking credentials.
  3. A `response_model` left pointing at the wrong shape after the endpoint's return value changed to support pagination.
- Testing discipline that generalizes well: verify every change *both* through Swagger UI *and* directly in the database (pgAdmin), and build/test endpoints incrementally rather than all at once.

---

## Glossary

| Term | Definition | Why It Matters |
|---|---|---|
| API | Application Programming Interface — the bridge between frontend and backend | Foundation of all backend work |
| Route | A URL path mapped to a handler function | How FastAPI dispatches requests |
| Path Parameter | A dynamic value embedded in the URL path | Identifies a specific resource |
| Query Parameter | A `key=value` pair after `?` in a URL | Filtering, searching, pagination inputs |
| Request Body | Data sent by the client, usually as JSON | Carries payload for POST/PUT operations |
| Pydantic | Data validation/schema library used by FastAPI | Automatic validation and docs generation |
| BaseModel | Pydantic's base class for defining schemas | Turns a plain class into a validated schema |
| Nested Model | A Pydantic model containing another Pydantic model | Models real-world nested JSON cleanly |
| CRUD | Create, Read, Update, Delete | The four fundamental data operations |
| Response Model | Defines the exact shape of data sent back to the client | Hides sensitive fields, validates output |
| HTTPException | FastAPI's built-in exception for returning clean HTTP errors | Standard way to signal client-facing errors |
| Global Exception Handler | A centralized handler for a specific exception type, app-wide | Avoids duplicated error-handling code |
| Dependency Injection | Supplying a function's required logic automatically via `Depends` | Enables reusable, DRY route logic |
| Middleware | A function that wraps every request/response globally | Logging, timing, CORS, cross-cutting concerns |
| SQLite | A lightweight, file-based database built into Python | Good for small apps, zero extra install |
| SQLAlchemy | A Python ORM that maps classes to database tables | Industry-standard DB access in FastAPI |
| ORM | Object Relational Mapper — DB access via Python objects instead of raw SQL | Cleaner, more maintainable DB code |
| Engine | SQLAlchemy's database connection object | The low-level link to the actual DB |
| Session | A SQLAlchemy object used to perform DB operations | Created/closed per request via `get_db()` |
| `async`/`await` | Python keywords for non-blocking concurrent execution | Lets FastAPI handle many requests at once |
| JWT | JSON Web Token — a signed token proving identity after login | Core mechanism for stateless authentication |
| OAuth2 | An authorization framework standardizing login/token flows | Powers Swagger's "Authorize" button and production auth |
| bcrypt | A password-hashing algorithm | Ensures passwords are never stored in plaintext |
| CORS | Cross-Origin Resource Sharing — a browser security policy | Must be configured for frontend-backend integration |
| `.env` file | A file holding secret/config values outside the codebase | Keeps secrets out of version control |
| `TestClient` | FastAPI's tool for simulating HTTP calls in tests | Enables automated endpoint testing with pytest |
| Pagination | Splitting large result sets into pages | Keeps large-dataset APIs fast and usable |
| TTL (Time To Live) | How long cached data stays valid before refreshing | Core caching-invalidation concept |
| Rate Limiting | Capping how often a client can call an endpoint | Protects APIs from spam/abuse |
| Static Files | Server-hosted assets (images, CSS, JS) served directly to clients | Needed for file upload/serving features |
| `ilike` | SQLAlchemy's case-insensitive SQL `LIKE` | Core building block for user-facing search |

---

## Revision Notes (One-Minute Summary)

- An **API** connects frontend and backend via JSON; **FastAPI** adds automatic validation, docs, and async support on top.
- **Path params** identify a resource; **query params** filter/sort/paginate; **request body** (validated by **Pydantic**, including **nested models**) carries the actual payload.
- **CRUD** = Create/Read/Update/Delete, first built on an in-memory list, then rebuilt on **SQLAlchemy** (`engine` → `SessionLocal` → `Base` → `get_db()` dependency → `filter(...).first()` as the core query idiom).
- **Response Models** control exactly what leaves your API (hide passwords/secrets); **status codes** + **HTTPException** + **global exception handlers** make errors clean and centralized.
- **Dependency Injection** (`Depends`) reuses logic per-route; **Middleware** wraps *every* request/response globally — know the difference.
- **Async/await** help specifically with I/O-bound concurrency, not raw CPU work.
- **Auth** progresses from basic JWT → production-grade **OAuth2 + JWT + bcrypt password hashing**, with `jwt.encode`/`jwt.decode`, token expiry, and Swagger's "Authorize" flow.
- **File upload** uses `UploadFile` + `shutil`; **CORS** is a browser rule configured via `CORSMiddleware`; **secrets** belong in `.env` + a `Settings`/config pattern, never hardcoded.
- **Testing** uses `TestClient` + `pytest` + `assert`.
- **Third-party integration** (`requests`), **scraping** (`BeautifulSoup`), **pagination** (`(page-1)*limit` math), **caching** (TTL check), and **rate limiting** (`slowapi`) are all composable backend utility skills.
- **Deployment** needs `requirements.txt`, a correct `.gitignore`, and an explicit Uvicorn start command on the host.
- The **Blog API capstone** combines all of the above against **PostgreSQL**, and contains three deliberately-flagged rough edges (missing `db.refresh()`, a stub login with no real credential check, and a `response_model`/return-shape mismatch after adding pagination) worth knowing how to spot and fix.

---

## Cheat Sheet

**Run the app**
```bash
uvicorn main:app --reload
```

**Core imports you'll reach for constantly**
```python
from fastapi import FastAPI, Depends, HTTPException, Header, Request, Query, UploadFile, File, status
from fastapi.responses import JSONResponse
from fastapi.staticfiles import StaticFiles
from fastapi.middleware.cors import CORSMiddleware
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from pydantic import BaseModel
from sqlalchemy import create_engine, Column, Integer, String, Text
from sqlalchemy.orm import sessionmaker, declarative_base, Session
```

**The DB-session dependency pattern (memorize this exact shape)**
```python
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

**The one query idiom that powers almost all CRUD**
```python
db.query(Model).filter(Model.id == id).first()
```

**Pydantic schema pair for any resource**
```python
class XCreate(BaseModel):
    field: type

class XResponse(BaseModel):
    id: int
    field: type
    class Config:
        from_attributes = True
```

**Raising a clean error**
```python
raise HTTPException(status_code=404, detail="Not found")
```

**Dependency injection**
```python
def my_dependency():
    ...

@app.get("/route")
def route(x = Depends(my_dependency)):
    ...
```

**Middleware skeleton**
```python
@app.middleware("http")
async def my_middleware(request: Request, call_next):
    response = await call_next(request)
    return response
```

**JWT create/verify skeleton**
```python
def create_token(data: dict):
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + timedelta(minutes=30)
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

def verify_token(token: str = Depends(oauth2_scheme)):
    try:
        return jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid or expired token")
```

**Pagination math**
```python
start = (page - 1) * limit
end = start + limit
data_slice = full_list[start:end]           # Python list
db_slice = query.offset(start).limit(limit).all()   # SQLAlchemy
```

**CORS skeleton**
```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

**Deployment essentials**
```bash
pip freeze > requirements.txt
# .gitignore must contain: venv/ .env __pycache__/
# Render Start Command: uvicorn main:app --host 0.0.0.0 --port <PORT>
```

**Common pip installs by topic**

| Topic | Install |
|---|---|
| Core | `fastapi uvicorn` |
| ORM (SQLite/Postgres) | `sqlalchemy` (+ `psycopg2-binary` for Postgres) |
| Basic JWT | `python-jose` |
| Production auth | `python-jose "passlib[bcrypt]" python-multipart` |
| Config | `python-dotenv` |
| Testing | `pytest httpx` |
| Third-party APIs | `requests` |
| Scraping | `beautifulsoup4` |
| Rate limiting | `slowapi` |

---

## Interview Questions and Answers

### 🟢 Beginner

> **Q1. What is an API?**
> **Answer:** A bridge between frontend and backend — the frontend sends a request, the backend responds, usually in JSON.
> **Explanation:** Every FastAPI endpoint is ultimately implementing this request/response contract.
> **Why Interviewers Ask This:** It confirms you understand the fundamental mental model before diving into framework specifics.
> **Possible Follow-up:** "What format is the response usually in, and why?"

> **Q2. What is the difference between GET and POST?**
> **Answer:** GET fetches/reads data and never modifies it; POST creates new data on the server.
> **Explanation:** GET requests can be tested directly in a browser URL bar; POST (and PUT/DELETE) require a tool like Swagger UI or Postman.
> **Why Interviewers Ask This:** Confirms basic HTTP-method literacy, foundational to any REST API discussion.
> **Possible Follow-up:** "Where does PUT fit, and how is it different from PATCH?"

> **Q3. What is a path parameter, and how does it differ from a query parameter?**
> **Answer:** A path parameter identifies *which* resource (embedded in the URL path, e.g. `/users/5`); a query parameter supplies optional filters/options after a `?` (e.g. `?limit=10`).
> **Explanation:** Path params are typically required and resource-identifying; query params are typically optional and filter-like.
> **Why Interviewers Ask This:** Tests whether you can design clean, conventional URL schemes.
> **Possible Follow-up:** "How would you make a query parameter optional with a default value?"

> **Q4. What is Pydantic, and why does FastAPI rely on it so heavily?**
> **Answer:** Pydantic is a data validation library; FastAPI uses it to automatically validate request/response data against declared types and to auto-generate interactive documentation.
> **Explanation:** Without Pydantic, you'd be writing manual `if/else` type checks everywhere.
> **Why Interviewers Ask This:** Pydantic is arguably FastAPI's single most distinguishing feature versus Flask/Django.
> **Possible Follow-up:** "What happens if a client sends the wrong data type to a Pydantic-validated field?"

> **Q5. What is CRUD?**
> **Answer:** Create, Read, Update, Delete — the four fundamental operations on any resource.
> **Explanation:** Maps naturally onto POST, GET, PUT, DELETE HTTP methods.
> **Why Interviewers Ask This:** Nearly every backend task is some flavor of CRUD; this question checks baseline vocabulary.
> **Possible Follow-up:** "What's the difference between PUT and PATCH?"

> **Q6. What is `HTTPException` used for?**
> **Answer:** FastAPI's built-in class for raising clean, structured HTTP errors with a status code and detail message.
> **Explanation:** `raise HTTPException(status_code=404, detail="Not found")` is the standard way to signal a client-facing error.
> **Why Interviewers Ask This:** Confirms you know FastAPI's idiomatic error-handling mechanism rather than ad-hoc error dicts.
> **Possible Follow-up:** "How would you avoid repeating this same error-raising logic across many routes?"

> **Q7. What does `uvicorn main:app --reload` do?**
> **Answer:** Runs the FastAPI app named `app` inside `main.py` using the Uvicorn ASGI server, auto-reloading on code changes.
> **Explanation:** Standard local-development run command.
> **Why Interviewers Ask This:** Basic tooling literacy check.
> **Possible Follow-up:** "What would change about this command for a production deployment?"

> **Q8. What is a virtual environment, and why use one?**
> **Answer:** An isolated Python environment per project so dependency versions don't clash across projects.
> **Explanation:** Considered industry standard for nearly all real projects.
> **Why Interviewers Ask This:** Tests basic professional Python hygiene.
> **Possible Follow-up:** "What problem occurs if you skip using one?"

> **Q9. What is Swagger UI, and what is it used for in FastAPI?**
> **Answer:** An auto-generated, interactive API documentation page (at `/docs`) where you can inspect and test every endpoint directly in the browser.
> **Explanation:** Removes the need for Postman during quick manual testing.
> **Why Interviewers Ask This:** A distinguishing FastAPI feature worth knowing how to use fluently.
> **Possible Follow-up:** "Can you test a POST request directly in the browser's URL bar? Why or why not?"

> **Q10. What is a Response Model, and why would you use one?**
> **Answer:** A Pydantic model that defines exactly what shape of data an endpoint sends back — used to hide sensitive fields (like passwords) and validate the output.
> **Explanation:** `response_model=` filters out anything not declared on the response schema, even if the underlying returned object has extra fields.
> **Why Interviewers Ask This:** Tests awareness of basic API security hygiene.
> **Possible Follow-up:** "What's the difference between a Request Model and a Response Model?"

> **Q11. What is JSON, and why does FastAPI default to it?**
> **Answer:** JavaScript Object Notation — a lightweight, structured, language-agnostic data format.
> **Explanation:** JSON is easy for virtually any frontend technology to parse and use, making it the de facto standard for REST APIs.
> **Why Interviewers Ask This:** Baseline data-format literacy.
> **Possible Follow-up:** "What other response formats could an API theoretically return?"

### 🟡 Intermediate

> **Q1. What is Dependency Injection in FastAPI, and how does `Depends` work?**
> **Answer:** A pattern where a function's required logic/dependency is supplied automatically by an external mechanism — `Depends(fn)` tells FastAPI to call `fn` and inject its return value into the route.
> **Explanation:** Promotes reusable, DRY logic (e.g., one `get_current_user` function reused across many routes).
> **Why Interviewers Ask This:** A core, frequently-tested FastAPI architectural concept.
> **Possible Follow-up:** "How would you use dependency injection to manage a database session per request?"

> **Q2. What's the difference between Middleware and a Dependency?**
> **Answer:** Middleware runs globally on *every* request/response; a dependency is opt-in and scoped to the specific routes that declare it.
> **Explanation:** Middleware suits logging, timing, CORS, and global auth; dependencies suit per-route logic like a specific permission check or a DB session.
> **Why Interviewers Ask This:** A classic "do you actually understand the architecture" question.
> **Possible Follow-up:** "Give a real example where you'd pick one over the other."

> **Q3. What is the difference between SQLite and SQLAlchemy, and when would you choose each?**
> **Answer:** SQLite is a lightweight, file-based database engine; SQLAlchemy is an ORM that can sit on top of SQLite (or Postgres, MySQL, etc.) and lets you model/query the database using Python classes instead of raw SQL.
> **Explanation:** SQLite alone suits small apps; SQLAlchemy is preferred for larger/enterprise applications because the resulting code stays clean and maintainable at scale.
> **Why Interviewers Ask This:** Tests understanding of the database-layer trade-offs, not just syntax memorization.
> **Possible Follow-up:** "Walk me through the three core SQLAlchemy setup objects and what each does."

> **Q4. Explain the `get_db()` dependency pattern and why it uses `yield`.**
> **Answer:** It opens a new database session, `yield`s it to the route for use, then closes it in a `finally` block — guaranteeing the session is cleaned up even if the route raises an exception.
> **Explanation:** This is a generator-based dependency, a FastAPI idiom specifically designed for "setup → use → teardown" resources like DB sessions.
> **Why Interviewers Ask This:** A very commonly asked FastAPI+SQLAlchemy integration question.
> **Possible Follow-up:** "What would go wrong if you didn't close the session?"

> **Q5. What's the difference between `db.commit()` and `db.refresh()`?**
> **Answer:** `commit()` persists the current transaction to the database; `refresh()` reloads the in-memory Python object with the latest data from the database (e.g., to pick up an auto-generated ID).
> **Explanation:** Skipping `refresh()` after a commit can mean you return stale data to the client even though the database itself is correct.
> **Why Interviewers Ask This:** A precise, detail-oriented question that separates surface familiarity from real hands-on SQLAlchemy experience.
> **Possible Follow-up:** "Where in a typical Create or Update endpoint would you place `db.refresh()`?"

> **Q6. What are the three parts of a JWT, and what does each contain?**
> **Answer:** Header (signing algorithm info), Payload (claims — user identity, expiry, etc.), Signature (verifies the token hasn't been tampered with, using the secret key + algorithm).
> **Explanation:** All three parts together make the token both informative and tamper-evident.
> **Why Interviewers Ask This:** Standard auth-fundamentals question for any backend role.
> **Possible Follow-up:** "Why should a JWT always have an expiry claim?"

> **Q7. Why is password hashing necessary, and what's `bcrypt`'s role?**
> **Answer:** Storing plaintext passwords is a severe security risk if the database is ever compromised; `bcrypt` is a hashing algorithm (used via `passlib`) that stores a one-way, salted hash instead of the raw password.
> **Explanation:** Login then compares a hash of the submitted password against the stored hash — never the raw password itself.
> **Why Interviewers Ask This:** A core, non-negotiable security practice every backend engineer should know cold.
> **Possible Follow-up:** "What's the difference between hashing and encryption?"

> **Q8. What is CORS, and how do you configure it correctly in FastAPI?**
> **Answer:** A browser-enforced security policy blocking cross-origin requests unless the server explicitly allows them; configured via `CORSMiddleware` with `allow_origins`, `allow_credentials`, `allow_methods`, and `allow_headers`.
> **Explanation:** A very common real-world "my frontend can't talk to my backend" debugging scenario.
> **Why Interviewers Ask This:** Tests practical full-stack integration experience, not just backend theory.
> **Possible Follow-up:** "What would you do differently for `allow_origins` in production versus local development?"

> **Q9. Why should secrets live in environment variables instead of source code?**
> **Answer:** Hardcoded secrets get pushed to version control (risking leaks), can't easily differ per environment, and are harder to rotate; `.env` + `os.getenv()` (or a config/Settings class) keeps them out of the codebase entirely.
> **Explanation:** A `.gitignore` entry for `.env` is the other half of this practice — without it, the "secret" file itself gets committed anyway.
> **Why Interviewers Ask This:** A universal backend hygiene question, framework-agnostic.
> **Possible Follow-up:** "How would you structure config for a project with many environment-specific values?"

> **Q10. How would you implement pagination, and what parameters should the response include?**
> **Answer:** Compute `start = (page - 1) * limit` and slice/offset the dataset accordingly; the response should include `page`, `limit`, `total`, and the sliced `data`.
> **Explanation:** At the SQL level this becomes `query.offset(start).limit(limit).all()` instead of a Python list slice.
> **Why Interviewers Ask This:** A frequently-asked "implement this live" style question.
> **Possible Follow-up:** "How would you add search/filtering on top of this pagination?"

> **Q11. What's the difference between `async def` and a plain `def` route in FastAPI?**
> **Answer:** `async def` lets a route use `await` for non-blocking I/O, allowing FastAPI to handle other requests concurrently while waiting; a plain `def` route runs synchronously (FastAPI still runs it efficiently via a thread pool, but it can't `await`).
> **Explanation:** Use `async def` specifically when the route performs I/O-bound work (API calls, DB calls); plain CPU-bound logic doesn't benefit from it.
> **Why Interviewers Ask This:** Confirms nuanced understanding, not just "always use async."
> **Possible Follow-up:** "What happens if you call a blocking operation inside an `async def` route without `await`?"

### 🔴 Advanced

> **Q1. Walk through the full request lifecycle of a protected, paginated SQLAlchemy endpoint — e.g., `GET /blogs?page=2&limit=5&search=Ram` with JWT auth.**
> **Answer:** Request hits the route → any middleware runs first (e.g., CORS, logging) → `Depends(get_db)` opens a fresh session → (if protected) `Depends(verify_token)` decodes and validates the JWT, raising 401 on failure → the route builds a base query, conditionally applies an `.ilike()` search filter, computes `offset`/`limit` from `page`/`limit`, executes the query → the DB session is closed in `get_db`'s `finally` block → the response (often validated against a `response_model`) is returned, passing back out through any middleware.
> **Explanation:** This stitches together nearly every architectural piece from the course into one trace.
> **Why Interviewers Ask This:** Tests whether you can reason about the full system, not just isolated snippets.
> **Possible Follow-up:** "Where exactly would a bug in the search filter's wildcard logic manifest, and how would you debug it?"

> **Q2. The capstone's `GET /blogs` endpoint declares `response_model=list[schemas.BlogResponse]` but returns a dict shaped like `{page, limit, total, data}`. What's wrong, and how would you fix it?**
> **Answer:** The declared response model doesn't match the actual return shape — FastAPI's response validation expects a bare list of `BlogResponse` objects, not a wrapping dict. Fix: either remove `response_model` here, or define a new `PaginatedBlogResponse` schema (e.g., with `page: int`, `limit: int`, `total: int`, `data: list[BlogResponse]`) and use that instead.
> **Explanation:** This is an example of a very realistic "find the bug" code-review scenario, taken directly from a flagged inconsistency in the course's own capstone code.
> **Why Interviewers Ask This:** Interviewers love planting a subtle, realistic mismatch like this to test attentiveness.
> **Possible Follow-up:** "How would FastAPI behave at runtime if this mismatch isn't fixed — would it error, or silently misbehave?"

> **Q3. The capstone's Create-Blog endpoint skips `db.refresh()` after `db.commit()`. Under what circumstances could this actually cause a bug, and why might it "work" in the demo anyway?**
> **Answer:** Without `refresh()`, the returned ORM object might not reflect DB-generated defaults (like an auto-incremented `id`) reliably across all SQLAlchemy/session configurations. It may "work" in the demo because some session configurations (e.g., certain `expire_on_commit` settings) automatically refresh attributes on access after commit — but this isn't something to rely on without being explicit.
> **Explanation:** A good answer shows awareness that SQLAlchemy's session behavior is configuration-dependent, not something to assume blindly.
> **Why Interviewers Ask This:** Separates "copied the code" from "understands why the code works."
> **Possible Follow-up:** "What session setting controls this behavior, and what would you set it to for predictable results?"

> **Q4. Design a global exception-handling strategy for an API that has dozens of routes raising similar but slightly different domain errors (e.g., `UserNotFoundException`, `BlogNotFoundException`, `OrderNotFoundException`).**
> **Answer:** Define a shared base exception (e.g., `NotFoundException(Exception)` with a `resource_name` attribute), have each specific exception subclass it, and register **one** `@app.exception_handler(NotFoundException)` that formats a consistent `404` response using `exception.resource_name`. This avoids one handler per exception type while still allowing per-domain exceptions to carry specific context.
> **Explanation:** Demonstrates scaling the course's "custom exception + global handler" pattern into a real, larger codebase.
> **Why Interviewers Ask This:** Tests architectural judgment beyond the toy example taught in the course.
> **Possible Follow-up:** "How would you still distinguish `UserNotFoundException` from `BlogNotFoundException` in logs or monitoring if they share one handler?"

> **Q5. Why must `jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])` take a *list* for `algorithms`, even when you only support one algorithm — and what's the security risk if this is implemented incorrectly?**
> **Answer:** `python-jose` (and most JWT libraries) require an explicit allow-list of acceptable algorithms to defend against **algorithm-confusion attacks**, where an attacker crafts a token using a different (often weaker, or `none`) algorithm than the server expects, attempting to bypass signature verification. Requiring an explicit list (even with one entry) forces the server to reject any token signed with an unexpected algorithm.
> **Explanation:** This directly explains why the course's own code (which sometimes used `algorithm=` singular) should really use the list form consistently.
> **Why Interviewers Ask This:** A genuinely important JWT security nuance that distinguishes a security-aware candidate.
> **Possible Follow-up:** "What is the 'alg: none' attack, specifically?"

> **Q6. How would you add rate limiting that applies per-authenticated-user instead of per-IP address?**
> **Answer:** Replace `slowapi`'s default `key_func=get_remote_address` with a custom key function that extracts the user's identity from the verified JWT (e.g., the `sub` claim) instead of the request's IP — since multiple users behind the same NAT/IP would otherwise share one rate-limit bucket, and a single user switching IPs would otherwise dodge limits meant for them.
> **Explanation:** Shows you understand `slowapi`'s extensibility, not just its default configuration.
> **Why Interviewers Ask This:** Tests whether you can adapt a taught pattern to a materially different real-world requirement.
> **Possible Follow-up:** "What happens to unauthenticated requests under this scheme — how would you rate-limit those separately?"

> **Q7. The capstone's login endpoint issues a valid JWT without checking any real credentials. If this shipped to production, what's the actual security impact, and how would you fix it properly?**
> **Answer:** Any client could call `/login` with *any* (or no) credentials and receive a fully valid, signed token granting access to every protected route — a complete authentication bypass. The fix: look up the submitted username in a real user table, use `verify_password(form_data.password, user.hashed_password)` (bcrypt comparison) before calling `create_token`, and return `401` on any mismatch — exactly the pattern already shown in Section 20's `fake_user_db` example, just wired to a real database instead of a hardcoded dict.
> **Explanation:** A direct, high-stakes extension of a flaw explicitly called out in the extraction notes — a great "spot the vulnerability" interview scenario.
> **Why Interviewers Ask This:** Security-mindedness under a tight, practical scenario is one of the most valuable things to screen for in a backend interview.
> **Possible Follow-up:** "How would you also prevent timing attacks during credential comparison?"

> **Q8. Compare the trade-offs between `Depends`-based per-route database sessions (as taught) versus a single shared session for the whole app's lifetime.**
> **Answer:** Per-request sessions (via `get_db()`+`yield`) isolate each request's transaction, avoid stale cached state leaking between unrelated requests, and are safe under concurrent requests. A single shared/global session risks stale data, thread-safety issues, and one request's uncommitted/failed transaction silently affecting another's.
> **Explanation:** Explains *why* the course's one specific pattern (session per request) is the right default, not an arbitrary convention.
> **Why Interviewers Ask This:** Tests understanding of *why*, not just *how*.
> **Possible Follow-up:** "How does this change in an async SQLAlchemy setup using `AsyncSession`?"

> **Q9. How would you redesign the caching example from Section 28 to work correctly across multiple server processes/workers (e.g., behind Gunicorn with 4 Uvicorn workers)?**
> **Answer:** The taught `global` in-memory cache only works within a single process — each worker would have its own separate `cache_data`/`last_fetch`, defeating the point. Fix: move the cache to a shared external store (e.g., Redis) with its own TTL support (`SETEX`), so all worker processes share one consistent cache state.
> **Explanation:** A classic "this demo doesn't scale as-is" follow-up that tests production-readiness thinking.
> **Why Interviewers Ask This:** Separates "learned the demo" from "understands deployment realities."
> **Possible Follow-up:** "What cache invalidation strategy would you use if the underlying data could change before the TTL expires?"

> **Q10. The course teaches CORS with a hardcoded `allow_origins` list. How would you manage this safely across local, staging, and production environments?**
> **Answer:** Move `allow_origins` into environment-specific configuration (via `.env`/`Settings`, Section 23), so each environment loads its own correct origin list at startup rather than a single hardcoded value baked into source. Avoid `allow_origins=["*"]` in production when `allow_credentials=True` is set, since browsers disallow wildcard origins combined with credentials, and it's also a security risk even where technically permitted.
> **Explanation:** Connects two course topics (CORS + config management) into one coherent production practice.
> **Why Interviewers Ask This:** Tests integration of multiple taught concepts into one realistic deployment concern.
> **Possible Follow-up:** "What would you do if you needed to support an unknown/dynamic set of subdomains as valid origins?"

---

## Scenario-Based Questions

> **Scenario 1: Your FastAPI backend works perfectly when tested via Swagger, but the React frontend's network requests fail with a "blocked by CORS policy" console error.**
> 1. **Initial investigation:** Confirm the frontend and backend are genuinely on different origins (different host/port).
> 2. **Metrics/logs to check:** Browser console error message (it names the blocked origin); backend startup logs to confirm `CORSMiddleware` is actually registered.
> 3. **Possible causes:** Missing/incorrect `CORSMiddleware` setup; frontend origin not included in `allow_origins`; using `allow_origins=["*"]` together with `allow_credentials=True` (browsers reject this combination).
> 4. **Debugging approach:** Check the exact origin string in the browser error against `allow_origins`; temporarily widen origins locally to confirm CORS is indeed the blocker, then narrow back down.
> 5. **Resolution:** Add the exact frontend origin (scheme + host + port) to `allow_origins`; ensure `allow_methods`/`allow_headers` cover what the frontend actually sends.
> 6. **Prevention:** Keep `allow_origins` in environment-specific config (not hardcoded), and add an integration test that hits the API cross-origin from a test client mimicking the frontend's origin.

> **Scenario 2: After deploying to Render, your app builds successfully but the live URL returns a server error immediately and the service won't stay "Live."**
> 1. **Initial investigation:** Check Render's deploy logs for the exact startup error (most likely a missing Start Command, or a missing environment variable the app expects at import time).
> 2. **Metrics/logs to check:** Render's build log vs. runtime log — differentiate "build succeeded" from "app crashed on start."
> 3. **Possible causes:** Missing/incorrect `uvicorn main:app --host 0.0.0.0 --port <PORT>` start command; an environment variable (e.g., `SECRET_KEY`, `DB_URL`) not set in Render's dashboard, causing `os.getenv()` to return `None` and crash downstream code.
> 4. **Debugging approach:** Reproduce locally with the exact same environment variables (or lack thereof) to see if the same crash occurs; add defensive `if not value: raise RuntimeError(...)` checks around critical env vars for clearer errors.
> 5. **Resolution:** Set the correct Start Command and add all required environment variables in Render's dashboard before redeploying.
> 6. **Prevention:** Maintain a documented list of required environment variables per project (e.g., in a `.env.example` file) so nothing is missed during deployment.

> **Scenario 3: Your `/blogs` search endpoint returns zero results for a search term you know exists in the data.**
> 1. **Initial investigation:** Confirm the exact SQL being generated (log the query or inspect it) and compare against what you expect.
> 2. **Metrics/logs to check:** The actual WHERE clause / filter condition applied; confirm the search string being passed in matches expectations (case, whitespace).
> 3. **Possible causes:** Forgotten `%` wildcards around the search term in an `ilike()`/`like()` filter (causing an exact-match-only comparison instead of "contains") — the exact bug explicitly demonstrated live in the capstone.
> 4. **Debugging approach:** Temporarily print/log the fully-formed filter string; test the same query directly against the database (e.g., via pgAdmin's query tool) to isolate whether it's an API bug or a data issue.
> 5. **Resolution:** Wrap the search term in `%` wildcards: `.filter(Model.field.ilike(f"%{search}%"))`.
> 6. **Prevention:** Add an automated test asserting that a partial, substring search term returns the expected record — this specific bug would have been caught instantly by such a test.

> **Scenario 4: A JWT-protected route starts rejecting all tokens with "Invalid or expired token" shortly after a deployment, even for freshly-issued tokens.**
> 1. **Initial investigation:** Check whether `SECRET_KEY` differs between the process that issued the token and the process verifying it (e.g., different `.env` values loaded in different environments, or a secret regenerated on each restart).
> 2. **Metrics/logs to check:** Confirm the exact `SECRET_KEY`/`ALGORITHM` values loaded at runtime in both the issuing and verifying code paths (without logging the actual secret value itself, for security).
> 3. **Possible causes:** `SECRET_KEY` not set via environment variable consistently across processes/workers; clock drift between servers affecting `exp` validation; algorithm mismatch between `jwt.encode` and `jwt.decode`.
> 4. **Debugging approach:** Decode a freshly-issued token manually (e.g., via a JWT debugger) to inspect its header/payload/expiry and compare against what the verifying code expects.
> 5. **Resolution:** Ensure `SECRET_KEY` and `ALGORITHM` are loaded identically everywhere (ideally from one shared `.env`/config source), and verify server clocks are in sync.
> 6. **Prevention:** Never hardcode or regenerate secrets per-process; store `SECRET_KEY` once in environment configuration shared across all app instances.

---

## Hands-on Exercises

### Easy
1. Build a `/greet/{name}` GET route that returns `{"message": f"Hello, {name}!"}`, with `name` validated as a string path parameter.
2. Add an optional `shout: bool = False` query parameter to the above route; if `True`, return the message in uppercase.
3. Create a Pydantic `Product` model (`name: str`, `price: float`, `in_stock: bool`) and a POST route that accepts it and echoes it back.

### Medium
4. Build a full in-memory CRUD API for a `Note` resource (`id`, `title`, `body`), following the exact pattern from Section 8 — including proper unique-ID handling and `HTTPException(404, ...)` on missing IDs.
5. Rebuild the same `Note` API on top of SQLite + SQLAlchemy, using the `get_db()` dependency pattern and the `filter(...).first()` query idiom.
6. Add a `response_model` to your `Note` API's GET routes that hides an internal `created_by_internal_id` field while still showing `id`, `title`, and `body`.
7. Add basic JWT authentication to your `Note` API: a `/login` endpoint with hardcoded credentials, and protect the Create/Update/Delete routes with `Depends(verify_token)`.
8. Add pagination (`page`, `limit`) and a case-insensitive `search` query parameter to your `Note` API's "get all" endpoint, using the `(page-1)*limit` offset formula.

### Advanced
9. Upgrade your `Note` API's authentication to production-style OAuth2 + JWT + bcrypt password hashing, backed by a real (even if small) `users` table instead of a hardcoded dict — fixing the exact "stub login" gap flagged in the capstone.
10. Add a custom exception (`NoteNotFoundException`) and a global exception handler for it, replacing every inline `HTTPException(404, "Note not found")` call with the custom exception.
11. Add CORS support so a minimal React app (or even a plain `fetch()` call from a different port) can call your `Note` API successfully, and verify it in the browser's Network tab.
12. Write `pytest` tests (using `TestClient`) covering: successful create, create validation failure, read-by-id success, read-by-id not-found, and the full auth-protected delete flow (both authorized and unauthorized cases).
13. Deploy your finished `Note` API to Render.com, including a correct `requirements.txt`, `.gitignore`, and Start Command, and verify the live `/docs` page works end-to-end.

---

## Practice Assignment

### Objective
Build a **"Bookshelf API"** — a smaller, self-contained sibling to the course's Blog API capstone — to consolidate persistence, auth, pagination/search, and testing in one project.

### Requirements
- A `Book` resource with fields: `id`, `title`, `author`, `genre`, `published_year`, `is_available` (bool).
- Full CRUD, backed by SQLite or PostgreSQL via SQLAlchemy (your choice), using the `get_db()` dependency pattern.
- Pydantic `BookCreate` / `BookResponse` schemas, with `BookResponse` using `from_attributes = True`.
- JWT-protected Create/Update/Delete routes (OAuth2 + JWT + bcrypt-hashed passwords against a real, even if tiny, user table — not a stub login).
- A `GET /books` endpoint supporting `page`, `limit`, and a case-insensitive `search` (by `title` or `author`) — with correctly-wildcarded `ilike()` filtering.
- At least 5 automated `pytest` tests covering the happy path and at least two realistic failure cases (not-found, unauthorized).

### Architecture
```text
bookshelf_api/
├── main.py           # app init, table creation, route wiring
├── database.py       # engine, SessionLocal, Base
├── models.py         # Book, User ORM models
├── schemas.py        # BookCreate, BookResponse, (User schemas if needed)
├── auth.py           # create_token, verify_token, password hash/verify helpers
├── config.py         # Settings class reading from .env
├── test_main.py       # pytest test suite
├── requirements.txt
└── .gitignore
```

### Expected Functionality
- `POST /login` issues a real JWT only after verifying a hashed password against a stored user.
- `POST /books`, `PUT /books/{id}`, `DELETE /books/{id}` all require a valid bearer token.
- `GET /books` and `GET /books/{id}` remain public.
- `GET /books?page=2&limit=5&search=tolkien` returns the correct paginated, filtered slice with `page`, `limit`, `total`, `data`.

### Suggested Implementation Order
1. Database + models + basic CRUD (no auth yet) — verify in Swagger + a DB viewer.
2. Add schemas and response models.
3. Add real (non-stub) auth; protect the mutating routes.
4. Add pagination + search to the list endpoint.
5. Write tests last, covering everything built so far.
6. Add `.env`/config, `.gitignore`, `requirements.txt`; deploy to Render.

### Expected Output
A running, deployed API whose `/docs` page lets you log in, create/update/delete books while authorized, and browse/search books while logged out — exactly mirroring the Blog API capstone's behavior, but for books, and with the stub-login and `db.refresh()` gaps fixed from the start.

### Challenges
- Correctly wiring `from_attributes = True` so `BookResponse` can be built directly from a SQLAlchemy `Book` object.
- Getting the `ilike()` wildcard search right on the first try (recall the capstone's live bug!).
- Keeping the paginated response's `response_model` consistent with its actual dict-shaped return value (recall the capstone's flagged inconsistency!).

### Bonus Improvements
- Add rate limiting (`slowapi`) to the public `GET /books` endpoint to prevent scraping abuse.
- Add a simple in-memory or Redis-backed cache for the most-requested book list queries.
- Add a file-upload endpoint for book cover images, served back via `StaticFiles`.

---

## Additional Resources

- **Official FastAPI documentation** — https://fastapi.tiangolo.com/ (the canonical reference for every feature in this guide).
- **Pydantic documentation** — https://docs.pydantic.dev/ (especially the migration notes on `from_attributes` vs. the older `orm_mode`).
- **SQLAlchemy ORM documentation** — https://docs.sqlalchemy.org/en/latest/orm/ (sessions, query API, relationship patterns).
- **python-jose on PyPI** — for JWT encode/decode details and supported algorithms.
- **passlib documentation** — for `CryptContext` and bcrypt configuration options.
- **Starlette CORS middleware docs** — FastAPI's `CORSMiddleware` is re-exported from Starlette; its docs cover edge cases (e.g., wildcard origins + credentials).
- **slowapi on PyPI** — for rate-limiting configuration beyond the basics shown here.
- **BeautifulSoup documentation** — https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- **Render.com documentation** — for deployment specifics beyond this course's walkthrough (custom domains, paid tiers, background workers).
- **pytest documentation** — https://docs.pytest.org/ — for fixtures, parametrization, and more advanced testing patterns beyond simple `assert` statements.

---

## Final Revision Sheet

**Core concepts to recall instantly:**
- API = frontend ↔ backend bridge via JSON. Route = URL path → function.
- Path param = *which* resource. Query param = *filter/option*. Body = *actual data*.
- Pydantic `BaseModel` = schema + automatic validation + auto-docs.
- CRUD = Create/Read/Update/Delete ↔ POST/GET/PUT/DELETE.

**Important definitions:**
- JWT = Header + Payload + Signature.
- ORM = database access via Python objects, not raw SQL.
- TTL = how long cached data stays valid before refetching.
- CORS = browser-enforced cross-origin security policy.

**Important commands/code to remember cold:**
```python
uvicorn main:app --reload

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

db.query(Model).filter(Model.id == id).first()

raise HTTPException(status_code=404, detail="Not found")

jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)
jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])

start = (page - 1) * limit
```

**Architecture/process to recall:**
- DB session lifecycle: `get_db()` → `yield` → route uses session → `finally: db.close()`.
- Auth lifecycle: login → `create_token` → client stores token → `Depends(verify_token)` gates protected routes → `401` on failure.
- Deployment lifecycle: `pip freeze > requirements.txt` → `.gitignore` → GitHub push → connect to Render → set Start Command + env vars → deploy.

**Best practices:**
- Always use a virtual environment for real projects.
- Always `db.refresh(obj)` after `db.commit()` if you're about to return that object.
- Always hash passwords (bcrypt) — never store or compare plaintext.
- Always wildcard (`%...%`) a `LIKE`/`ILIKE` search filter unless you specifically want an exact match.
- Always keep secrets in `.env` (gitignored), never hardcoded.
- Always set an expiry (`exp`) claim on every JWT you issue.

**Common mistakes to avoid:**
- Duplicate/non-unique IDs in any CRUD resource.
- Forgetting `db.refresh()` and returning stale data.
- Forgetting `%` wildcards in a search filter (silently returns zero results).
- A `response_model` that no longer matches the endpoint's actual return shape after a refactor (e.g., adding pagination).
- A login endpoint that issues tokens without actually checking credentials.
- Using `jwt.decode(..., algorithm=...)` (singular) instead of `algorithms=[...]` (list).

**Interview points to keep sharp:**
- Middleware (global) vs. Dependency (per-route) — know this cold.
- PUT (full replace) vs. PATCH (partial update) — this course only built PUT, but the distinction is a classic question.
- Why async helps specifically with I/O-bound work, not CPU-bound work.
- The security impact of a stub/unvalidated login endpoint — be ready to explain this exactly as a vulnerability, not just "a simplification."
- Algorithm-confusion attacks and why `jwt.decode` should take an explicit algorithms allow-list.

**Things to remember about this specific course's capstone (great "describe a project" interview material):**
- The Blog API: PostgreSQL + SQLAlchemy + Pydantic schemas + JWT-protected mutating routes + public read routes + pagination/search — five parts, one Git branch, pushed to GitHub at the end.
- Three deliberately-flagged rough edges you should be able to name and fix: missing `db.refresh()`, the stub login, and the pagination `response_model` mismatch.
</content>