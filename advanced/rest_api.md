# Advanced REST API Development with Python

## Table of Contents
1. Introduction to REST APIs
2. REST Architectural Principles
3. HTTP Fundamentals
4. Choosing a Python Framework
5. Setting Up the Development Environment
6. Building APIs with FastAPI
7. Request Validation with Pydantic
8. Routing and API Versioning
9. Database Integration with SQLAlchemy
10. Authentication and Authorization
11. Dependency Injection
12. Error Handling and Validation
13. Async Programming in APIs
14. Pagination, Filtering, and Sorting
15. API Documentation
16. Testing REST APIs
17. Caching Strategies
18. Rate Limiting
19. Logging and Monitoring
20. Security Best Practices
21. Deployment Strategies
22. Containerization with Docker
23. CI/CD for APIs
24. Performance Optimization
25. Webhooks and Background Tasks
26. Building Production-Ready APIs
27. Real-World Project Structure
28. Advanced Concepts
29. Common REST API Interview Questions
30. Conclusion

---

# 1. Introduction to REST APIs

A REST API (Representational State Transfer Application Programming Interface) allows applications to communicate over HTTP using standard methods such as:

| Method | Purpose |
|---|---|
| GET | Retrieve data |
| POST | Create data |
| PUT | Replace data |
| PATCH | Partially update data |
| DELETE | Remove data |

REST APIs are widely used in:

- Web applications
- Mobile applications
- Microservices
- IoT systems
- Cloud-native architectures
- SaaS platforms

Python is one of the best languages for building APIs because of:

- Readability
- Large ecosystem
- Strong async support
- Excellent frameworks
- Rapid development speed

---

# 2. REST Architectural Principles

## Statelessness

Each request must contain all required information.

```http
GET /users/10 HTTP/1.1
Authorization: Bearer token
```

The server should not rely on previous requests.

---

## Resource-Based Design

Resources should use nouns:

### Good

```text
/users
/orders
/products
```

### Bad

```text
/getUsers
/createOrder
```

---

## Uniform Interface

Use consistent conventions:

```text
GET /users
GET /users/1
POST /users
DELETE /users/1
```

---

## Client-Server Separation

Frontend and backend evolve independently.

---

## Cacheability

Responses may include:

```http
Cache-Control: max-age=3600
```

---

# 3. HTTP Fundamentals

## HTTP Request Structure

```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer token
```

Body:

```json
{
  "name": "Alice",
  "email": "alice@example.com"
}
```

---

## HTTP Status Codes

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Validation Error |
| 500 | Internal Server Error |

---

## Idempotency

### Idempotent

```http
PUT /users/1
DELETE /users/1
```

### Non-Idempotent

```http
POST /orders
```

---

# 4. Choosing a Python Framework

## FastAPI

### Advantages

- Extremely fast
- Async-first
- Automatic OpenAPI docs
- Pydantic validation
- Type hints
- Modern architecture

### Best For

- High-performance APIs
- Microservices
- Async applications

---

## Flask

### Advantages

- Minimalistic
- Flexible
- Large ecosystem

### Best For

- Small APIs
- Rapid prototyping

---

## Django REST Framework

### Advantages

- Batteries included
- ORM
- Authentication system
- Admin panel

### Best For

- Enterprise applications
- Large monoliths

---

# 5. Setting Up the Development Environment

## Create Virtual Environment

```bash
python -m venv venv
```

### Activate

Linux/macOS:

```bash
source venv/bin/activate
```

Windows:

```powershell
venv\Scripts\activate
```

---

## Install Dependencies

```bash
pip install fastapi uvicorn sqlalchemy psycopg2-binary pydantic python-jose passlib[bcrypt]
```

---

# 6. Building APIs with FastAPI

## First API

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {"message": "Hello REST API"}
```

Run:

```bash
uvicorn main:app --reload
```

---

## Path Parameters

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

---

## Query Parameters

```python
@app.get("/products")
def get_products(limit: int = 10):
    return {"limit": limit}
```

---

## Request Body

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    email: str

@app.post("/users")
def create_user(user: User):
    return user
```

---

# 7. Request Validation with Pydantic

## Validation Example

```python
from pydantic import BaseModel, EmailStr, Field

class UserCreate(BaseModel):
    name: str = Field(min_length=2, max_length=100)
    email: EmailStr
    age: int = Field(gt=0, lt=120)
```

---

## Nested Models

```python
class Address(BaseModel):
    city: str
    country: str

class User(BaseModel):
    name: str
    address: Address
```

---

## Response Models

```python
class UserResponse(BaseModel):
    id: int
    name: str

@app.get("/users/{id}", response_model=UserResponse)
def get_user(id: int):
    return {"id": id, "name": "Alice"}
```

---

# 8. Routing and API Versioning

## APIRouter

```python
from fastapi import APIRouter

router = APIRouter(prefix="/users", tags=["Users"])

@router.get("/")
def get_users():
    return []
```

---

## Include Router

```python
app.include_router(router)
```

---

## API Versioning

```text
/api/v1/users
/api/v2/users
```

Example:

```python
v1_router = APIRouter(prefix="/api/v1")
```

---

# 9. Database Integration with SQLAlchemy

## Install SQLAlchemy

```bash
pip install sqlalchemy
```

---

## Database Connection

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "postgresql://user:password@localhost/db"

engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(bind=engine)
```

---

## ORM Model

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True)
    name = Column(String)
    email = Column(String, unique=True)
```

---

## Dependency Injection for DB

```python
from fastapi import Depends


def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

---

## CRUD Example

```python
@app.post("/users")
def create_user(user: UserCreate, db=Depends(get_db)):
    db_user = User(name=user.name, email=user.email)
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user
```

---

# 10. Authentication and Authorization

## JWT Authentication

Install:

```bash
pip install python-jose passlib[bcrypt]
```

---

## Password Hashing

```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"])

hashed = pwd_context.hash("secret")
```

---

## Create JWT Token

```python
from jose import jwt
from datetime import datetime, timedelta

SECRET_KEY = "super-secret"
ALGORITHM = "HS256"


def create_access_token(data: dict):
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(minutes=30)
    to_encode.update({"exp": expire})

    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
```

---

## OAuth2 Example

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login")
```

---

## Protected Route

```python
@app.get("/profile")
def profile(token: str = Depends(oauth2_scheme)):
    return {"token": token}
```

---

# 11. Dependency Injection

FastAPI dependency injection makes code reusable.

## Authentication Dependency

```python
async def get_current_user(token: str = Depends(oauth2_scheme)):
    return decode_token(token)
```

---

## Route Using Dependency

```python
@app.get("/dashboard")
def dashboard(user=Depends(get_current_user)):
    return user
```

---

# 12. Error Handling and Validation

## HTTP Exceptions

```python
from fastapi import HTTPException

@app.get("/users/{id}")
def get_user(id: int):
    if id != 1:
        raise HTTPException(status_code=404, detail="User not found")

    return {"id": id}
```

---

## Custom Exception Handler

```python
from fastapi.responses import JSONResponse

@app.exception_handler(ValueError)
async def value_error_handler(request, exc):
    return JSONResponse(
        status_code=400,
        content={"message": str(exc)}
    )
```

---

# 13. Async Programming in APIs

## Async Endpoint

```python
@app.get("/async")
async def async_endpoint():
    return {"message": "Async route"}
```

---

## Why Async Matters

Benefits:

- Handles many connections
- Better throughput
- Non-blocking I/O
- Great for APIs calling external services

---

## Async Database Example

```python
from sqlalchemy.ext.asyncio import create_async_engine
```

---

# 14. Pagination, Filtering, and Sorting

## Pagination

```python
@app.get("/items")
def get_items(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}
```

---

## Filtering

```python
@app.get("/products")
def get_products(category: str | None = None):
    return {"category": category}
```

---

## Sorting

```python
@app.get("/products")
def get_products(sort_by: str = "price"):
    return {"sort_by": sort_by}
```

---

# 15. API Documentation

FastAPI automatically generates:

| URL | Purpose |
|---|---|
| /docs | Swagger UI |
| /redoc | ReDoc |

---

## Add Metadata

```python
app = FastAPI(
    title="Advanced REST API",
    version="1.0.0",
    description="Production-ready API"
)
```

---

# 16. Testing REST APIs

## Install Pytest

```bash
pip install pytest httpx
```

---

## Test Example

```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)


def test_root():
    response = client.get("/")

    assert response.status_code == 200
```

---

## Async Testing

```python
import pytest
from httpx import AsyncClient

@pytest.mark.asyncio
async def test_async():
    async with AsyncClient(app=app, base_url="http://test") as ac:
        response = await ac.get("/")

    assert response.status_code == 200
```

---

# 17. Caching Strategies

## Why Cache?

Caching improves:

- Performance
- Scalability
- Response times

---

## Redis Example

```bash
pip install redis
```

---

```python
import redis

r = redis.Redis(host="localhost", port=6379)
```

---

## Cache Response

```python
cached = r.get("users")
```

---

# 18. Rate Limiting

Protect APIs against abuse.

## Install SlowAPI

```bash
pip install slowapi
```

---

## Example

```python
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)
```

---

## Route Limiting

```python
@app.get("/limited")
@limiter.limit("5/minute")
def limited():
    return {"message": "Limited"}
```

---

# 19. Logging and Monitoring

## Basic Logging

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
```

---

## Log Example

```python
logger.info("User logged in")
```

---

## Monitoring Tools

| Tool | Purpose |
|---|---|
| Prometheus | Metrics |
| Grafana | Visualization |
| Sentry | Error tracking |
| ELK Stack | Centralized logging |

---

# 20. Security Best Practices

## Use HTTPS

Never expose sensitive APIs over HTTP.

---

## Validate Input

Always validate:

- JSON bodies
- Headers
- Query params
- Uploaded files

---

## Avoid SQL Injection

### Unsafe

```python
query = f"SELECT * FROM users WHERE id={user_id}"
```

### Safe

Use ORM or parameterized queries.

---

## Secure Headers

```http
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'
```

---

## Store Secrets Securely

Use:

- Environment variables
- Secret managers
- Vault systems

Never hardcode secrets.

---

# 21. Deployment Strategies

## Uvicorn

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

---

## Gunicorn + Uvicorn Workers

```bash
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
```

---

## Reverse Proxy with Nginx

Benefits:

- SSL termination
- Load balancing
- Static file serving
- Better performance

---

# 22. Containerization with Docker

## Dockerfile

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## Build Image

```bash
docker build -t fastapi-app .
```

---

## Run Container

```bash
docker run -p 8000:8000 fastapi-app
```

---

# 23. CI/CD for APIs

## GitHub Actions Example

```yaml
name: CI

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: pytest
```

---

# 24. Performance Optimization

## Use Async

Async endpoints improve throughput.

---

## Use Connection Pooling

Database connection pooling reduces latency.

---

## Compress Responses

```python
from fastapi.middleware.gzip import GZipMiddleware

app.add_middleware(GZipMiddleware)
```

---

## Minimize Database Queries

Avoid N+1 query problems.

---

## Profile Performance

Tools:

- cProfile
- Pyinstrument
- Locust

---

# 25. Webhooks and Background Tasks

## Background Tasks

```python
from fastapi import BackgroundTasks


def send_email(email: str):
    print(f"Email sent to {email}")

@app.post("/register")
def register(email: str, background_tasks: BackgroundTasks):
    background_tasks.add_task(send_email, email)
    return {"message": "Registration successful"}
```

---

## Webhook Example

```python
@app.post("/webhook")
async def webhook(payload: dict):
    print(payload)
    return {"status": "received"}
```

---

# 26. Building Production-Ready APIs

## Recommended Stack

| Component | Recommendation |
|---|---|
| Framework | FastAPI |
| Database | PostgreSQL |
| ORM | SQLAlchemy |
| Cache | Redis |
| Queue | Celery |
| Reverse Proxy | Nginx |
| Container | Docker |
| Monitoring | Prometheus + Grafana |

---

## Production Checklist

- Input validation
- Authentication
- Authorization
- Logging
- Monitoring
- Rate limiting
- HTTPS
- Database migrations
- Testing
- Documentation
- CI/CD

---

# 27. Real-World Project Structure

```text
project/
│
├── app/
│   ├── main.py
│   ├── routers/
│   ├── models/
│   ├── schemas/
│   ├── services/
│   ├── repositories/
│   ├── database/
│   ├── core/
│   ├── middleware/
│   └── tests/
│
├── requirements.txt
├── Dockerfile
├── .env
└── README.md
```

---

# 28. Advanced Concepts

## API Gateway

Responsibilities:

- Authentication
- Rate limiting
- Routing
- Load balancing

Popular tools:

- Kong
- NGINX
- Traefik
- AWS API Gateway

---

## CQRS Pattern

Separate:

- Read operations
- Write operations

Improves scalability.

---

## Event-Driven Architecture

Use message brokers:

- RabbitMQ
- Kafka
- Redis Streams

---

## API Version Deprecation

Strategies:

- Header-based warnings
- Sunset headers
- Gradual migration

---

# 29. Common REST API Interview Questions

## What is REST?

An architectural style using stateless HTTP communication.

---

## Difference Between PUT and PATCH?

| PUT | PATCH |
|---|---|
| Full replacement | Partial update |
| Idempotent | Usually idempotent |

---

## What Makes REST Scalable?

- Statelessness
- Caching
- Layered systems
- Resource-based design

---

## What is JWT?

A compact token format for authentication.

---

## What is Idempotency?

Repeated requests produce the same result.

---

# 30. Conclusion

You now understand:

- REST fundamentals
- HTTP principles
- FastAPI development
- Authentication
- Async APIs
- SQLAlchemy integration
- Docker deployment
- Production best practices
- Performance optimization
- API security
- Monitoring and scaling
