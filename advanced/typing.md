# Python Typing (Type Hints)

## Table of Contents

1. Introduction
2. Why Typing Matters
3. The Evolution of Python Typing
4. Basic Type Hints
5. Built-in Generic Types
6. Optional and Union Types
7. Literal and Final
8. Type Aliases
9. Typed Collections
10. Callable Types
11. Protocols and Structural Typing
12. Generics
13. TypeVar, ParamSpec, and TypeVarTuple
14. Self Type
15. TypedDict
16. Dataclass Typing
17. Enum Typing
18. Async Typing
19. Context Manager Typing
20. Type Narrowing
21. Overloads
22. NewType
23. Annotated Types
24. Runtime Type Checking
25. Mypy Deep Dive
26. Pyright and Other Type Checkers
27. Typing in Large Codebases
28. Common Typing Patterns
29. Anti-Patterns and Pitfalls
30. Real-World Example Project
31. Best Practices
32. Cheat Sheet
33. Further Reading

---

# 1. Introduction

Python is dynamically typed, but modern Python supports powerful static type annotations through the `typing` ecosystem.

Type hints improve:

- Readability
- IDE support
- Refactoring safety
- Static analysis
- API design
- Team collaboration
- Documentation quality

Typing does **not** change runtime behavior by default.

```python
x: int = 10
x = "hello"  # Python allows this at runtime
```

A static type checker such as `mypy` or `pyright` detects the issue.

---

# 2. Why Typing Matters

## Without Type Hints

```python

def process(data):
    return data.upper()
```

Problems:

- What is `data`?
- String? Bytes? Custom object?
- What does the function return?

## With Type Hints

```python

def process(data: str) -> str:
    return data.upper()
```

Benefits:

- Self-documenting
- IDE autocomplete
- Static validation
- Easier onboarding

---

# 3. The Evolution of Python Typing

| Python Version | Feature |
|---|---|
| 3.5 | `typing` module |
| 3.6 | Variable annotations |
| 3.7 | `from __future__ import annotations` |
| 3.8 | `Literal`, `TypedDict`, Protocol improvements |
| 3.9 | Native generics (`list[int]`) |
| 3.10 | `X | Y` union syntax |
| 3.11 | `Self`, `TypeVarTuple`, better typing performance |
| 3.12+ | Improved generic syntax |

---

# 4. Basic Type Hints

## Variables

```python
name: str = "Alice"
age: int = 30
price: float = 10.5
is_active: bool = True
```

## Functions

```python

def add(a: int, b: int) -> int:
    return a + b
```

## Returning None

```python

def log(message: str) -> None:
    print(message)
```

---

# 5. Built-in Generic Types

Python 3.9+ allows native generic syntax.

## Lists

```python
numbers: list[int] = [1, 2, 3]
```

## Dictionaries

```python
scores: dict[str, int] = {
    "Alice": 100,
    "Bob": 90,
}
```

## Tuples

```python
point: tuple[int, int] = (10, 20)
```

## Sets

```python
tags: set[str] = {"python", "typing"}
```

---

# 6. Optional and Union Types

## Union

```python
from typing import Union


def stringify(value: int | float) -> str:
    return str(value)
```

Equivalent older syntax:

```python
Union[int, float]
```

## Optional

```python
from typing import Optional


def find_user(user_id: int) -> str | None:
    if user_id == 1:
        return "Alice"
    return None
```

`Optional[X]` means:

```python
X | None
```

---

# 7. Literal and Final

## Literal

Restricts values to exact constants.

```python
from typing import Literal

HttpMethod = Literal["GET", "POST", "PUT", "DELETE"]


def request(method: HttpMethod) -> None:
    print(method)
```

## Final

```python
from typing import Final

API_VERSION: Final = "v1"
```

Type checkers prevent reassignment.

---

# 8. Type Aliases

## Traditional Syntax

```python
UserId = int
JsonDict = dict[str, str]
```

## Python 3.12+

```python
type UserId = int
```

## Complex Alias

```python
JsonValue = (
    str
    | int
    | float
    | bool
    | None
    | list["JsonValue"]
    | dict[str, "JsonValue"]
)
```

---

# 9. Typed Collections

## Sequence vs List

Prefer abstract types when mutation is unnecessary.

```python
from collections.abc import Sequence


def total(values: Sequence[int]) -> int:
    return sum(values)
```

Works with:

- list
- tuple
- array
- custom sequences

## Mapping

```python
from collections.abc import Mapping


def print_config(config: Mapping[str, str]) -> None:
    for k, v in config.items():
        print(k, v)
```

---

# 10. Callable Types

## Basic Callable

```python
from collections.abc import Callable

Operation = Callable[[int, int], int]


def execute(fn: Operation, a: int, b: int) -> int:
    return fn(a, b)
```

## Flexible Callable

```python
Callable[..., int]
```

Accepts any arguments but returns `int`.

---

# 11. Protocols and Structural Typing

Protocols allow "duck typing" with static analysis.

## Without Inheritance

```python
from typing import Protocol


class Serializable(Protocol):
    def serialize(self) -> str:
        ...


class User:
    def serialize(self) -> str:
        return "user"


class Product:
    def serialize(self) -> str:
        return "product"



def save(obj: Serializable) -> None:
    print(obj.serialize())
```

`User` and `Product` satisfy the protocol automatically.

---

# 12. Generics

Generics create reusable typed components.

## Generic Container

```python
from typing import Generic, TypeVar

T = TypeVar("T")


class Box(Generic[T]):
    def __init__(self, value: T):
        self.value = value

    def get(self) -> T:
        return self.value


int_box = Box[int](123)
str_box = Box[str]("hello")
```

---

# 13. TypeVar, ParamSpec, and TypeVarTuple

## TypeVar

```python
from typing import TypeVar

T = TypeVar("T")


def identity(value: T) -> T:
    return value
```

## Bounded TypeVar

```python
from typing import TypeVar

T = TypeVar("T", bound=float)
```

## ParamSpec

Useful for decorators.

```python
from collections.abc import Callable
from typing import ParamSpec, TypeVar

P = ParamSpec("P")
R = TypeVar("R")



def logger(fn: Callable[P, R]) -> Callable[P, R]:
    def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
        print(f"Calling {fn.__name__}")
        return fn(*args, **kwargs)

    return wrapper
```

## TypeVarTuple

Variadic generics.

```python
from typing import TypeVarTuple

Ts = TypeVarTuple("Ts")
```

Advanced use cases include tensor libraries and shape-safe APIs.

---

# 14. Self Type

`Self` improves fluent APIs.

```python
from typing import Self


class Query:
    def filter(self, condition: str) -> Self:
        print(condition)
        return self

    def limit(self, value: int) -> Self:
        print(value)
        return self
```

---

# 15. TypedDict

`TypedDict` provides typed dictionaries.

```python
from typing import TypedDict


class UserData(TypedDict):
    id: int
    name: str
    email: str


user: UserData = {
    "id": 1,
    "name": "Alice",
    "email": "alice@example.com",
}
```

## Optional Keys

```python
from typing import NotRequired


class Config(TypedDict):
    host: str
    port: int
    timeout: NotRequired[int]
```

---

# 16. Dataclass Typing

Typing works naturally with dataclasses.

```python
from dataclasses import dataclass


@dataclass
class User:
    id: int
    name: str
    email: str
```

## Frozen Dataclass

```python
@dataclass(frozen=True)
class Point:
    x: int
    y: int
```

---

# 17. Enum Typing

```python
from enum import Enum


class Status(Enum):
    PENDING = "pending"
    SUCCESS = "success"
    FAILED = "failed"



def process(status: Status) -> None:
    print(status)
```

---

# 18. Async Typing

## Async Function

```python
async def fetch() -> str:
    return "data"
```

## Awaitable

```python
from collections.abc import Awaitable


def execute(task: Awaitable[str]) -> None:
    pass
```

## AsyncIterator

```python
from collections.abc import AsyncIterator


async def stream() -> AsyncIterator[int]:
    for i in range(5):
        yield i
```

---

# 19. Context Manager Typing

## Sync Context Manager

```python
from collections.abc import Generator
from contextlib import contextmanager


@contextmanager
def open_resource() -> Generator[str, None, None]:
    yield "resource"
```

## Async Context Manager

```python
from contextlib import asynccontextmanager
from collections.abc import AsyncGenerator


@asynccontextmanager
async def connect() -> AsyncGenerator[str, None]:
    yield "connection"
```

---

# 20. Type Narrowing

Type narrowing helps type checkers infer more precise types.

## isinstance

```python

def process(value: int | str) -> None:
    if isinstance(value, int):
        print(value + 1)
    else:
        print(value.upper())
```

## Custom Type Guards

```python
from typing import TypeGuard



def is_string_list(values: list[object]) -> TypeGuard[list[str]]:
    return all(isinstance(v, str) for v in values)
```

---

# 21. Overloads

Overloads improve APIs with multiple signatures.

```python
from typing import overload


@overload
def parse(data: str) -> dict: ...


@overload
def parse(data: bytes) -> list: ...



def parse(data):
    if isinstance(data, str):
        return {}
    return []
```

---

# 22. NewType

Creates lightweight distinct types.

```python
from typing import NewType

UserId = NewType("UserId", int)
ProductId = NewType("ProductId", int)



def get_user(user_id: UserId) -> None:
    pass
```

Prevents mixing incompatible identifiers.

---

# 23. Annotated Types

`Annotated` attaches metadata.

```python
from typing import Annotated

PositiveInt = Annotated[int, "must be positive"]
```

Often used by:

- FastAPI
- Pydantic
- Validation frameworks

## FastAPI Example

```python
from typing import Annotated
from fastapi import Query


async def get_users(
    limit: Annotated[int, Query(gt=0, le=100)]
):
    return []
```

---

# 24. Runtime Type Checking

Type hints are mostly static.

## Manual Validation

```python

def process(value: int) -> None:
    if not isinstance(value, int):
        raise TypeError("value must be int")
```

## Third-Party Libraries

Popular runtime validation tools:

- Pydantic
- beartype
- typeguard

## Pydantic Example

```python
from pydantic import BaseModel


class User(BaseModel):
    id: int
    name: str
```

---

# 25. Mypy Deep Dive

## Installation

```bash
pip install mypy
```

## Running Mypy

```bash
mypy app.py
```

## Strict Mode

```bash
mypy --strict .
```

## Common Configuration

```toml
# pyproject.toml
[tool.mypy]
python_version = "3.12"
strict = true
warn_unused_ignores = true
warn_return_any = true
```

## Ignoring Errors

```python
value = unknown()  # type: ignore
```

Avoid excessive ignores.

## Revealing Types

```python
reveal_type(variable)
```

Excellent for debugging typing issues.

---

# 26. Pyright and Other Type Checkers

## Pyright

Fast and strict.

```bash
npm install -g pyright
```

## Ruff

Ruff also supports typing-related linting.

## Comparison

| Tool | Strength |
|---|---|
| mypy | Ecosystem standard |
| pyright | Speed and accuracy |
| pyre | Meta ecosystem |
| pytype | Google ecosystem |

---

# 27. Typing in Large Codebases

## Gradual Typing Strategy

1. Start with public APIs
2. Add strict checks to core modules
3. Enable CI validation
4. Increase strictness gradually

## Recommended Strictness Order

1. Function return types
2. Function arguments
3. Dataclasses/models
4. Internal helpers
5. Complex generics

## Example CI Step

```yaml
- name: Type Check
  run: mypy .
```

---

# 28. Common Typing Patterns

## Result Pattern

```python
from dataclasses import dataclass


@dataclass
class Success:
    value: str


@dataclass
class Failure:
    error: str


Result = Success | Failure
```

## Repository Pattern

```python
from typing import Protocol


class UserRepository(Protocol):
    def get_by_id(self, user_id: int) -> str:
        ...
```

## Event Handler

```python
from collections.abc import Callable

EventHandler = Callable[[str], None]
```

---

# 29. Anti-Patterns and Pitfalls

## Using Any Everywhere

```python
from typing import Any


def process(data: Any) -> Any:
    return data
```

This removes type safety.

## Overly Complex Types

Avoid unreadable nested unions.

Bad:

```python
list[dict[str, tuple[int | str | None, ...]]]
```

Prefer aliases.

## Runtime Assumptions

Typing does not enforce runtime safety.

```python
x: int = "hello"
```

Still valid Python.

## Mutable Default Arguments

```python

def add_item(items: list[str] = []):
    items.append("x")
```

Typing does not fix logic bugs.

---

# 30. Real-World Example Project

## Typed Service Layer

```python
from dataclasses import dataclass
from typing import Protocol


@dataclass
class User:
    id: int
    name: str


class UserRepository(Protocol):
    def get_by_id(self, user_id: int) -> User | None:
        ...


class InMemoryUserRepository:
    def __init__(self):
        self.users: dict[int, User] = {
            1: User(1, "Alice")
        }

    def get_by_id(self, user_id: int) -> User | None:
        return self.users.get(user_id)


class UserService:
    def __init__(self, repository: UserRepository):
        self.repository = repository

    def get_username(self, user_id: int) -> str:
        user = self.repository.get_by_id(user_id)

        if user is None:
            raise ValueError("User not found")

        return user.name
```

Benefits:

- Strong contracts
- Easier refactoring
- Better IDE support
- Safer dependency injection

---

# 31. Best Practices

## Prefer Concrete Simplicity

Good:

```python
list[str]
```

Bad:

```python
Sequence[str] | list[str]
```

Unless abstraction is necessary.

## Use Strict Type Checking

```bash
mypy --strict
```

## Avoid Any

Use precise types whenever possible.

## Prefer Protocols Over Inheritance

Structural typing is flexible and decoupled.

## Use Type Aliases

Improve readability.

## Type Public APIs First

Most value comes from module boundaries.

## Keep Types Readable

Readable code is more important than ultra-precise types.

---

# 32. Cheat Sheet

## Common Types

```python
str
int
float
bool
None
```

## Collections

```python
list[str]
dict[str, int]
set[int]
tuple[int, str]
```

## Unions

```python
int | str
str | None
```

## Generics

```python
TypeVar
Generic
```

## Advanced

```python
Protocol
TypedDict
Annotated
Literal
Final
Self
ParamSpec
TypeGuard
NewType
```

---

# 33. Further Reading

## Official Documentation

- Python typing docs
- PEP 484
- PEP 544
- PEP 604
- PEP 612
- PEP 673

## Recommended Tools

- mypy
- pyright
- pydantic
- beartype
- ruff

## Recommended Books

- Fluent Python
- Robust Python
- Effective Python

---

# Conclusion

Modern Python typing has evolved into a sophisticated static analysis ecosystem.

Strong typing in Python enables:

- Safer refactoring
- Better tooling
- Cleaner APIs
- More maintainable systems
- Improved developer productivity

The best approach is gradual:

1. Start with public APIs
2. Enable static checking
3. Add stricter typing incrementally
4. Use advanced features only when valuable

Typing is most effective when it improves clarity, not complexity.
