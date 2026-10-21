# FastAPI Learning Path: Day 1

Welcome to Day 1 of learning **FastAPI**! This guide is designed to be simple enough for beginners while going deep enough so you truly understand *how* and *why* things work under the hood.

---

## 1. FastAPI Basics

### What is FastAPI?
FastAPI is a modern, high-performance web framework for building APIs with Python 3.8+ based on standard Python type hints. 

### Why is it so popular?
1. **Fast Performance:** It is on par with NodeJS and Go, thanks to **Starlette** (which handles the web/HTTP parts) and **Pydantic** (which handles the data validation parts).
2. **Automatic Documentation:** When you write code in FastAPI, it automatically generates interactive API documentation (Swagger UI and ReDoc) without any extra configuration.
3. **Type Hints & Validation:** It uses standard Python type hints (`str`, `int`, `bool`) to validate data automatically. If a user sends the wrong data type, FastAPI catches it and sends back a clear error message.

### Installation & Setup
To run FastAPI, you need the framework itself and an **ASGI server** (like Uvicorn) to serve your application.

```bash
pip install "fastapi[standard]"
```
*(Note: Installing `fastapi[standard]` automatically includes `uvicorn` and other helpful tools).*

### Your First App (`main.py`)
Create a file named `main.py` and add the following code:

```python
from fastapi import FastAPI

# 1. Create an instance of the FastAPI class
app = FastAPI()

# 2. Define a route (path operation)
@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

### Running the Server
Open your terminal in the same directory as `main.py` and run:

```bash
fastapi dev main.py
# Or using uvicorn directly:
# uvicorn main:app --reload
```
* `main`: The file `main.py` (the Python module).
* `app`: The object created inside of `main.py` with the line `app = FastAPI()`.
* `--reload` / `dev`: Makes the server restart automatically every time you save changes to your code.

### The Magic of `/docs`
Once your server is running, open your browser and visit:
* **App URL:** `http://127.0.0.1:8000` (You will see `{"message": "Hello, World!"}`)
* **Interactive Docs (Swagger UI):** `http://127.0.0.1:8000/docs` — Here you can test your API endpoints directly in the browser!

---

## 2. Routes (Path Operations)

In FastAPI, a "Route" is often called a **Path Operation**. Let's break down what that means.

```python
@app.get("/users")
def get_users():
    return ["Alice", "Bob"]
```

### Anatomy of a Route
1. **Path (Endpoint/Route):** The part of the URL starting from the first `/`. In `https://example.com/items/foo`, the path is `/items/foo`.
2. **Operation (HTTP Method):** Refers to the HTTP method used to communicate with the server. The most common are:
   * `GET`: To read/fetch data.
   * `POST`: To create new data.
   * `PUT`: To update existing data.
   * `DELETE`: To delete data.
3. **Path Operation Decorator (`@app.get(...)`):** The `@` symbol in Python is a decorator. It tells FastAPI that the function right below it is responsible for handling requests that go to that specific **path** using that specific **operation**.
4. **Path Operation Function:** The Python function below the decorator (`def get_users():`). FastAPI calls this function whenever a request matches the path and method, and converts whatever dictionary, list, or model you return into **JSON**.

---

## 3. GET Requests

**GET** requests are used exclusively to **retrieve data** from the server. They should never change the state of the server (e.g., don't use a GET request to delete a user).

When fetching data, you often need to pass extra information in the URL. FastAPI gives you two ways to do this in a GET request: **Path Parameters** and **Query Parameters**.

### A. Path Parameters
Path parameters are variable parts of the URL path itself. They are typically used to identify a **specific resource** (like a specific user ID or product ID).

```python
from fastapi import FastAPI

app = FastAPI()

# {item_id} is a path parameter
@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id, "Double ID": item_id * 2}
```

#### Deep Dive: Why `item_id: int` matters
Notice the type hint `: int`. This does two huge things for you:
1. **Data Conversion:** In a URL like `http://127.0.0.1:8000/items/5`, the `5` is technically a text string (`"5"`). FastAPI sees `: int` and automatically converts `"5"` into the Python integer `5`.
2. **Data Validation:** If someone visits `http://127.0.0.1:8000/items/apple`, FastAPI intercepts the request before your function even runs and returns a `422 Unprocessable Entity` JSON error telling the user that `"apple"` is not a valid integer.

> **⚠️ Route Order Matters!**
> FastAPI evaluates routes from top to bottom. If you have `/users/me` and `/users/{user_id}`, you **must** put `/users/me` first. Otherwise, `/users/{user_id}` will catch the request and think `"me"` is the `user_id`!

---

### B. Query Parameters
When you declare function parameters that are **not** part of the `{path}`, FastAPI automatically interprets them as **query parameters**.

Query parameters are the key-value pairs that go after the `?` in a URL, separated by `&`. They are commonly used for **filtering, searching, or pagination**.

Example URL: `http://127.0.0.1:8000/books?genre=scifi&limit=10`

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/books")
def get_books(genre: str, limit: int = 10, published: bool | None = None):
    return {
        "genre": genre,
        "limit": limit,
        "published": published
    }
```

#### Deep Dive: Required vs. Optional Query Parameters
Look closely at the three parameters in `def get_books(...)`:
1. **`genre: str` (Required):** Because it has no default value (`= ...`), the user *must* include `?genre=something` in the URL, or FastAPI will return an error.
2. **`limit: int = 10` (Optional with Default):** Because we gave it a default value of `10`, if the user visits `/books?genre=scifi`, `limit` will automatically be `10`.
3. **`published: bool | None = None` (Completely Optional):** Using Python 3.10+ union syntax (`bool | None = None`), this parameter can be a boolean (`True`/`False`) or `None` if omitted. (Note: In URLs, `?published=true`, `?published=1`, and `?published=yes` are all smartly converted to Python's `True`!).

---

## 4. POST Requests

While `GET` is for reading data, **POST** is used to **send data to the server to create a new resource** (like registering a new user, publishing a blog post, or submitting an order).

### Why not just use GET with Query Parameters to create data?
1. **Security & Privacy:** URLs are logged in browser history and server logs. You should *never* put sensitive data (like passwords) in a URL query string.
2. **Size Limits:** URLs have a character limit (usually around 2000 characters). You can't send a long blog post or complex nested data via a URL.
3. **HTTP Standards:** GET requests are meant to be *idempotent* (running them 10 times shouldn't change the server). POST requests are designed to change server state by adding data inside the hidden **Request Body**.

*Note: You cannot easily test a `POST` request just by typing a URL into your browser's address bar (browsers always make `GET` requests when you hit Enter). You must use FastAPI's `/docs` page, Postman, `curl`, or frontend JavaScript (`fetch`) to test POST routes.*

---

## 5. Request Body (with Pydantic)

A **Request Body** is the data Payload sent by the client (e.g., a web browser or mobile app) to your API. Usually, this data is formatted as **JSON**.

To tell FastAPI what the incoming JSON should look like, we use **Pydantic**.

### What is Pydantic?
Pydantic is a Python library for data validation and settings management using Python type annotations. In FastAPI, you define the "shape" (schema) of your expected JSON data by creating a class that inherits from Pydantic's `BaseModel`.

### Step-by-Step Implementation

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# 1. Define the Schema (Data Model)
class Product(BaseModel):
    name: str
    price: float
    in_stock: bool = True                 # Optional, defaults to True
    description: str | None = None        # Optional, defaults to None

# 2. Use the Model in a POST route
@app.post("/products")
def create_product(product: Product):
    # Inside the function, `product` is a Python object!
    taxed_price = product.price * 1.15
    
    return {
        "message": "Product created successfully!",
        "product_name": product.name,
        "final_price": round(taxed_price, 2),
        "raw_data": product
    }
```

### What happens under the hood when a request arrives?
Suppose a user sends the following JSON in the body of a `POST` request to `/products`:
```json
{
    "name": "Mechanical Keyboard",
    "price": 99.99
}
```
Because you typed `product: Product` in your function, FastAPI automatically:
1. **Reads the body** of the request as JSON.
2. **Converts the corresponding types** (if needed).
3. **Validates the data:**
   * Did they include `name` and `price`? Yes.
   * What about `in_stock` and `description`? They were omitted, so Pydantic fills them in with their defaults (`True` and `None`).
   * If `price` was `"expensive"` instead of a number, FastAPI immediately stops and returns a detailed error showing *exactly* which field failed and why.
4. **Gives you a Python Object:** It hands you the validated data in the variable `product`. You get full editor autocomplete (e.g., typing `product.` in VS Code will pop up `name`, `price`, `in_stock`, and `description`).

---

### Combining Path, Query, and Request Body!
One of FastAPI's smartest features is that you can combine all three concepts in a single function, and FastAPI knows exactly where to grab each piece of data from:

```python
@app.post("/stores/{store_id}/products")
def add_store_product(
    store_id: int,           # 1. Matched in the URL Path -> Path Parameter
    product: Product,        # 2. Inherits from BaseModel -> Request Body (JSON)
    notify_admin: bool = False # 3. Singular type not in path -> Query Parameter
):
    return {
        "store_id": store_id,
        "notify_admin": notify_admin,
        "product": product
    }
```
**How FastAPI tells them apart:**
* If the parameter is also declared in the **path** (`{store_id}`), it will be used as a **path parameter**.
* If the parameter is of a **singular type** (like `int`, `float`, `str`, `bool`) not in the path, it will be interpreted as a **query parameter**.
* If the parameter is declared to be of the type of a **Pydantic model** (`Product`), it will be interpreted as a **request body**.

---

## 6. Putting It All Together (Day 1 Mini-Project)

Here is a complete, runnable `main.py` file that combines everything from Day 1 into a simple in-memory "Todo List" API. Copy this code, run it, and play with it in `http://127.0.0.1:8000/docs`!

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI(title="Day 1 Todo API")

# Fake in-memory database
todos_db = [
    {"id": 1, "title": "Learn FastAPI Basics", "completed": True},
    {"id": 2, "title": "Master GET and POST", "completed": False},
]

# Pydantic Model for creating a new Todo (Request Body)
class TodoCreate(BaseModel):
    title: str
    completed: bool = False


# 1. Basic Root Route
@app.get("/")
def home():
    return {"status": "API is running! Visit /docs to test."}


# 2. GET with Query Parameter (Filter by completed status)
@app.get("/todos")
def get_all_todos(completed: bool | None = None):
    if completed is None:
        return todos_db
    # Filter the list if the query parameter was provided
    filtered_todos = [todo for todo in todos_db if todo["completed"] == completed]
    return filtered_todos


# 3. GET with Path Parameter (Fetch a single Todo by ID)
@app.get("/todos/{todo_id}")
def get_single_todo(todo_id: int):
    for todo in todos_db:
        if todo["id"] == todo_id:
            return todo
    return {"error": "Todo not found"}


# 4. POST with Request Body (Create a new Todo)
@app.post("/todos")
def create_todo(new_todo: TodoCreate):
    new_id = len(todos_db) + 1
    
    # Convert Pydantic object into a standard Python dictionary
    todo_dict = new_todo.model_dump() 
    todo_dict["id"] = new_id
    
    todos_db.append(todo_dict)
    return {"message": "Todo added!", "todo": todo_dict}
```

### Quick Day 1 Cheat Sheet

| Concept | When to use it | Example in FastAPI |
| :--- | :--- | :--- |
| **`@app.get()`** | Fetching/reading data from the server. | `@app.get("/users")` |
| **`@app.post()`** | Sending data to create a new record. | `@app.post("/users")` |
| **Path Parameter** | Identifying a specific resource in the URL. | `@app.get("/users/{id}")` |
| **Query Parameter** | Filtering, sorting, or optional URL settings. | `def get_users(limit: int = 10):` |
| **Request Body** | Sending structured JSON payloads (POST/PUT). | `def create_user(user: UserSchema):` |