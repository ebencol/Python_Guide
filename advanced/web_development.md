# Web Development in Python with FastAPI, Django, and Flask

## Table of Contents

1. Introduction
2. Choosing the Right Framework
3. Python Web Development Fundamentals
4. Environment Setup
5. Advanced Flask
   - Architecture
   - Routing
   - Middleware
   - Database Integration
   - Authentication
   - REST APIs
   - Testing
   - Deployment
6. Advanced Django
   - Architecture
   - ORM Deep Dive
   - Class-Based Views
   - Authentication & Permissions
   - Django REST Framework
   - Async Django
   - Caching
   - Testing
   - Deployment
7. Advanced FastAPI
   - Architecture
   - Dependency Injection
   - Pydantic Models
   - Async Programming
   - Security
   - Background Tasks
   - WebSockets
   - Testing
   - Deployment
8. Comparing Flask, Django, and FastAPI
9. Database Design & Optimization
10. Authentication Strategies
11. API Design Best Practices
12. Caching Strategies
13. Background Jobs & Task Queues
14. Real-Time Applications
15. Microservices Architecture
16. Containerization with Docker
17. CI/CD for Python Web Applications
18. Monitoring & Observability
19. Security Best Practices
20. Performance Optimization
21. Scaling Python Web Applications
22. Production Deployment Patterns
23. Project Structure Recommendations
24. Conclusion

---

# 1. Introduction

Python has become one of the dominant languages for backend and full-stack web development. Three frameworks stand out in modern Python web development:

| Framework | Best For | Strengths |
|---|---|---|
| Flask | Lightweight APIs & microservices | Simplicity, flexibility |
| Django | Large monolithic applications | Batteries included |
| FastAPI | High-performance APIs | Async-first, type safety |

This tutorial explores advanced concepts in all three frameworks, including architecture, scaling, optimization, testing, deployment, and production-grade patterns.

---

# 2. Choosing the Right Framework

## Flask

### Advantages

- Minimalistic and flexible
- Easy to learn
- Excellent for microservices
- Large ecosystem of extensions

### Disadvantages

- Requires manual architecture decisions
- Less opinionated
- More boilerplate for large systems

### Ideal Use Cases

- Small to medium APIs
- Microservices
- Prototypes
- Lightweight backend services

---

## Django

### Advantages

- Full-featured framework
- Powerful ORM
- Admin panel included
- Authentication included
- Excellent ecosystem

### Disadvantages

- Heavyweight for small projects
- ORM abstractions can become complex
- Less flexibility

### Ideal Use Cases

- Enterprise applications
- CMS systems
- SaaS products
- Large monoliths

---

## FastAPI

### Advantages

- Extremely fast
- Async-native
- Automatic OpenAPI generation
- Strong typing with Pydantic
- Excellent developer experience

### Disadvantages

- Smaller ecosystem compared to Django
- Async complexity
- Less mature than Flask/Django

### Ideal Use Cases

- High-performance APIs
- AI/ML services
- Real-time systems
- Async-heavy workloads

---

# 3. Python Web Development Fundamentals

## WSGI vs ASGI

### WSGI

Traditional synchronous Python web interface.

Used by:

- Flask
- Django (traditionally)

### ASGI

Modern asynchronous server interface.

Used by:

- FastAPI
- Starlette
- Modern Django async views

---

## Request Lifecycle

### Typical Flow

```text
Client -> Web Server -> Middleware -> Router -> Controller/View -> Database -> Response
```

---

## HTTP Concepts

### Important HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve data |
| POST | Create data |
| PUT | Replace resource |
| PATCH | Partial update |
| DELETE | Remove resource |

---

# 4. Environment Setup

## Virtual Environments

```bash
python -m venv venv
source venv/bin/activate
```

---

## Dependency Management

### pip-tools

```bash
pip install pip-tools
```

Compile dependencies:

```bash
pip-compile requirements.in
```

Install:

```bash
pip-sync
```

---

## Recommended Tooling

| Tool | Purpose |
|---|---|
| Black | Formatting |
| Ruff | Linting |
| MyPy | Static typing |
| Pytest | Testing |
| Docker | Containerization |
| Poetry | Packaging |

---

# 5. Advanced Flask

# Flask Architecture

## Application Factory Pattern

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy


db = SQLAlchemy()


def create_app(config_object="config.Config"):
    app = Flask(__name__)
    app.config.from_object(config_object)

    db.init_app(app)

    from .routes import api_bp
    app.register_blueprint(api_bp)

    return app
```

---

## Blueprints

```python
from flask import Blueprint

api_bp = Blueprint("api", __name__, url_prefix="/api")


@api_bp.route("/health")
def health():
    return {"status": "ok"}
```

---

# Flask Database Integration

## SQLAlchemy Models

```python
from datetime import datetime

from .extensions import db


class User(db.Model):
    __tablename__ = "users"

    id = db.Column(db.Integer, primary_key=True)
    email = db.Column(db.String(255), unique=True, nullable=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
```

---

## Database Migrations

```bash
flask db init
flask db migrate -m "initial migration"
flask db upgrade
```

---

# Flask Authentication

## JWT Authentication

```python
from flask_jwt_extended import (
    JWTManager,
    create_access_token,
    jwt_required,
)

jwt = JWTManager()


@app.route("/login", methods=["POST"])
def login():
    token = create_access_token(identity="user_id")
    return {"access_token": token}


@app.route("/profile")
@jwt_required()
def profile():
    return {"message": "protected route"}
```

---

# Flask Middleware

## Custom Middleware

```python
from time import time


@app.before_request
def before_request():
    g.start_time = time()


@app.after_request
def after_request(response):
    duration = time() - g.start_time
    response.headers["X-Response-Time"] = str(duration)
    return response
```

---

# Flask REST APIs

## Marshmallow Serialization

```python
from marshmallow import Schema, fields


class UserSchema(Schema):
    id = fields.Int()
    email = fields.Email()
```

---

# Flask Testing

## Pytest Example

```python
import pytest

from app import create_app


@pytest.fixture
def app():
    app = create_app("config.TestConfig")
    yield app


def test_health(client):
    response = client.get("/api/health")
    assert response.status_code == 200
```

---

# Flask Deployment

## Gunicorn

```bash
gunicorn -w 4 -b 0.0.0.0:8000 app:create_app()
```

---

# 6. Advanced Django

# Django Architecture

## Django Project Structure

```text
project/
├── manage.py
├── project/
│   ├── settings.py
│   ├── urls.py
│   └── asgi.py
├── users/
├── products/
└── orders/
```

---

# Django ORM Deep Dive

## Advanced Models

```python
from django.db import models


class TimestampMixin(models.Model):
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        abstract = True


class Product(TimestampMixin):
    name = models.CharField(max_length=255)
    price = models.DecimalField(max_digits=10, decimal_places=2)
```

---

## Query Optimization

### select_related

```python
orders = Order.objects.select_related("customer")
```

### prefetch_related

```python
orders = Order.objects.prefetch_related("items")
```

---

## Custom Managers

```python
class PublishedManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(published=True)


class Article(models.Model):
    published = models.BooleanField(default=False)

    objects = models.Manager()
    published_objects = PublishedManager()
```

---

# Django Class-Based Views

## Generic Views

```python
from django.views.generic import ListView

from .models import Product


class ProductListView(ListView):
    model = Product
    template_name = "products/list.html"
    paginate_by = 20
```

---

# Django REST Framework

## Serializer

```python
from rest_framework import serializers

from .models import Product


class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = "__all__"
```

---

## ViewSet

```python
from rest_framework.viewsets import ModelViewSet


class ProductViewSet(ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

---

## Router

```python
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register(r"products", ProductViewSet)
```

---

# Django Authentication

## Custom User Model

```python
from django.contrib.auth.models import AbstractUser


class User(AbstractUser):
    bio = models.TextField(blank=True)
```

---

# Async Django

## Async Views

```python
from django.http import JsonResponse


async def async_view(request):
    return JsonResponse({"message": "async response"})
```

---

# Django Caching

## Redis Cache

```python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://localhost:6379/1",
    }
}
```

---

# Django Testing

## TestCase Example

```python
from django.test import TestCase


class ProductTests(TestCase):
    def test_create_product(self):
        product = Product.objects.create(name="Laptop")
        self.assertEqual(product.name, "Laptop")
```

---

# Django Deployment

## ASGI Deployment

```bash
gunicorn project.asgi:application \
    -k uvicorn.workers.UvicornWorker
```

---

# 7. Advanced FastAPI

# FastAPI Architecture

## Basic Application

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/health")
async def health():
    return {"status": "ok"}
```

---

# Pydantic Models

## Request Validation

```python
from pydantic import BaseModel, EmailStr


class UserCreate(BaseModel):
    email: EmailStr
    password: str
```

---

# Dependency Injection

## Database Session Dependency

```python
from sqlalchemy.orm import Session


def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

---

## Using Dependencies

```python
from fastapi import Depends


@app.get("/users")
def get_users(db: Session = Depends(get_db)):
    return db.query(User).all()
```

---

# Async Programming

## Async Database Access

```python
from sqlalchemy.ext.asyncio import AsyncSession


async def get_users(db: AsyncSession):
    result = await db.execute(select(User))
    return result.scalars().all()
```

---

# FastAPI Security

## OAuth2 Password Flow

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")
```

---

# Background Tasks

```python
from fastapi import BackgroundTasks


def send_email(email: str):
    print(f"Sending email to {email}")


@app.post("/register")
def register(background_tasks: BackgroundTasks):
    background_tasks.add_task(send_email, "user@example.com")
    return {"status": "registered"}
```

---

# WebSockets

```python
from fastapi import WebSocket


@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()

    while True:
        data = await websocket.receive_text()
        await websocket.send_text(f"Echo: {data}")
```

---

# FastAPI Testing

```python
from fastapi.testclient import TestClient

client = TestClient(app)


def test_health():
    response = client.get("/health")
    assert response.status_code == 200
```

---

# FastAPI Deployment

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Production:

```bash
gunicorn app.main:app \
    -k uvicorn.workers.UvicornWorker \
    -w 4
```

---

# 8. Comparing Flask, Django, and FastAPI

| Feature | Flask | Django | FastAPI |
|---|---|---|---|
| Performance | Medium | Medium | High |
| Async Support | Limited | Partial | Native |
| ORM Included | No | Yes | No |
| Admin Panel | No | Yes | No |
| Learning Curve | Easy | Medium | Medium |
| Flexibility | High | Medium | High |
| API Development | Good | Good | Excellent |
| Enterprise Apps | Moderate | Excellent | Good |

---

# 9. Database Design & Optimization

# Indexing

## Single Column Index

```python
class Product(models.Model):
    sku = models.CharField(max_length=100, db_index=True)
```

---

## Composite Index

```python
class Meta:
    indexes = [
        models.Index(fields=["category", "created_at"]),
    ]
```

---

# N+1 Query Problem

## Bad

```python
orders = Order.objects.all()

for order in orders:
    print(order.customer.name)
```

---

## Good

```python
orders = Order.objects.select_related("customer")
```

---

# Connection Pooling

## SQLAlchemy Pooling

```python
engine = create_engine(
    DATABASE_URL,
    pool_size=20,
    max_overflow=40,
)
```

---

# 10. Authentication Strategies

# Session Authentication

Traditional server-side authentication.

Best for:

- Server-rendered applications
- Internal dashboards

---

# JWT Authentication

Best for:

- APIs
- Mobile apps
- SPAs
- Microservices

---

# OAuth2

Best for:

- Third-party login
- Enterprise SSO

---

# API Keys

Best for:

- Service-to-service communication
- Internal APIs

---

# 11. API Design Best Practices

# RESTful Naming

## Good

```text
GET /users
POST /users
GET /users/1
DELETE /users/1
```

---

## Bad

```text
GET /getUsers
POST /createUser
```

---

# Pagination

## Cursor Pagination

```python
@app.get("/items")
def get_items(cursor: str | None = None, limit: int = 20):
    pass
```

---

# API Versioning

```text
/api/v1/users
/api/v2/users
```

---

# Idempotency

Use idempotency keys for safe retries.

```http
Idempotency-Key: abc123
```

---

# 12. Caching Strategies

# Redis Caching

## FastAPI Example

```python
import redis

r = redis.Redis(host="localhost", port=6379)
```

---

# Cache Aside Pattern

```python
cached = redis.get(key)

if cached:
    return cached

result = expensive_query()
redis.set(key, result)
```

---

# CDN Caching

Use CDNs for:

- Images
- Static assets
- Public APIs

---

# 13. Background Jobs & Task Queues

# Celery

## Installation

```bash
pip install celery redis
```

---

## Celery Task

```python
from celery import Celery

celery = Celery(__name__, broker="redis://localhost:6379/0")


@celery.task
def send_email(email: str):
    print(email)
```

---

# Task Queues Comparison

| Tool | Best For |
|---|---|
| Celery | Heavy workloads |
| RQ | Simplicity |
| Dramatiq | Modern async queues |
| Huey | Lightweight tasks |

---

# 14. Real-Time Applications

# WebSockets

## FastAPI Chat Example

```python
connections = []


@app.websocket("/chat")
async def chat(websocket: WebSocket):
    await websocket.accept()
    connections.append(websocket)

    while True:
        message = await websocket.receive_text()

        for connection in connections:
            await connection.send_text(message)
```

---

# Server-Sent Events

Useful for:

- Notifications
- Streaming updates
- Live dashboards

---

# 15. Microservices Architecture

# Service Decomposition

Example services:

- Authentication Service
- Billing Service
- Notification Service
- Analytics Service

---

# Communication Patterns

## Synchronous

- REST
- gRPC

## Asynchronous

- Kafka
- RabbitMQ
- Redis Streams

---

# API Gateway

Responsibilities:

- Authentication
- Rate limiting
- Logging
- Routing

---

# 16. Containerization with Docker

# Flask Dockerfile

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["gunicorn", "app:create_app()"]
```

---

# Docker Compose

```yaml
version: '3.9'

services:
  api:
    build: .
    ports:
      - "8000:8000"

  postgres:
    image: postgres:16

  redis:
    image: redis:7
```

---

# 17. CI/CD for Python Web Applications

# GitHub Actions

```yaml
name: CI

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - run: pip install -r requirements.txt
      - run: pytest
```

---

# Deployment Pipelines

Typical stages:

1. Lint
2. Test
3. Build Docker image
4. Push image
5. Deploy
6. Smoke tests

---

# 18. Monitoring & Observability

# Logging

## Structured Logging

```python
import logging

logger = logging.getLogger(__name__)

logger.info(
    "user_login",
    extra={"user_id": 1},
)
```

---

# Metrics

Popular tools:

- Prometheus
- Grafana
- Datadog

---

# Distributed Tracing

Use OpenTelemetry.

```bash
pip install opentelemetry-api
```

---

# 19. Security Best Practices

# Input Validation

Always validate:

- Query parameters
- JSON payloads
- Headers
- Uploaded files

---

# SQL Injection Prevention

## Unsafe

```python
query = f"SELECT * FROM users WHERE id = {user_id}"
```

---

## Safe

```python
User.objects.filter(id=user_id)
```

---

# CSRF Protection

Django includes CSRF protection by default.

Flask requires extensions.

---

# Rate Limiting

## FastAPI Example

```python
from slowapi import Limiter

limiter = Limiter(key_func=get_remote_address)
```

---

# HTTPS Everywhere

Always terminate TLS using:

- NGINX
- Traefik
- Cloudflare

---

# 20. Performance Optimization

# Async I/O

Best for:

- Network-heavy workloads
- Streaming
- High concurrency

---

# Database Optimization

Strategies:

- Proper indexing
- Query optimization
- Connection pooling
- Read replicas

---

# Compression

Enable:

- GZip
- Brotli

---

# Benchmarking

## Load Testing

```bash
wrk -t4 -c100 -d30s http://localhost:8000
```

---

# 21. Scaling Python Web Applications

# Horizontal Scaling

Scale using:

- Kubernetes
- ECS
- Docker Swarm

---

# Stateless Services

Store sessions in:

- Redis
- Database

Avoid in-memory sessions.

---

# Load Balancing

Use:

- NGINX
- HAProxy
- AWS ALB

---

# 22. Production Deployment Patterns

# Reverse Proxy

Example architecture:

```text
Internet
   ↓
NGINX
   ↓
Gunicorn/Uvicorn
   ↓
Python Application
```

---

# Blue-Green Deployment

Benefits:

- Zero downtime
- Easy rollback

---

# Canary Deployment

Gradually shift traffic.

Useful for:

- Risk reduction
- A/B testing

---

# 23. Project Structure Recommendations

# Flask Structure

```text
app/
├── api/
├── models/
├── services/
├── repositories/
├── extensions/
└── tests/
```

---

# Django Structure

```text
project/
├── apps/
│   ├── users/
│   ├── billing/
│   └── products/
├── core/
├── config/
└── tests/
```

---

# FastAPI Structure

```text
app/
├── api/
├── dependencies/
├── services/
├── repositories/
├── schemas/
├── models/
└── tests/
```

---

# 24. Conclusion

Flask, Django, and FastAPI each excel in different areas of modern web development.

## Use Flask When

- You want maximum flexibility
- Building microservices
- Creating lightweight APIs

## Use Django When

- Building large applications
- You need batteries included
- Admin panel is important

## Use FastAPI When

- Performance matters
- Building async APIs
- Strong typing is valuable

Modern production systems often combine multiple frameworks:

- Django for admin/internal tools
- FastAPI for high-performance APIs
- Flask for lightweight services

The most important skill is understanding architecture, scalability, security, and maintainability — not just framework syntax.
