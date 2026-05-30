# FastAPI

## Table of Contents

1. Introduction
2. Why FastAPI?
3. Installation and Project Setup
4. Your First API
5. Path Parameters
6. Query Parameters
7. Request Bodies with Pydantic
8. Response Models
9. Dependency Injection
10. Validation
11. Error Handling
12. Authentication and Authorization
13. Database Integration with SQLAlchemy
14. Async Programming
15. Background Tasks
16. Middleware
17. Testing with Pytest
18. Configuration Management
19. Logging
20. Deployment
21. Best Practices
22. Example Project Structure

---

# 1. Introduction

FastAPI is a modern Python web framework for building APIs with:

- High performance
- Automatic OpenAPI documentation
- Type safety
- Async support
- Dependency injection

It is built on:

- Starlette (web layer)
- Pydantic (data validation)

---

# 2. Why FastAPI?

Advantages:

- Excellent developer experience
- Automatic validation
- Built-in Swagger UI
- High performance comparable to Node.js and Go
- Native async/await support

---

# 3. Installation and Project Setup

```bash
python -m venv .venv
source .venv/bin/activate

pip install fastapi uvicorn
```

Run:

```bash
uvicorn main:app --reload
```

---

# 4. Your First API

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello FastAPI"}
```

Swagger UI:

- http://localhost:8000/docs

ReDoc:

- http://localhost:8000/redoc

---

# 5. Path Parameters

```python
@app.get("/users/{user_id}")
async def get_user(user_id: int):
    return {"user_id": user_id}
```

FastAPI automatically converts and validates types.

---

# 6. Query Parameters

```python
@app.get("/products")
async def list_products(
    page: int = 1,
    limit: int = 20
):
    return {
        "page": page,
        "limit": limit
    }
```

Example:

```text
/products?page=2&limit=50
```

---

# 7. Request Bodies with Pydantic

```python
from pydantic import BaseModel

class UserCreate(BaseModel):
    username: str
    email: str
    age: int

@app.post("/users")
async def create_user(user: UserCreate):
    return user
```

Benefits:

- Validation
- Serialization
- Documentation generation

---

# 8. Response Models

```python
class UserResponse(BaseModel):
    id: int
    username: str

@app.get(
    "/users/{user_id}",
    response_model=UserResponse
)
async def get_user(user_id: int):
    return {
        "id": user_id,
        "username": "alice",
        "password": "secret"
    }
```

The password is automatically excluded.

---

# 9. Dependency Injection

```python
from fastapi import Depends

def get_current_user():
    return {"username": "alice"}

@app.get("/profile")
async def profile(
    user=Depends(get_current_user)
):
    return user
```

Useful for:

- Authentication
- Database sessions
- Services

---

# 10. Validation

```python
from pydantic import Field

class Product(BaseModel):
    name: str
    price: float = Field(gt=0)
    stock: int = Field(ge=0)
```

Validation errors automatically return HTTP 422.

---

# 11. Error Handling

```python
from fastapi import HTTPException

@app.get("/items/{item_id}")
async def get_item(item_id: int):

    if item_id != 1:
        raise HTTPException(
            status_code=404,
            detail="Item not found"
        )

    return {"id": item_id}
```

---

# 12. Authentication and Authorization

OAuth2 example:

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(
    tokenUrl="token"
)

@app.get("/secure")
async def secure_route(
    token: str = Depends(oauth2_scheme)
):
    return {"token": token}
```

Common production choices:

- JWT
- OAuth2
- OpenID Connect

---

# 13. Database Integration with SQLAlchemy

Install:

```bash
pip install sqlalchemy psycopg[binary]
```

Session dependency:

```python
from sqlalchemy.orm import Session

def get_db():
    db = SessionLocal()

    try:
        yield db
    finally:
        db.close()
```

Usage:

```python
@app.get("/users")
def get_users(
    db: Session = Depends(get_db)
):
    return db.query(User).all()
```

---

# 14. Async Programming

```python
@app.get("/external-data")
async def get_data():
    data = await service.fetch_data()
    return data
```

Use async when:

- Network requests
- Database drivers supporting async
- File operations

Avoid unnecessary async for CPU-heavy work.

---

# 15. Background Tasks

```python
from fastapi import BackgroundTasks

def send_email(email: str):
    print(f"Email sent to {email}")

@app.post("/register")
async def register(
    email: str,
    tasks: BackgroundTasks
):
    tasks.add_task(send_email, email)

    return {"status": "queued"}
```

---

# 16. Middleware

```python
from fastapi import Request

@app.middleware("http")
async def log_requests(
    request: Request,
    call_next
):
    response = await call_next(request)
    return response
```

Common middleware:

- Logging
- Authentication
- Metrics
- CORS

---

# 17. Testing with Pytest

```python
from fastapi.testclient import TestClient

client = TestClient(app)

def test_root():
    response = client.get("/")

    assert response.status_code == 200
```

Run:

```bash
pytest
```

---

# 18. Configuration Management

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str

    class Config:
        env_file = ".env"

settings = Settings()
```

Example:

```env
DATABASE_URL=postgresql://...
SECRET_KEY=mysecret
```

---

# 19. Logging

```python
import logging

logger = logging.getLogger(__name__)

@app.get("/health")
async def health():
    logger.info("Health check")
    return {"status": "ok"}
```

Recommended:

- Structured logging
- Request IDs
- Centralized log aggregation

---

# 20. Deployment

Production server:

```bash
gunicorn \
    -k uvicorn.workers.UvicornWorker \
    main:app
```

Common deployment targets:

- Docker
- Kubernetes
- AWS
- Azure
- Google Cloud

---

# 21. Best Practices

## Use Dependency Injection

Keep business logic separate from endpoints.

## Use Response Models

Avoid leaking internal fields.

## Organize by Feature

```text
app/
├── api/
├── services/
├── repositories/
├── models/
├── schemas/
└── core/
```

## Prefer Async for I/O

Use async for:

- HTTP calls
- Databases
- Message queues

## Write Tests

Cover:

- Unit tests
- Integration tests
- API tests

---

# 22. Example Project Structure

```text
app/
├── api/
│   ├── users.py
│   └── products.py
│
├── core/
│   ├── config.py
│   └── security.py
│
├── db/
│   ├── session.py
│   └── models.py
│
├── schemas/
│   ├── user.py
│   └── product.py
│
├── services/
│   ├── user_service.py
│   └── product_service.py
│
├── main.py
└── tests/
```

---

# Recommended Learning Path

1. Routing
2. Pydantic Models
3. Dependency Injection
4. Authentication
5. SQLAlchemy
6. Async Programming
7. Testing
8. Docker
9. CI/CD
10. Kubernetes
