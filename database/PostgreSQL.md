# Advanced PostgreSQL with Python

## Table of Contents

1. Introduction
2. Setting Up PostgreSQL and Python
3. PostgreSQL Architecture Fundamentals
4. Database Design Best Practices
5. Schema Design and Normalization
6. Advanced Data Types
7. Indexing Deep Dive
8. Query Optimization and Performance Tuning
9. Advanced SQL Queries
10. Transactions and Concurrency Control
11. Connection Pooling
12. Partitioning and Scaling
13. Full-Text Search
14. JSONB and Semi-Structured Data
15. Materialized Views and Caching
16. Stored Procedures and Functions
17. Async PostgreSQL with Python
18. Monitoring and Observability
19. Backup and Recovery
20. Security Best Practices
21. Real-World Project Structure
22. Common Anti-Patterns
23. Performance Checklist
24. Conclusion

---

# 1. Introduction

PostgreSQL is one of the most advanced open-source relational databases available today. It provides:

- ACID compliance
- Advanced indexing
- Rich SQL support
- JSON capabilities
- Full-text search
- Partitioning
- Concurrency control
- Extensibility

Python integrates exceptionally well with PostgreSQL through:

- psycopg3
- SQLAlchemy
- asyncpg
- Alembic
- Django ORM

This tutorial focuses on advanced PostgreSQL usage with Python, emphasizing:

- Database design
- Indexing strategies
- Query optimization
- Performance tuning
- Advanced SQL
- Production-grade patterns

---

# 2. Setting Up PostgreSQL and Python

## Install PostgreSQL

### Ubuntu/Debian

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

### macOS

```bash
brew install postgresql
brew services start postgresql
```

### Windows

Download from:

```text
https://www.postgresql.org/download/windows/
```

---

## Create Database and User

```sql
CREATE USER app_user WITH PASSWORD 'strong_password';

CREATE DATABASE ecommerce;

GRANT ALL PRIVILEGES ON DATABASE ecommerce TO app_user;
```

---

## Install Python Libraries

```bash
pip install psycopg[binary]
pip install sqlalchemy
pip install asyncpg
pip install alembic
```

---

## Basic Connection with psycopg3

```python
import psycopg

conn = psycopg.connect(
    host="localhost",
    dbname="ecommerce",
    user="app_user",
    password="strong_password",
    port=5432,
)

with conn.cursor() as cur:
    cur.execute("SELECT version();")
    print(cur.fetchone())

conn.close()
```

---

# 3. PostgreSQL Architecture Fundamentals

Understanding PostgreSQL internals is critical for optimization.

## Core Components

### Postmaster Process

Main PostgreSQL process managing:

- Client connections
- Background workers
- Crash recovery

### Shared Buffers

Memory cache for database pages.

### WAL (Write-Ahead Logging)

Ensures durability and crash recovery.

### MVCC (Multi-Version Concurrency Control)

Allows concurrent reads/writes without locking entire tables.

---

## Why MVCC Matters

PostgreSQL stores multiple row versions.

Benefits:

- Readers do not block writers
- Writers do not block readers
- High concurrency

Downside:

- Dead tuples accumulate
- VACUUM becomes necessary

---

# 4. Database Design Best Practices

Good database design is the foundation of performance.

## Design Principles

### Use Proper Primary Keys

Prefer:

```sql
BIGSERIAL PRIMARY KEY
```

or:

```sql
UUID PRIMARY KEY
```

---

### Avoid Over-Normalization

Too many joins can hurt performance.

Balance:

- Normalization
- Read efficiency

---

### Use Constraints

Constraints improve:

- Data integrity
- Query planner statistics
- Reliability

Examples:

```sql
CHECK (price > 0)
UNIQUE(email)
NOT NULL
FOREIGN KEY
```

---

## Example Schema

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    email TEXT UNIQUE NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE products (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    price NUMERIC(10,2) NOT NULL,
    stock INTEGER NOT NULL
);

CREATE TABLE orders (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT REFERENCES users(id),
    total NUMERIC(10,2),
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE order_items (
    order_id BIGINT REFERENCES orders(id),
    product_id BIGINT REFERENCES products(id),
    quantity INTEGER,
    PRIMARY KEY(order_id, product_id)
);
```

---

# 5. Schema Design and Normalization

## First Normal Form (1NF)

Avoid repeating groups.

Bad:

```text
phones = "123,456"
```

Good:

Separate phone table.

---

## Second Normal Form (2NF)

Remove partial dependencies.

---

## Third Normal Form (3NF)

Remove transitive dependencies.

---

## When to Denormalize

Denormalization can improve:

- Read-heavy workloads
- Analytics
- Dashboard performance

Example:

```sql
ALTER TABLE orders
ADD COLUMN user_email TEXT;
```

Useful when:

- Joins become expensive
- Data changes infrequently

---

# 6. Advanced Data Types

PostgreSQL supports rich data types.

## JSONB

```sql
CREATE TABLE events (
    id BIGSERIAL PRIMARY KEY,
    payload JSONB
);
```

Insert:

```sql
INSERT INTO events(payload)
VALUES ('{"type": "login", "device": "mobile"}');
```

Query:

```sql
SELECT *
FROM events
WHERE payload->>'device' = 'mobile';
```

---

## Arrays

```sql
CREATE TABLE articles (
    id BIGSERIAL PRIMARY KEY,
    tags TEXT[]
);
```

Query:

```sql
SELECT *
FROM articles
WHERE 'python' = ANY(tags);
```

---

## UUID

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE api_keys (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4()
);
```

---

# 7. Indexing Deep Dive

Indexes are essential for performance.

# 7.1 B-Tree Index

Default PostgreSQL index.

Best for:

- Equality
- Range queries
- Sorting

```sql
CREATE INDEX idx_users_email
ON users(email);
```

---

## How PostgreSQL Uses Indexes

Without index:

```text
Sequential Scan
```

With index:

```text
Index Scan
```

---

## Analyze Query Plans

```sql
EXPLAIN ANALYZE
SELECT *
FROM users
WHERE email = 'john@example.com';
```

---

# 7.2 Composite Indexes

```sql
CREATE INDEX idx_orders_user_created
ON orders(user_id, created_at);
```

Effective for:

```sql
WHERE user_id = ?
ORDER BY created_at
```

Order matters.

---

# 7.3 Partial Indexes

Index only subset of rows.

```sql
CREATE INDEX idx_active_users
ON users(email)
WHERE deleted_at IS NULL;
```

Benefits:

- Smaller index
- Faster lookups
- Reduced memory usage

---

# 7.4 GIN Indexes

Used for:

- JSONB
- Arrays
- Full-text search

```sql
CREATE INDEX idx_events_payload
ON events USING GIN(payload);
```

---

# 7.5 GiST Indexes

Useful for:

- Geospatial data
- Range types
- Similarity searches

---

# 7.6 BRIN Indexes

Efficient for very large append-only tables.

Example:

```sql
CREATE INDEX idx_logs_created
ON logs USING BRIN(created_at);
```

Excellent for:

- Time-series data
- Huge datasets

---

# 7.7 Indexing Pitfalls

Too many indexes cause:

- Slower inserts
- Slower updates
- Increased disk usage

Avoid indexing:

- Low-cardinality columns
- Small tables
- Frequently updated columns

---

# 8. Query Optimization and Performance Tuning

# 8.1 EXPLAIN ANALYZE

Most important optimization tool.

```sql
EXPLAIN ANALYZE
SELECT *
FROM orders
WHERE total > 100;
```

Key metrics:

- Seq Scan
- Index Scan
- Cost
- Rows
- Actual time

---

# 8.2 Avoid SELECT *

Bad:

```sql
SELECT * FROM users;
```

Good:

```sql
SELECT id, email FROM users;
```

Benefits:

- Less I/O
- Better index usage
- Reduced network traffic

---

# 8.3 Pagination Optimization

## OFFSET Problem

Bad:

```sql
SELECT *
FROM orders
ORDER BY id
LIMIT 20 OFFSET 100000;
```

Efficient alternative:

```sql
SELECT *
FROM orders
WHERE id > 100000
ORDER BY id
LIMIT 20;
```

Called:

```text
Keyset Pagination
```

---

# 8.4 Batch Inserts

Slow:

```python
for item in items:
    cur.execute(...)
```

Fast:

```python
with conn.cursor() as cur:
    cur.executemany(
        "INSERT INTO products(name, price) VALUES (%s, %s)",
        products,
    )
```

---

# 8.5 VACUUM and ANALYZE

VACUUM removes dead tuples.

```sql
VACUUM ANALYZE;
```

Autovacuum handles this automatically.

Tune carefully for high-write systems.

---

# 8.6 Work Memory

Increase for large sorts and joins.

```sql
SET work_mem = '256MB';
```

---

# 8.7 Avoid N+1 Queries

Bad:

```python
for user in users:
    cur.execute(
        "SELECT * FROM orders WHERE user_id = %s",
        (user.id,)
    )
```

Good:

```sql
SELECT users.id, orders.id
FROM users
LEFT JOIN orders
ON users.id = orders.user_id;
```

---

# 9. Advanced SQL Queries

# 9.1 Common Table Expressions (CTEs)

```sql
WITH top_customers AS (
    SELECT user_id, SUM(total) AS spending
    FROM orders
    GROUP BY user_id
)
SELECT *
FROM top_customers
WHERE spending > 1000;
```

---

# 9.2 Window Functions

Powerful analytical queries.

## Ranking

```sql
SELECT
    id,
    total,
    RANK() OVER (ORDER BY total DESC) AS rank
FROM orders;
```

---

## Running Total

```sql
SELECT
    created_at,
    total,
    SUM(total) OVER (ORDER BY created_at) AS cumulative
FROM orders;
```

---

# 9.3 Recursive Queries

```sql
WITH RECURSIVE hierarchy AS (
    SELECT id, manager_id, name
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    SELECT e.id, e.manager_id, e.name
    FROM employees e
    JOIN hierarchy h
    ON e.manager_id = h.id
)
SELECT * FROM hierarchy;
```

---

# 9.4 UPSERT

```sql
INSERT INTO users(email)
VALUES ('john@example.com')
ON CONFLICT(email)
DO UPDATE SET
    email = EXCLUDED.email;
```

---

# 9.5 LATERAL Joins

```sql
SELECT u.email, o.total
FROM users u
CROSS JOIN LATERAL (
    SELECT total
    FROM orders
    WHERE orders.user_id = u.id
    ORDER BY created_at DESC
    LIMIT 1
) o;
```

---

# 10. Transactions and Concurrency Control

## ACID Properties

- Atomicity
- Consistency
- Isolation
- Durability

---

## Python Transaction Example

```python
with psycopg.connect(...) as conn:
    with conn.cursor() as cur:
        cur.execute(
            "UPDATE accounts SET balance = balance - 100 WHERE id = 1"
        )

        cur.execute(
            "UPDATE accounts SET balance = balance + 100 WHERE id = 2"
        )
```

Automatic rollback on failure.

---

## Isolation Levels

```sql
READ COMMITTED
REPEATABLE READ
SERIALIZABLE
```

---

## Row-Level Locking

```sql
SELECT *
FROM products
WHERE id = 1
FOR UPDATE;
```

Prevents race conditions.

---

# 11. Connection Pooling

Opening database connections is expensive.

Use pooling.

## psycopg Pool

```python
from psycopg_pool import ConnectionPool

pool = ConnectionPool(
    conninfo="dbname=ecommerce user=app_user"
)

with pool.connection() as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT 1")
```

---

## PgBouncer

External connection pooler.

Benefits:

- Lower memory usage
- Better scalability
- Faster connections

---

# 12. Partitioning and Scaling

Partitioning improves performance for huge tables.

## Range Partitioning

```sql
CREATE TABLE logs (
    id BIGSERIAL,
    created_at DATE
) PARTITION BY RANGE(created_at);
```

Partition:

```sql
CREATE TABLE logs_2026
PARTITION OF logs
FOR VALUES FROM ('2026-01-01') TO ('2027-01-01');
```

---

## Benefits

- Faster queries
- Easier maintenance
- Better VACUUM performance

---

# 13. Full-Text Search

## Create Search Vector

```sql
ALTER TABLE articles
ADD COLUMN search_vector tsvector;
```

Populate:

```sql
UPDATE articles
SET search_vector =
    to_tsvector('english', title || ' ' || content);
```

Index:

```sql
CREATE INDEX idx_articles_search
ON articles USING GIN(search_vector);
```

Search:

```sql
SELECT *
FROM articles
WHERE search_vector @@ plainto_tsquery('python optimization');
```

---

# 14. JSONB and Semi-Structured Data

JSONB combines relational and NoSQL capabilities.

## Example

```sql
CREATE TABLE user_preferences (
    user_id BIGINT,
    settings JSONB
);
```

Insert:

```sql
INSERT INTO user_preferences
VALUES (
    1,
    '{"theme": "dark", "notifications": true}'
);
```

Query nested field:

```sql
SELECT settings->>'theme'
FROM user_preferences;
```

---

## JSONB Containment

```sql
SELECT *
FROM user_preferences
WHERE settings @> '{"theme": "dark"}';
```

---

# 15. Materialized Views and Caching

Materialized views store precomputed results.

## Create View

```sql
CREATE MATERIALIZED VIEW monthly_sales AS
SELECT
    DATE_TRUNC('month', created_at) AS month,
    SUM(total) AS revenue
FROM orders
GROUP BY month;
```

Refresh:

```sql
REFRESH MATERIALIZED VIEW monthly_sales;
```

---

## Use Cases

- Analytics dashboards
- Expensive aggregations
- Reporting systems

---

# 16. Stored Procedures and Functions

## PostgreSQL Function

```sql
CREATE OR REPLACE FUNCTION get_order_total(order_id BIGINT)
RETURNS NUMERIC AS $$
DECLARE
    result NUMERIC;
BEGIN
    SELECT SUM(quantity * price)
    INTO result
    FROM order_items
    WHERE order_items.order_id = get_order_total.order_id;

    RETURN result;
END;
$$ LANGUAGE plpgsql;
```

---

## Call from Python

```python
with conn.cursor() as cur:
    cur.execute("SELECT get_order_total(%s)", (1,))
    print(cur.fetchone())
```

---

# 17. Async PostgreSQL with Python

High-performance APIs often use async database access.

## asyncpg Example

```python
import asyncio
import asyncpg

async def main():
    conn = await asyncpg.connect(
        user="app_user",
        password="strong_password",
        database="ecommerce",
        host="localhost"
    )

    rows = await conn.fetch(
        "SELECT * FROM users LIMIT 5"
    )

    for row in rows:
        print(row)

    await conn.close()

asyncio.run(main())
```

---

## Benefits

- Better concurrency
- Lower latency
- Efficient I/O usage

---

# 18. Monitoring and Observability

## pg_stat_statements

Tracks expensive queries.

Enable:

```sql
CREATE EXTENSION pg_stat_statements;
```

Analyze:

```sql
SELECT
    query,
    calls,
    total_exec_time
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 10;
```

---

## Important Metrics

Monitor:

- Slow queries
- Connection count
- Cache hit ratio
- Deadlocks
- Replication lag
- WAL generation

---

# 19. Backup and Recovery

## Logical Backup

```bash
pg_dump ecommerce > backup.sql
```

Restore:

```bash
psql ecommerce < backup.sql
```

---

## Physical Backup

Use:

- WAL archiving
- Streaming replication
- pg_basebackup

---

# 20. Security Best Practices

## Use Least Privilege

```sql
GRANT SELECT, INSERT ON orders TO reporting_user;
```

---

## Enforce SSL

```text
ssl = on
```

---

## Parameterized Queries

Never:

```python
f"SELECT * FROM users WHERE email = '{email}'"
```

Always:

```python
cur.execute(
    "SELECT * FROM users WHERE email = %s",
    (email,)
)
```

---

# 21. Real-World Project Structure

```text
project/
├── app/
│   ├── db/
│   │   ├── models.py
│   │   ├── connection.py
│   │   ├── repositories/
│   │   └── migrations/
│   ├── services/
│   ├── api/
│   └── config.py
├── tests/
├── alembic/
└── requirements.txt
```

---

## Repository Pattern Example

```python
class UserRepository:
    def __init__(self, conn):
        self.conn = conn

    def get_by_email(self, email: str):
        with self.conn.cursor() as cur:
            cur.execute(
                "SELECT id, email FROM users WHERE email = %s",
                (email,)
            )
            return cur.fetchone()
```

---

# 22. Common Anti-Patterns

## Anti-Pattern 1: Missing Indexes

Causes full table scans.

---

## Anti-Pattern 2: Too Many Indexes

Slows writes.

---

## Anti-Pattern 3: Storing Everything in JSONB

Relational data should remain relational.

---

## Anti-Pattern 4: Large Transactions

Can:

- Hold locks too long
- Increase WAL
- Cause bloat

---

## Anti-Pattern 5: ORM Abuse

ORM-generated queries may become inefficient.

Always inspect SQL.

---

# 23. Performance Checklist

## Schema

- Proper normalization
- Correct data types
- Constraints defined

---

## Queries

- Avoid SELECT *
- Use pagination properly
- Use JOINs efficiently
- Analyze query plans

---

## Indexes

- Add indexes for frequent filters
- Remove unused indexes
- Use partial indexes
- Use GIN for JSONB

---

## Infrastructure

- Use connection pooling
- Configure autovacuum
- Monitor slow queries
- Tune shared_buffers
- Tune work_mem

---

# 24. Conclusion

Advanced PostgreSQL development with Python requires understanding:

- Database internals
- Query optimization
- Indexing strategies
- Concurrency
- Scaling patterns
- Monitoring

Key takeaways:

1. Design schema carefully.
2. Index only where beneficial.
3. Use EXPLAIN ANALYZE frequently.
4. Monitor production continuously.
5. Optimize queries before scaling hardware.
6. Use PostgreSQL advanced features strategically.

