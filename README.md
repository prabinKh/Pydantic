# Pydantic



Pydantic is a high-performance data validation and parsing library that leverages Python type hints to ensure your data is correct at runtime. Written in Rust for speed, it converts raw input (JSON, form data, environment variables, etc.) into strongly-typed Python objects.

## Table of Contents
- [Overview](#overview)
- [What is Pydantic?](#what-is-pydantic)
- 
- [Benefits of Pydantic](#benefits-of-pydantic)
- [Why Use Pydantic?](#why-use-pydantic)
- [What is Uvicorn?](#what-is-uvicorn)
- [Why Use Uvicorn?](#why-use-uvicorn)
- [Installation](#installation)
- [Usage](#usage)
- [Advanced Pydantic Features](#advanced-pydantic-features)
- [Running Tests](#running-tests)
- [Production Deployment](#production-deployment)
- [Troubleshooting](#troubleshooting)
- [Project Structure](#project-structure)
- [Contributing](#contributing)


## Overview
The Bookstore API allows users to manage book entries with validated data models. It leverages:
- **FastAPI**: A modern, high-performance Python web framework for building APIs.
- **Pydantic**: For type-safe data validation and serialization using Python type hints.
- **Uvicorn**: A lightning-fast ASGI server for serving the application.

## What is Pydantic?
Pydantic is a Python library for data validation and serialization using Python type hints. It enables developers to define data schemas via Python classes, leveraging type annotations to enforce data validation and ensure data integrity. Pydantic is widely used with frameworks like FastAPI for handling data models efficiently.

## Benefits of Pydantic
- **Type-Safe Validation**: Uses Python type hints to validate data, catching errors early and reducing bugs.
- **Fast Performance**: Core validation logic is written in Rust, making it up to 17x faster than Pydantic V1 in some cases ([Pydantic Docs](https://docs.pydantic.dev)).
- **JSON Schema Generation**: Automatically generates JSON schemas for integration with tools like Swagger UI.
- **Strict and Lax Modes**: Supports strict validation (no type coercion) or lax mode (automatic type conversion).
- **IDE Integration**: Works seamlessly with IDEs and static type checkers like mypy for better developer experience.
- **Custom Validators**: Enables custom validation logic for complex requirements ([Real Python](https://realpython.com)).
- **Settings Management**: Simplifies handling of environment variables via `pydantic-settings` ([FastAPI Docs](https://fastapi.tiangolo.com)).
- **Serialization**: Easily converts Python objects to JSON and vice versa, ideal for APIs ([LibHunt](https://www.libhunt.com)).

## Why Use Pydantic?
Pydantic is used to:
- Ensure data conforms to expected types and constraints, reducing runtime errors.
- Simplify data parsing and serialization in APIs, particularly with FastAPI.
- Provide clear, type-safe data models for better code maintainability.
- Enable automatic generation of API documentation via JSON schemas.
- Handle complex data validation with minimal boilerplate code ([Pydantic Docs](https://docs.pydantic.dev)).

## What is Uvicorn?
Uvicorn is a high-performance ASGI (Asynchronous Server Gateway Interface) server implementation for Python, built using `uvloop` and `httptools`. It is commonly used to serve web applications built with FastAPI, Starlette, or other ASGI-compatible frameworks ([Uvicorn Docs](https://www.uvicorn.org)).

## Why Use Uvicorn?
- **High Performance**: Leverages `uvloop` for an asynchronous event loop, offering performance comparable to Node.js or Go ([FastAPI Docs](https://fastapi.tiangolo.com)).
- **ASGI Compatibility**: Supports modern Python web frameworks, enabling asynchronous features like WebSockets and background tasks.
- **Auto-Reload for Development**: The `--reload` flag enables automatic server restarts during development.
- **Production-Ready with Gunicorn**: Can be paired with Gunicorn for scalable, multi-process deployments.
- **Lightweight and Simple**: Easy to configure and run with minimal setup ([Uvicorn Docs](https://www.uvicorn.org)).

## Installation
1. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   cd bookstore-api
   ```

2. **Create and Activate a Virtual Environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
   Create a `requirements.txt` with:
   ```
   fastapi>=0.115.0
   pydantic>=2.9.2
   uvicorn>=0.30.6
   pydantic-settings>=2.5.2
   pytest>=8.3.3
   ```

## Usage
1. **Run the Application**:
   Save the following code in `main.py`:
   ```python
   from fastapi import FastAPI
   from pydantic import BaseModel
   from typing import Optional

   app = FastAPI()

   class Book(BaseModel):
       title: str
       author: str
       publication_year: int
       isbn: str
       available_copies: Optional[int] = 5

   @app.post("/books/")
   async def create_book(book: Book):
       return {"message": "Book created", "book": book}

   if __name__ == "__main__":
       import uvicorn
       uvicorn.run(app, host="127.0.0.1", port=8000)
   ```

2. **Start the Server**:
   ```bash
   uvicorn main:app --host 127.0.0.1 --port 8000 --reload
   ```
   - `main:app` refers to the `app` object in `main.py`.
   - `--reload` enables auto-reload for development.
   - Access the API at `http://127.0.0.1:8000` and interactive docs at `http://127.0.0.1:8000/docs`.

3. **Test the API**:
   Send a POST request using `curl` or Postman:
   ```bash
   curl -X POST "http://localhost:8000/books/" -H "Content-Type: application/json" -d '{"title": "Mastering Python", "author": "John Doe", "publication_year": 2023, "isbn": "1234567890", "available_copies": 5}'
   ```

## Advanced Pydantic Features
### Custom Validators
Add custom validation logic to Pydantic models. Example:
```python
from pydantic import BaseModel, validator
from typing import Optional

class Book(BaseModel):
    title: str
    author: str
    publication_year: int
    isbn: str
    available_copies: Optional[int] = 5

    @validator("publication_year")
    def check_year(cls, v):
        if v < 1800 or v > 2025:
            raise ValueError("Publication year must be between 1800 and 2025")
        return v
```

### Settings Management
Use `pydantic-settings` to manage environment variables:
```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    app_name: str = "Bookstore API"
    debug: bool = False

    class Config:
        env_file = ".env"

settings = Settings()
print(settings.app_name)  # Output: Bookstore API
```

## Running Tests
1. Install `pytest`:
   ```bash
   pip install pytest
   ```

2. Create a test file (`test_main.py`):
   ```python
   from fastapi.testclient import TestClient
   from main import app

   client = TestClient(app)

   def test_create_book():
       response = client.post("/books/", json={
           "title": "Test Book",
           "author": "Test Author",
           "publication_year": 2023,
           "isbn": "1234567890",
           "available_copies": 5
       })
       assert response.status_code == 200
       assert response.json()["message"] == "Book created"
   ```

3. Run tests:
   ```bash
   pytest
   ```

## Production Deployment
For production, use Gunicorn with Uvicorn workers for scalability:
```bash
pip install gunicorn
gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app
```
- `-w 4`: Runs 4 worker processes.
- Configure environment variables (e.g., `PORT`, `HOST`) in a `.env` file.
- Use a reverse proxy like Nginx for load balancing and SSL termination.

## Troubleshooting
- **Pydantic Errors**: Check validation errors in the console or enable `debug=True` in your FastAPI app.
- **Uvicorn Issues**: Ensure the correct Python version (3.7+) and verify `uvloop` compatibility on your system.
- **Common Fixes**: Update dependencies (`pip install --upgrade pydantic uvicorn fastapi`) or check logs for detailed errors.
- **Community Support**: Visit [Pydantic GitHub](https://github.com/pydantic/pydantic) or [Uvicorn GitHub](https://github.com/encode/uvicorn) for issues and discussions.

