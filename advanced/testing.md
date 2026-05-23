# Testing in Python

## Table of Contents
1. Introduction to Testing in Python
2. Testing Pyramid
3. Unit Testing with `pytest`
4. Fixtures and Dependency Injection
5. Parametrized Testing
6. Mocking and Patching
7. Integration Testing
8. End-to-End (E2E) Testing
9. Testing APIs
10. Database Testing
11. Async Testing
12. Performance and Load Testing
13. Test Coverage and Reporting
14. CI/CD Integration
15. Best Practices
16. Real-World Project Structure
17. Advanced `pytest` Plugins
18. Common Anti-Patterns
19. Final Thoughts

---

# 1. Introduction to Testing in Python

Testing ensures software reliability, maintainability, and correctness. In modern Python development, testing is not optional — it is a critical engineering discipline.

## Types of Tests

| Test Type | Purpose | Speed | Scope |
|---|---|---|---|
| Unit Test | Test isolated functions/classes | Fast | Small |
| Integration Test | Verify interaction between modules | Medium | Medium |
| End-to-End Test | Validate complete workflows | Slow | Large |
| Performance Test | Measure speed/scalability | Variable | System-wide |

---

# 2. Testing Pyramid

A healthy testing strategy follows the testing pyramid.
              / \
```text      /   \
           /      \
          /   E2E  \
         /----------\
        /Integration \
       /--------------\
      /   Unit Tests   \
     /__________________\
```

## Key Idea

- Many unit tests
- Fewer integration tests
- Very few E2E tests

This minimizes maintenance cost while maximizing confidence.

---

# 3. Unit Testing with pytest

## Installing pytest

```bash
pip install pytest
```

## Basic Example

### File: `math_utils.py`

```python
def add(a, b):
    return a + b
```

### File: `test_math_utils.py`

```python
from math_utils import add


def test_add():
    assert add(2, 3) == 5
```

## Running Tests

```bash
pytest
```

## Verbose Output

```bash
pytest -v
```

## Running a Specific Test

```bash
pytest test_math_utils.py::test_add
```

---

# 4. Fixtures and Dependency Injection

Fixtures are reusable components for test setup and teardown.

## Basic Fixture

```python
import pytest


@pytest.fixture
def sample_user():
    return {
        "id": 1,
        "name": "Alice",
        "email": "alice@example.com"
    }


def test_user_email(sample_user):
    assert sample_user["email"] == "alice@example.com"
```

## Fixture Scope

```python
@pytest.fixture(scope="module")
def db_connection():
    print("Connect to DB")
    yield
    print("Disconnect from DB")
```

## Available Scopes

| Scope | Lifetime |
|---|---|
| function | Per test |
| class | Per class |
| module | Per module |
| session | Entire test session |

---

# 5. Parametrized Testing

Avoid duplicated test logic using parametrization.

```python
import pytest


@pytest.mark.parametrize(
    "a,b,result",
    [
        (1, 2, 3),
        (5, 5, 10),
        (-1, 1, 0)
    ]
)
def test_add(a, b, result):
    assert a + b == result
```

## Benefits

- Cleaner tests
- Better coverage
- Easier maintenance

---

# 6. Mocking and Patching

Mocking isolates external dependencies.

## Why Mock?

You should mock:

- APIs
- Databases
- File systems
- Third-party services
- Payment gateways

## Using unittest.mock

```python
from unittest.mock import patch


def fetch_data():
    import requests
    response = requests.get("https://api.example.com")
    return response.json()


@patch("requests.get")
def test_fetch_data(mock_get):
    mock_get.return_value.json.return_value = {
        "status": "ok"
    }

    data = fetch_data()

    assert data["status"] == "ok"
```

## Using MagicMock

```python
from unittest.mock import MagicMock


mock_db = MagicMock()
mock_db.save.return_value = True

assert mock_db.save() is True
```

---

# 7. Integration Testing

Integration tests verify that components work together.

## Example Architecture

```text
Client → API → Service → Database
```

## Example with FastAPI

### Application

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/health")
def health_check():
    return {"status": "ok"}
```

### Integration Test

```python
from fastapi.testclient import TestClient
from app import app


client = TestClient(app)


def test_health_check():
    response = client.get("/health")

    assert response.status_code == 200
    assert response.json() == {
        "status": "ok"
    }
```

## Running Integration Tests Separately

```bash
pytest tests/integration
```

## Marking Integration Tests

```python
import pytest


@pytest.mark.integration
def test_database_connection():
    pass
```

Run:

```bash
pytest -m integration
```

---

# 8. End-to-End (E2E) Testing

E2E tests simulate real user workflows.

## Typical E2E Flow

```text
User Login → Browse Products → Add to Cart → Checkout
```

## Tools for E2E Testing

| Tool | Purpose |
|---|---|
| Selenium | Browser automation |
| Playwright | Modern browser automation |
| Cypress | Frontend E2E testing |

---

## Selenium Example

### Installation

```bash
pip install selenium
```

### Basic Example

```python
from selenium import webdriver
from selenium.webdriver.common.by import By


def test_google_search():
    driver = webdriver.Chrome()

    driver.get("https://google.com")

    search_box = driver.find_element(By.NAME, "q")
    search_box.send_keys("pytest tutorial")
    search_box.submit()

    assert "pytest" in driver.title.lower()

    driver.quit()
```

---

## Playwright Example

### Installation

```bash
pip install playwright
playwright install
```

### Example

```python
from playwright.sync_api import sync_playwright


def test_homepage():
    with sync_playwright() as p:
        browser = p.chromium.launch()
        page = browser.new_page()

        page.goto("https://example.com")

        assert "Example" in page.title()

        browser.close()
```

---

# 9. Testing APIs

API testing is one of the most common testing tasks.

## Using requests

```python
import requests


def test_api_status():
    response = requests.get(
        "https://jsonplaceholder.typicode.com/posts/1"
    )

    assert response.status_code == 200
```

## Schema Validation

```python
from pydantic import BaseModel


class Post(BaseModel):
    userId: int
    id: int
    title: str
    body: str


def test_response_schema():
    response = requests.get(
        "https://jsonplaceholder.typicode.com/posts/1"
    )

    Post(**response.json())
```

---

# 10. Database Testing

Testing databases requires isolation and cleanup.

## Using SQLite for Tests

```python
import sqlite3
import pytest


@pytest.fixture
def db_connection():
    conn = sqlite3.connect(":memory:")
    yield conn
    conn.close()


def test_insert_user(db_connection):
    cursor = db_connection.cursor()

    cursor.execute(
        "CREATE TABLE users(id INTEGER, name TEXT)"
    )

    cursor.execute(
        "INSERT INTO users VALUES(1, 'Alice')"
    )

    cursor.execute("SELECT * FROM users")

    user = cursor.fetchone()

    assert user == (1, 'Alice')
```

---

# 11. Async Testing

Modern Python applications often use asynchronous programming.

## Installing pytest-asyncio

```bash
pip install pytest-asyncio
```

## Example

```python
import asyncio
import pytest


async def fetch_data():
    await asyncio.sleep(1)
    return {
        "status": "ok"
    }


@pytest.mark.asyncio
async def test_fetch_data():
    result = await fetch_data()

    assert result["status"] == "ok"
```

---

# 12. Performance and Load Testing

Performance tests validate scalability and latency.

## Common Metrics

- Response time
- Throughput
- Memory usage
- CPU usage
- Error rate

## Using pytest-benchmark

### Installation

```bash
pip install pytest-benchmark
```

### Example

```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)


def test_fibonacci_performance(benchmark):
    benchmark(fibonacci, 20)
```

---

# 13. Test Coverage and Reporting

Coverage measures how much code is tested.

## Installing Coverage Plugin

```bash
pip install pytest-cov
```

## Generate Coverage Report

```bash
pytest --cov=app
```

## HTML Report

```bash
pytest --cov=app --cov-report=html
```

Generated report:

```text
htmlcov/index.html
```

## Coverage Goals

| Project Type | Recommended Coverage |
|---|---|
| Small apps | 80% |
| Enterprise systems | 90%+ |
| Critical systems | 95%+ |

---

# 14. CI/CD Integration

Automated testing is essential in CI/CD pipelines.

## GitHub Actions Example

### `.github/workflows/tests.yml`

```yaml
name: Python Tests

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install pytest pytest-cov

      - name: Run tests
        run: pytest --cov=app
```

---

# 15. Best Practices

## 1. Keep Tests Independent

Bad:

```python
state = []


def test_one():
    state.append(1)


def test_two():
    assert len(state) == 0
```

Good:

```python
@pytest.fixture
def state():
    return []
```

---

## 2. Use Descriptive Test Names

Bad:

```python
def test_a():
    pass
```

Good:

```python
def test_login_rejects_invalid_password():
    pass
```

---

## 3. Avoid Testing Internal Implementation

Focus on behavior rather than implementation details.

---

## 4. Minimize Mocking

Over-mocking creates fragile tests.

Mock external systems, not your own business logic.

---

## 5. Keep Tests Fast

Fast tests encourage frequent execution.

---

# 16. Real-World Project Structure

```text
project/
│
├── app/
│   ├── api/
│   ├── services/
│   ├── models/
│   └── main.py
│
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── e2e/
│   ├── conftest.py
│   └── test_main.py
│
├── requirements.txt
└── pytest.ini
```

## Example pytest.ini

```ini
[pytest]
testpaths = tests
python_files = test_*.py
addopts = -v
markers =
    integration: integration tests
    e2e: end-to-end tests
```

---

# 17. Advanced pytest Plugins

| Plugin | Purpose |
|---|---|
| pytest-xdist | Parallel execution |
| pytest-cov | Coverage reporting |
| pytest-mock | Better mocking |
| pytest-benchmark | Performance testing |
| pytest-asyncio | Async support |
| pytest-django | Django testing |

## Parallel Execution Example

```bash
pip install pytest-xdist
pytest -n auto
```

---

# 18. Common Anti-Patterns

## Fragile Tests

Tests that break after small UI or implementation changes.

---

## Excessive Sleep Calls

Bad:

```python
import time


time.sleep(5)
```

Use explicit waits instead.

---

## Giant E2E Suites

Too many E2E tests slow development.

Prefer:

- More unit tests
- Moderate integration tests
- Minimal E2E tests

---

## Ignoring Flaky Tests

Flaky tests reduce trust in automation.

Always investigate flaky behavior.

---

# 19. Final Thoughts

Advanced testing in Python involves much more than writing assertions.

A mature testing strategy includes:

- Reliable unit tests
- Meaningful integration tests
- Critical-path E2E tests
- Continuous automation
- Coverage analysis
- Performance validation

## Recommended Stack

| Purpose | Tool |
|---|---|
| Unit Testing | pytest |
| Mocking | unittest.mock / pytest-mock |
| API Testing | requests / TestClient |
| Async Testing | pytest-asyncio |
| E2E Testing | Playwright |
| Coverage | pytest-cov |
| CI/CD | GitHub Actions |

---

# Additional Learning Resources

## Official Documentation

- pytest Documentation
- Playwright Python Documentation
- Selenium Documentation
- FastAPI Testing Guide

## Recommended Topics to Learn Next

- Contract Testing
- Property-Based Testing
- Mutation Testing
- Chaos Engineering
- Security Testing
- Testcontainers
- Distributed System Testing

---

# Summary

This tutorial covered:

- pytest fundamentals
- fixtures and parametrization
- mocking and patching
- integration testing
- end-to-end testing
- API testing
- database testing
- async testing
- performance testing
- CI/CD automation
- testing best practices

A strong testing culture dramatically improves software quality and developer productivity.
