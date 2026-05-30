# SQLAlchemy

## Table of Contents

1.  Introduction
2.  SQLAlchemy Architecture
3.  Installation
4.  Core vs ORM
5.  Engine and Connections
6.  Metadata and Tables
7.  SQLAlchemy Core Queries
8.  ORM Models
9.  Sessions and Unit of Work
10. Relationships
11. Querying Data
12. Migrations with Alembic
13. Async SQLAlchemy
14. Repository Pattern
15. FastAPI Integration
16. Performance Tips
17. Testing
18. Common Pitfalls
19. Project Structure
20. Best Practices

------------------------------------------------------------------------

# 1. Introduction

SQLAlchemy is the most widely used database toolkit and ORM for Python.
It provides:

-   Database abstraction
-   SQL expression language
-   ORM (Object Relational Mapper)
-   Connection pooling
-   Transaction management
-   Async support

SQLAlchemy supports:

-   PostgreSQL
-   MySQL
-   SQLite
-   Oracle
-   Microsoft SQL Server

------------------------------------------------------------------------

# 2. SQLAlchemy Architecture

``` text
Application
    │
    ▼
 ORM Layer
    │
    ▼
 SQL Expression Language
    │
    ▼
 Engine / Dialect
    │
    ▼
 Database
```

Key concepts:

-   Engine
-   Connection
-   Session
-   Metadata
-   Table
-   Mapper
-   Relationship

------------------------------------------------------------------------

# 3. Installation

``` bash
pip install sqlalchemy
```

PostgreSQL:

``` bash
pip install sqlalchemy psycopg[binary]
```

Async support:

``` bash
pip install sqlalchemy asyncpg
```

------------------------------------------------------------------------

# 4. Core vs ORM

## Core

SQL-centric.

``` python
from sqlalchemy import select

stmt = select(users)
```

Advantages:

-   Fast
-   Explicit SQL
-   Great for analytics

## ORM

Object-centric.

``` python
user = User(name="Alice")
session.add(user)
```

Advantages:

-   Easier business logic
-   Rich relationships
-   Domain modeling

------------------------------------------------------------------------

# 5. Engine and Connections

``` python
from sqlalchemy import create_engine

engine = create_engine(
    "postgresql+psycopg://user:password@localhost/appdb",
    echo=True,
)
```

Connection example:

``` python
with engine.connect() as conn:
    result = conn.execute(text("SELECT 1"))
```

------------------------------------------------------------------------

# 6. Metadata and Tables

``` python
from sqlalchemy import MetaData, Table, Column
from sqlalchemy import Integer, String

metadata = MetaData()

users = Table(
    "users",
    metadata,
    Column("id", Integer, primary_key=True),
    Column("name", String(100)),
)
```

Create tables:

``` python
metadata.create_all(engine)
```

------------------------------------------------------------------------

# 7. SQLAlchemy Core Queries

Insert:

``` python
stmt = users.insert().values(name="Alice")
conn.execute(stmt)
```

Select:

``` python
stmt = users.select()
rows = conn.execute(stmt)
```

Update:

``` python
stmt = (
    users.update()
    .where(users.c.id == 1)
    .values(name="Bob")
)
```

Delete:

``` python
stmt = users.delete().where(users.c.id == 1)
```

------------------------------------------------------------------------

# 8. ORM Models

SQLAlchemy 2.x style:

``` python
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped, mapped_column

class Base(DeclarativeBase):
    pass


class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    email: Mapped[str]
```

Create schema:

``` python
Base.metadata.create_all(engine)
```

------------------------------------------------------------------------

# 9. Sessions and Unit of Work

``` python
from sqlalchemy.orm import sessionmaker

SessionLocal = sessionmaker(bind=engine)

session = SessionLocal()
```

Insert:

``` python
user = User(name="Alice", email="a@example.com")

session.add(user)
session.commit()
```

Rollback:

``` python
try:
    session.commit()
except:
    session.rollback()
    raise
```

------------------------------------------------------------------------

# 10. Relationships

## One-to-Many

``` python
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship

class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)

    posts: Mapped[list["Post"]] = relationship(
        back_populates="author"
    )


class Post(Base):
    __tablename__ = "posts"

    id: Mapped[int] = mapped_column(primary_key=True)

    author_id: Mapped[int] = mapped_column(
        ForeignKey("users.id")
    )

    author: Mapped[User] = relationship(
        back_populates="posts"
    )
```

------------------------------------------------------------------------

# 11. Querying Data

Get all:

``` python
from sqlalchemy import select

stmt = select(User)

users = session.scalars(stmt).all()
```

Filter:

``` python
stmt = select(User).where(
    User.email == "a@example.com"
)
```

Ordering:

``` python
stmt = (
    select(User)
    .order_by(User.name)
)
```

Pagination:

``` python
stmt = (
    select(User)
    .offset(20)
    .limit(10)
)
```

------------------------------------------------------------------------

# 12. Migrations with Alembic

Install:

``` bash
pip install alembic
```

Initialize:

``` bash
alembic init migrations
```

Generate migration:

``` bash
alembic revision --autogenerate -m "create users"
```

Apply:

``` bash
alembic upgrade head
```

------------------------------------------------------------------------

# 13. Async SQLAlchemy

Engine:

``` python
from sqlalchemy.ext.asyncio import create_async_engine

engine = create_async_engine(
    "postgresql+asyncpg://user:password@localhost/appdb"
)
```

Session:

``` python
from sqlalchemy.ext.asyncio import AsyncSession

async with AsyncSession(engine) as session:
    ...
```

Query:

``` python
result = await session.execute(
    select(User)
)

users = result.scalars().all()
```

------------------------------------------------------------------------

# 14. Repository Pattern

``` python
class UserRepository:

    def __init__(self, session):
        self.session = session

    def get(self, user_id: int):
        return self.session.get(User, user_id)

    def add(self, user: User):
        self.session.add(user)
```

Benefits:

-   Testability
-   Separation of concerns
-   Cleaner services

------------------------------------------------------------------------

# 15. FastAPI Integration

Database dependency:

``` python
from collections.abc import Generator

def get_session() -> Generator:
    session = SessionLocal()

    try:
        yield session
    finally:
        session.close()
```

Endpoint:

``` python
@app.get("/users")
def list_users(
    session: Session = Depends(get_session)
):
    return session.scalars(
        select(User)
    ).all()
```

------------------------------------------------------------------------

# 16. Performance Tips

## Avoid N+1 Queries

Bad:

``` python
users = session.scalars(
    select(User)
).all()

for user in users:
    print(user.posts)
```

Better:

``` python
from sqlalchemy.orm import selectinload

stmt = (
    select(User)
    .options(selectinload(User.posts))
)
```

## Select Only Needed Columns

``` python
stmt = select(
    User.id,
    User.name,
)
```

## Use Bulk Operations Carefully

``` python
session.bulk_insert_mappings(
    User,
    records,
)
```

------------------------------------------------------------------------

# 17. Testing

SQLite in-memory database:

``` python
engine = create_engine(
    "sqlite:///:memory:"
)
```

Fixture:

``` python
@pytest.fixture
def session():
    ...
```

Each test should:

-   Create schema
-   Run test
-   Roll back state

------------------------------------------------------------------------

# 18. Common Pitfalls

## Forgetting Commit

``` python
session.add(user)
# missing commit
```

## Long-Lived Sessions

Avoid global sessions.

Use request-scoped sessions.

## N+1 Queries

Use:

``` python
joinedload()
selectinload()
```

## Mixing Sync and Async

Don't use synchronous sessions in async code.

------------------------------------------------------------------------

# 19. Suggested Project Structure

``` text
app/
├── api/
├── services/
├── repositories/
├── models/
├── schemas/
├── database/
│   ├── session.py
│   └── base.py
├── migrations/
└── main.py
```

------------------------------------------------------------------------

# 20. Best Practices

1.  Prefer SQLAlchemy 2.x style APIs.
2.  Use PostgreSQL for production.
3.  Use Alembic for migrations.
4.  Keep sessions short-lived.
5.  Use typed ORM models.
6.  Avoid N+1 queries.
7.  Separate repositories and services.
8.  Use async only when needed.
9.  Write integration tests.
10. Monitor generated SQL.

------------------------------------------------------------------------

# Summary

SQLAlchemy combines:

-   Powerful SQL generation
-   Rich ORM capabilities
-   Transaction management
-   Async support
-   Excellent FastAPI integration

For modern Python applications, a common stack is:

``` text
FastAPI
    +
SQLAlchemy 2.x
    +
Alembic
    +
PostgreSQL
```
