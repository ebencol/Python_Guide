# Asynchronous I/O in Python

## Table of Contents

1. Introduction to Asynchronous I/O
2. Concurrency Models in Python
3. Understanding the Event Loop
4. Coroutines and `async` / `await`
5. Tasks and Scheduling
6. Awaitables and Futures
7. Async Context Managers and Iterators
8. Structured Concurrency with `TaskGroup`
9. Cancellation and Timeouts
10. Async Networking
11. Async HTTP Clients and Servers
12. Async Database Access
13. Async File I/O
14. Queues, Pipelines, and Producer-Consumer Patterns
15. Synchronization Primitives
16. Threading vs Multiprocessing vs AsyncIO
17. Performance Optimization Techniques
18. Debugging Async Applications
19. Testing Async Code
20. Real-World Architecture Patterns
21. Common Pitfalls
22. Best Practices
23. Building a High-Performance Async Service
24. Advanced Topics
25. Conclusion

---

# 1. Introduction to Asynchronous I/O

Asynchronous I/O (Async I/O) is a concurrency model that allows a program to perform multiple tasks seemingly at the same time without blocking execution while waiting for I/O operations.

Typical blocking operations include:

- Network requests
- Database queries
- Reading/writing files
- Waiting for external APIs
- Message queues
- Timers and sleep operations

## Why Async Matters

Modern applications spend most of their time waiting for I/O rather than using CPU.

Examples:

| Application Type | Waiting Source |
|---|---|
| Web server | HTTP requests |
| Chat server | Socket communication |
| API gateway | Upstream services |
| Data pipeline | Database/network |
| Web scraper | HTTP responses |

Async programming improves:

- Scalability
- Throughput
- Responsiveness
- Resource efficiency

---

# 2. Concurrency Models in Python

## Synchronous Programming

```python
import requests

urls = [
    "https://example.com",
    "https://python.org",
    "https://github.com",
]

for url in urls:
    response = requests.get(url)
    print(response.status_code)
```

Problems:

- Each request blocks execution.
- Slow overall throughput.
- Poor resource utilization.

---

## Multithreading

```python
from concurrent.futures import ThreadPoolExecutor
import requests

urls = ["https://example.com"] * 10


def fetch(url):
    return requests.get(url).status_code

with ThreadPoolExecutor(max_workers=5) as executor:
    results = list(executor.map(fetch, urls))

print(results)
```

Advantages:

- Good for I/O-bound work
- Familiar model

Disadvantages:

- Context-switch overhead
- Harder synchronization
- GIL limitations for CPU-bound tasks

---

## Multiprocessing

```python
from multiprocessing import Pool


def square(x):
    return x * x

with Pool(4) as pool:
    results = pool.map(square, range(10))

print(results)
```

Advantages:

- True parallelism
- Bypasses GIL

Disadvantages:

- High memory usage
- Inter-process communication overhead

---

## AsyncIO

```python
import asyncio


async def hello():
    print("Hello")
    await asyncio.sleep(1)
    print("World")


asyncio.run(hello())
```

Advantages:

- Excellent for I/O-heavy workloads
- Lightweight concurrency
- Thousands of tasks possible
- Efficient resource usage

Disadvantages:

- Not ideal for CPU-bound work
- Requires async-compatible libraries
- Learning curve

---

# 3. Understanding the Event Loop

The event loop is the core of AsyncIO.

It:

1. Schedules coroutines
2. Waits for I/O events
3. Resumes paused tasks
4. Executes callbacks

## Conceptual Workflow

```text
Task starts
   ↓
await encountered
   ↓
Task pauses
   ↓
Event loop runs other tasks
   ↓
I/O completes
   ↓
Task resumes
```

---

## Minimal Event Loop Example

```python
import asyncio


async def task(name, delay):
    print(f"Starting {name}")
    await asyncio.sleep(delay)
    print(f"Finished {name}")


async def main():
    await asyncio.gather(
        task("A", 2),
        task("B", 1),
    )


asyncio.run(main())
```

Output:

```text
Starting A
Starting B
Finished B
Finished A
```

Both tasks run concurrently.

---

# 4. Coroutines and `async` / `await`

## Coroutines

A coroutine is a special function that can pause and resume.

```python
async def my_coroutine():
    return 42
```

Calling it returns a coroutine object:

```python
coro = my_coroutine()
print(coro)
```

---

## Awaiting Coroutines

```python
import asyncio


async def fetch_data():
    await asyncio.sleep(1)
    return {"data": 123}


async def main():
    result = await fetch_data()
    print(result)


asyncio.run(main())
```

---

## Chained Coroutines

```python
import asyncio


async def step1():
    await asyncio.sleep(1)
    return 10


async def step2(value):
    await asyncio.sleep(1)
    return value * 2


async def main():
    value = await step1()
    result = await step2(value)
    print(result)


asyncio.run(main())
```

---

# 5. Tasks and Scheduling

## Creating Tasks

Tasks schedule coroutines to run concurrently.

```python
import asyncio


async def worker(name):
    await asyncio.sleep(1)
    print(name)


async def main():
    task1 = asyncio.create_task(worker("A"))
    task2 = asyncio.create_task(worker("B"))

    await task1
    await task2


asyncio.run(main())
```

---

## `asyncio.gather`

Runs multiple coroutines concurrently.

```python
import asyncio


async def square(x):
    await asyncio.sleep(1)
    return x * x


async def main():
    results = await asyncio.gather(
        square(2),
        square(3),
        square(4),
    )

    print(results)


asyncio.run(main())
```

---

## Handling Exceptions in Gather

```python
import asyncio


async def good():
    return 1


async def bad():
    raise ValueError("Boom")


async def main():
    results = await asyncio.gather(
        good(),
        bad(),
        return_exceptions=True,
    )

    print(results)


asyncio.run(main())
```

---

# 6. Awaitables and Futures

## Awaitables

Objects that can be used with `await`:

- Coroutines
- Tasks
- Futures

---

## Futures

A `Future` represents a result that will be available later.

```python
import asyncio


async def set_future(future):
    await asyncio.sleep(1)
    future.set_result("done")


async def main():
    loop = asyncio.get_running_loop()
    future = loop.create_future()

    asyncio.create_task(set_future(future))

    result = await future
    print(result)


asyncio.run(main())
```

---

# 7. Async Context Managers and Iterators

## Async Context Managers

```python
class AsyncResource:
    async def __aenter__(self):
        print("Acquire")
        return self

    async def __aexit__(self, exc_type, exc, tb):
        print("Release")


async def main():
    async with AsyncResource():
        print("Using resource")
```

---

## Async Iterators

```python
import asyncio


class AsyncCounter:
    def __init__(self, limit):
        self.limit = limit
        self.current = 0

    def __aiter__(self):
        return self

    async def __anext__(self):
        if self.current >= self.limit:
            raise StopAsyncIteration

        await asyncio.sleep(0.5)
        self.current += 1
        return self.current


async def main():
    async for number in AsyncCounter(5):
        print(number)


asyncio.run(main())
```

---

# 8. Structured Concurrency with `TaskGroup`

Python 3.11 introduced `TaskGroup`.

Benefits:

- Better lifecycle management
- Automatic cancellation propagation
- Cleaner error handling

## Example

```python
import asyncio


async def worker(name, delay):
    await asyncio.sleep(delay)
    print(name)


async def main():
    async with asyncio.TaskGroup() as tg:
        tg.create_task(worker("A", 1))
        tg.create_task(worker("B", 2))


asyncio.run(main())
```

---

## Error Propagation

```python
import asyncio


async def good_task():
    await asyncio.sleep(1)
    print("Good")


async def bad_task():
    await asyncio.sleep(0.5)
    raise RuntimeError("Failure")


async def main():
    try:
        async with asyncio.TaskGroup() as tg:
            tg.create_task(good_task())
            tg.create_task(bad_task())
    except* RuntimeError as e:
        print("Caught:", e)


asyncio.run(main())
```

---

# 9. Cancellation and Timeouts

Cancellation is fundamental in async systems.

## Cancelling Tasks

```python
import asyncio


async def worker():
    try:
        while True:
            print("Working...")
            await asyncio.sleep(1)
    except asyncio.CancelledError:
        print("Cancelled")
        raise


async def main():
    task = asyncio.create_task(worker())

    await asyncio.sleep(3)
    task.cancel()

    try:
        await task
    except asyncio.CancelledError:
        print("Task fully cancelled")


asyncio.run(main())
```

---

## Timeouts

```python
import asyncio


async def slow_operation():
    await asyncio.sleep(5)


async def main():
    try:
        await asyncio.wait_for(slow_operation(), timeout=2)
    except asyncio.TimeoutError:
        print("Timed out")


asyncio.run(main())
```

---

# 10. Async Networking

## TCP Echo Server

```python
import asyncio


async def handle_client(reader, writer):
    data = await reader.read(100)

    writer.write(data)
    await writer.drain()

    writer.close()
    await writer.wait_closed()


async def main():
    server = await asyncio.start_server(
        handle_client,
        "127.0.0.1",
        8888,
    )

    async with server:
        await server.serve_forever()


asyncio.run(main())
```

---

## TCP Client

```python
import asyncio


async def client():
    reader, writer = await asyncio.open_connection(
        "127.0.0.1",
        8888,
    )

    writer.write(b"hello")
    await writer.drain()

    response = await reader.read(100)
    print(response)

    writer.close()
    await writer.wait_closed()


asyncio.run(client())
```

---

# 11. Async HTTP Clients and Servers

## Using `aiohttp`

Install:

```bash
pip install aiohttp
```

---

## Async HTTP Client

```python
import aiohttp
import asyncio


async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()


async def main():
    async with aiohttp.ClientSession() as session:
        html = await fetch(session, "https://example.com")
        print(len(html))


asyncio.run(main())
```

---

## Concurrent Requests

```python
import aiohttp
import asyncio


URLS = [
    "https://example.com",
    "https://python.org",
    "https://github.com",
]


async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()


async def main():
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(session, url) for url in URLS]
        results = await asyncio.gather(*tasks)

        print([len(r) for r in results])


asyncio.run(main())
```

---

## Building an Async API with FastAPI

Install:

```bash
pip install fastapi uvicorn
```

---

```python
from fastapi import FastAPI
import asyncio

app = FastAPI()


@app.get("/data")
async def get_data():
    await asyncio.sleep(1)
    return {"message": "async response"}
```

Run:

```bash
uvicorn app:app --reload
```

---

# 12. Async Database Access

## Async PostgreSQL with `asyncpg`

Install:

```bash
pip install asyncpg
```

---

## Basic Example

```python
import asyncpg
import asyncio


async def main():
    conn = await asyncpg.connect(
        user="postgres",
        password="password",
        database="testdb",
        host="localhost",
    )

    rows = await conn.fetch("SELECT * FROM users")

    for row in rows:
        print(dict(row))

    await conn.close()


asyncio.run(main())
```

---

## Connection Pools

```python
import asyncpg
import asyncio


async def main():
    pool = await asyncpg.create_pool(
        user="postgres",
        password="password",
        database="testdb",
        host="localhost",
        min_size=5,
        max_size=20,
    )

    async with pool.acquire() as conn:
        rows = await conn.fetch("SELECT NOW()")
        print(rows)

    await pool.close()


asyncio.run(main())
```

---

## SQLAlchemy Async ORM

```python
from sqlalchemy.ext.asyncio import create_async_engine
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "postgresql+asyncpg://user:pass@localhost/db"

engine = create_async_engine(DATABASE_URL)

AsyncSessionLocal = sessionmaker(
    engine,
    class_=AsyncSession,
    expire_on_commit=False,
)
```

---

# 13. Async File I/O

Python file I/O is usually blocking.

Use `aiofiles` for async-compatible file operations.

Install:

```bash
pip install aiofiles
```

---

## Reading Files

```python
import aiofiles
import asyncio


async def main():
    async with aiofiles.open("data.txt", mode="r") as f:
        content = await f.read()
        print(content)


asyncio.run(main())
```

---

## Writing Files

```python
import aiofiles
import asyncio


async def main():
    async with aiofiles.open("output.txt", mode="w") as f:
        await f.write("Hello Async")


asyncio.run(main())
```

---

# 14. Queues, Pipelines, and Producer-Consumer Patterns

## Async Queue

```python
import asyncio


async def producer(queue):
    for i in range(5):
        await asyncio.sleep(1)
        await queue.put(i)
        print(f"Produced {i}")


async def consumer(queue):
    while True:
        item = await queue.get()

        print(f"Consumed {item}")

        queue.task_done()


async def main():
    queue = asyncio.Queue()

    producer_task = asyncio.create_task(producer(queue))
    consumer_task = asyncio.create_task(consumer(queue))

    await producer_task
    await queue.join()

    consumer_task.cancel()


asyncio.run(main())
```

---

## Multi-Stage Pipeline

```python
import asyncio


async def producer(queue):
    for i in range(10):
        await queue.put(i)

    await queue.put(None)


async def processor(input_q, output_q):
    while True:
        item = await input_q.get()

        if item is None:
            await output_q.put(None)
            break

        await output_q.put(item * 2)


async def consumer(queue):
    while True:
        item = await queue.get()

        if item is None:
            break

        print(item)


async def main():
    q1 = asyncio.Queue()
    q2 = asyncio.Queue()

    await asyncio.gather(
        producer(q1),
        processor(q1, q2),
        consumer(q2),
    )


asyncio.run(main())
```

---

# 15. Synchronization Primitives

## Async Lock

```python
import asyncio

counter = 0
lock = asyncio.Lock()


async def increment():
    global counter

    async with lock:
        temp = counter
        await asyncio.sleep(0.1)
        counter = temp + 1


async def main():
    await asyncio.gather(*[increment() for _ in range(100)])
    print(counter)


asyncio.run(main())
```

---

## Semaphore

Useful for rate limiting.

```python
import asyncio

semaphore = asyncio.Semaphore(3)


async def worker(i):
    async with semaphore:
        print(f"Task {i} running")
        await asyncio.sleep(1)


async def main():
    await asyncio.gather(*(worker(i) for i in range(10)))


asyncio.run(main())
```

---

## Event

```python
import asyncio


event = asyncio.Event()


async def waiter():
    print("Waiting")
    await event.wait()
    print("Event received")


async def trigger():
    await asyncio.sleep(2)
    event.set()


async def main():
    await asyncio.gather(waiter(), trigger())


asyncio.run(main())
```

---

# 16. Threading vs Multiprocessing vs AsyncIO

| Feature | Threading | Multiprocessing | AsyncIO |
|---|---|---|---|
| Best for | I/O | CPU | I/O |
| Parallelism | Limited by GIL | True | Cooperative |
| Memory usage | Medium | High | Low |
| Scalability | Moderate | Low | High |
| Context switching | OS-level | OS-level | User-space |
| Complexity | Medium | High | Medium |

---

# 17. Performance Optimization Techniques

## Use Connection Pools

Avoid creating connections repeatedly.

---

## Limit Concurrency

Too many tasks can overload systems.

```python
import asyncio

semaphore = asyncio.Semaphore(100)
```

---

## Batch Requests

Bad:

```python
for item in items:
    await process(item)
```

Better:

```python
await asyncio.gather(*(process(item) for item in items))
```

---

## Avoid Blocking Calls

Never use:

```python
time.sleep(1)
```

Inside async code.

Use:

```python
await asyncio.sleep(1)
```

---

## Offload CPU Work

```python
import asyncio
from concurrent.futures import ProcessPoolExecutor



def cpu_heavy(n):
    return sum(i * i for i in range(n))


async def main():
    loop = asyncio.get_running_loop()

    with ProcessPoolExecutor() as pool:
        result = await loop.run_in_executor(
            pool,
            cpu_heavy,
            10_000_000,
        )

    print(result)


asyncio.run(main())
```

---

# 18. Debugging Async Applications

## Enable Debug Mode

```python
import asyncio

asyncio.run(main(), debug=True)
```

---

## Logging Slow Callbacks

```python
import logging

logging.basicConfig(level=logging.DEBUG)
```

---

## Detect Forgotten Awaits

Bad:

```python
fetch_data()
```

Correct:

```python
await fetch_data()
```

---

## Inspect Running Tasks

```python
for task in asyncio.all_tasks():
    print(task)
```

---

# 19. Testing Async Code

## Using `pytest-asyncio`

Install:

```bash
pip install pytest pytest-asyncio
```

---

## Async Test Example

```python
import asyncio
import pytest


async def async_add(a, b):
    await asyncio.sleep(0.1)
    return a + b


@pytest.mark.asyncio
async def test_async_add():
    result = await async_add(2, 3)
    assert result == 5
```

---

## Mocking Async Functions

```python
from unittest.mock import AsyncMock


mock = AsyncMock()
mock.return_value = "data"
```

---

# 20. Real-World Architecture Patterns

## Fan-Out / Fan-In

```python
results = await asyncio.gather(
    service_a(),
    service_b(),
    service_c(),
)
```

Common in API gateways.

---

## Background Workers

```python
asyncio.create_task(background_worker())
```

Used for:

- Notifications
- Cleanup jobs
- Metrics collection
- Streaming

---

## Rate-Limited APIs

```python
semaphore = asyncio.Semaphore(10)
```

Prevent API abuse.

---

## Streaming Pipelines

Useful for:

- ETL systems
- Kafka consumers
- Log processors
- Video pipelines

---

# 21. Common Pitfalls

## Blocking the Event Loop

Bad:

```python
import time

time.sleep(5)
```

---

## Creating Too Many Tasks

```python
tasks = [asyncio.create_task(work()) for _ in range(1_000_000)]
```

Can exhaust memory.

---

## Forgetting Cancellation Handling

Always clean up resources.

```python
try:
    await work()
finally:
    await cleanup()
```

---

## Mixing Sync and Async Incorrectly

Bad:

```python
requests.get(url)
```

Inside async code.

Use async libraries instead.

---

# 22. Best Practices

## Prefer Structured Concurrency

Use `TaskGroup` when possible.

---

## Keep Coroutines Small

Avoid giant async functions.

---

## Use Timeouts Everywhere

```python
await asyncio.wait_for(task(), timeout=5)
```

---

## Backpressure Matters

Use queues and semaphores.

---

## Use Async-Compatible Libraries

Examples:

| Sync | Async |
|---|---|
| requests | aiohttp/httpx |
| psycopg2 | asyncpg |
| pymongo | motor |
| open() | aiofiles |

---

# 23. Building a High-Performance Async Service

## Example Architecture

```text
Client Requests
       ↓
FastAPI Server
       ↓
Task Queue
       ↓
Async Workers
       ↓
Database / APIs
```

---

## Example Service

```python
from fastapi import FastAPI
import asyncio

app = FastAPI()


async def external_call():
    await asyncio.sleep(1)
    return {"status": "ok"}


@app.get("/aggregate")
async def aggregate():
    results = await asyncio.gather(
        external_call(),
        external_call(),
        external_call(),
    )

    return {"results": results}
```

---

# 24. Advanced Topics

## Async Generators

```python
import asyncio


async def generator():
    for i in range(5):
        await asyncio.sleep(1)
        yield i


async def main():
    async for value in generator():
        print(value)


asyncio.run(main())
```

---

## Context Variables

Useful for request-scoped data.

```python
import contextvars

request_id = contextvars.ContextVar("request_id")
```

---

## Event Loop Policies

```python
import asyncio

policy = asyncio.get_event_loop_policy()
print(policy)
```

---

## UVLoop

`uvloop` is a high-performance event loop implementation.

Install:

```bash
pip install uvloop
```

Usage:

```python
import uvloop
import asyncio

asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())
```

Often significantly faster than the default loop.

---

## AnyIO and Trio

Modern alternatives to raw asyncio.

Benefits:

- Better structured concurrency
- Improved cancellation semantics
- Cleaner APIs

---

# 25. Conclusion

Asynchronous I/O is one of the most important techniques for building scalable modern Python applications.

Key takeaways:

- AsyncIO excels for I/O-bound workloads.
- The event loop orchestrates cooperative multitasking.
- Coroutines, tasks, and structured concurrency form the core model.
- Proper cancellation, timeouts, and synchronization are critical.
- Performance depends on avoiding blocking operations.
- Async architecture enables highly scalable systems.

Asynchronous programming unlocks the ability to build:

- High-performance APIs
- Real-time systems
- Streaming platforms
- Web crawlers
- Distributed systems
- Event-driven architectures

The modern Python ecosystem increasingly relies on asynchronous programming, making it an essential skill for advanced backend and systems development.
