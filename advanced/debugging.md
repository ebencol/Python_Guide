# Debugging in Python

## Table of Contents

1. Introduction
2. The Debugging Mindset
3. Understanding Python Errors
4. Reading Tracebacks Effectively
5. Logging Fundamentals
6. Structured Logging
7. Using the Built-in Debugger (`pdb`)
8. Modern Debugging with `breakpoint()`
9. Advanced `pdb` Techniques
10. Debugging with IDEs
11. Debugging Asynchronous Code
12. Debugging Multithreaded Applications
13. Debugging Multiprocessing Systems
14. Debugging Memory Leaks
15. Profiling and Performance Debugging
16. Debugging Network Applications
17. Debugging Database Issues
18. Debugging Web Applications
19. Debugging APIs
20. Debugging Tests
21. Exception Handling Strategies
22. Observability and Production Debugging
23. Post-Mortem Debugging
24. Core Dumps and Crash Analysis
25. Using `faulthandler`
26. Debugging Native Extensions
27. Remote Debugging
28. Time Travel and Replay Debugging
29. Debugging Containers and Kubernetes
30. Common Debugging Anti-Patterns
31. Building a Reproducible Debugging Workflow
32. Recommended Tools
33. Practical Real-World Debugging Scenarios
34. Conclusion

---

# 1. Introduction

Debugging is one of the most important engineering skills in software development. Writing code is only part of the job — understanding why systems fail, identifying hidden assumptions, tracing execution flow, and diagnosing production issues are equally critical.

Advanced debugging is not simply about fixing syntax errors. It involves:

- Understanding runtime behavior
- Inspecting state transitions
- Diagnosing concurrency problems
- Tracing distributed systems
- Investigating performance bottlenecks
- Handling production incidents
- Building observability into applications

Python provides a rich ecosystem for debugging:

- Built-in debuggers (`pdb`)
- Logging systems
- Profilers
- Memory analyzers
- Tracing tools
- Async inspection tools
- Production observability tooling

This tutorial covers debugging from beginner-friendly fundamentals to advanced production-grade debugging workflows.

---

# 2. The Debugging Mindset

Strong debugging skills come from systematic thinking.

## Key Principles

### 1. Reproduce First

Never start randomly changing code.

Instead:

- Reproduce consistently
- Isolate the minimal failing case
- Remove unrelated complexity

### 2. Observe Before Changing

Inspect:

- Inputs
- Outputs
- Intermediate state
- Environment variables
- External dependencies
- Timing

### 3. Hypothesis-Driven Debugging

Treat debugging like science.

Example:

```python
# Hypothesis:
# User session expires because timezone conversion is incorrect.
```

Then validate the hypothesis with instrumentation.

### 4. Narrow the Scope

Binary search the codebase:

- Which layer fails?
- Which request?
- Which thread?
- Which deployment?
- Which dependency?

### 5. Preserve Evidence

Avoid destroying valuable debugging data:

- Save logs
- Save stack traces
- Save payloads
- Save crash dumps

---

# 3. Understanding Python Errors

## Syntax Errors

Detected before execution.

```python
if True
    print("missing colon")
```

Output:

```text
SyntaxError: expected ':'
```

## Runtime Errors

Occur during execution.

```python
x = 1 / 0
```

```text
ZeroDivisionError
```

## Logical Errors

Code runs but produces incorrect behavior.

```python
def average(numbers):
    return sum(numbers) / (len(numbers) - 1)
```

No exception is raised, but logic is incorrect.

---

# 4. Reading Tracebacks Effectively

Example:

```python
def divide(a, b):
    return a / b


def process():
    return divide(10, 0)


process()
```

Traceback:

```text
Traceback (most recent call last):
  File "app.py", line 8, in <module>
    process()
  File "app.py", line 5, in process
    return divide(10, 0)
  File "app.py", line 2, in divide
    return a / b
ZeroDivisionError: division by zero
```

## Important Insight

Read tracebacks from bottom to top.

Bottom:

```text
ZeroDivisionError: division by zero
```

Root cause:

```python
return a / b
```

Call chain:

```text
process() -> divide()
```

## Rich Tracebacks

Install:

```bash
pip install rich
```

Example:

```python
from rich.traceback import install

install()
```

Benefits:

- Syntax highlighting
- Local variable inspection
- Better readability

---

# 5. Logging Fundamentals

Logging is the most important debugging tool in production systems.

## Why Logging Matters

`print()` statements are insufficient because:

- No log levels
- No timestamps
- No filtering
- No persistence
- No structured context

## Basic Logging

```python
import logging

logging.basicConfig(level=logging.INFO)

logging.info("Application started")
logging.warning("Potential issue")
logging.error("Something failed")
```

## Log Levels

| Level | Purpose |
|---|---|
| DEBUG | Detailed diagnostic information |
| INFO | Normal operation |
| WARNING | Unexpected but recoverable |
| ERROR | Failure occurred |
| CRITICAL | Severe system failure |

## Logging Exceptions

```python
import logging

logger = logging.getLogger(__name__)

try:
    1 / 0
except ZeroDivisionError:
    logger.exception("Division failed")
```

`logger.exception()` automatically includes stack traces.

---

# 6. Structured Logging

Structured logs are machine-readable.

## JSON Logging

Install:

```bash
pip install python-json-logger
```

Example:

```python
import logging
from pythonjsonlogger import jsonlogger

handler = logging.StreamHandler()
formatter = jsonlogger.JsonFormatter()

handler.setFormatter(formatter)

logger = logging.getLogger()
logger.addHandler(handler)
logger.setLevel(logging.INFO)

logger.info(
    "User login",
    extra={
        "user_id": 123,
        "ip": "192.168.1.1"
    }
)
```

Output:

```json
{
  "message": "User login",
  "user_id": 123,
  "ip": "192.168.1.1"
}
```

## Benefits

Structured logging enables:

- Searchable logs
- Log aggregation
- Correlation IDs
- Observability pipelines
- Metrics extraction

---

# 7. Using the Built-in Debugger (`pdb`)

Python ships with an interactive debugger.

## Basic Usage

```python
import pdb

x = 10
pdb.set_trace()

y = x * 2
```

Execution pauses.

## Common Commands

| Command | Meaning |
|---|---|
| n | Next line |
| s | Step into |
| c | Continue |
| l | List source |
| p | Print variable |
| pp | Pretty print |
| q | Quit |
| bt | Stack trace |
| u | Move up stack |
| d | Move down stack |

Example:

```text
(Pdb) p x
10
```

---

# 8. Modern Debugging with `breakpoint()`

Python 3.7 introduced:

```python
breakpoint()
```

Equivalent to:

```python
import pdb
pdb.set_trace()
```

Example:

```python
def calculate():
    value = 42
    breakpoint()
    return value * 2
```

## Advantages

- Cleaner syntax
- Configurable via environment variables
- IDE integration

Disable breakpoints:

```bash
PYTHONBREAKPOINT=0
```

---

# 9. Advanced `pdb` Techniques

## Conditional Breakpoints

```python
for i in range(100):
    if i == 42:
        breakpoint()
```

## Post-Mortem Debugging

```python
import pdb

try:
    broken()
except Exception:
    pdb.post_mortem()
```

Allows inspection after failure.

## Running Scripts Under Debugger

```bash
python -m pdb app.py
```

## Breakpoints in Specific Functions

```text
(Pdb) break mymodule.py:42
```

## Inspecting Stack Frames

```text
(Pdb) where
```

or:

```text
(Pdb) bt
```

---

# 10. Debugging with IDEs

Modern IDEs provide graphical debuggers.

## Popular IDEs

- entity["software","PyCharm","JetBrains Python IDE"]
- entity["software","Visual Studio Code","Microsoft code editor"]
- entity["software","Cursor","AI code editor"]

## Important Features

### Breakpoints

Pause execution.

### Conditional Breakpoints

Pause only when:

```python
user_id == 42
```

### Watch Expressions

Continuously inspect values.

### Variable Explorer

Inspect:

- Objects
- Lists
- Dictionaries
- Closures
- Frames

### Step Controls

| Action | Meaning |
|---|---|
| Step Into | Enter function |
| Step Over | Skip internals |
| Step Out | Exit current function |

---

# 11. Debugging Asynchronous Code

Async bugs are notoriously difficult.

## Common Problems

- Forgotten `await`
- Race conditions
- Deadlocks
- Task cancellation
- Event loop misuse

## Example Bug

```python
import asyncio


async def fetch_data():
    await asyncio.sleep(1)
    return 42


def main():
    result = fetch_data()
    print(result)


main()
```

Output:

```text
<coroutine object>
```

Fix:

```python
async def main():
    result = await fetch_data()
    print(result)


asyncio.run(main())
```

## Enable Asyncio Debug Mode

```python
import asyncio

asyncio.run(main(), debug=True)
```

Or:

```bash
PYTHONASYNCIODEBUG=1
```

## Detect Slow Coroutines

```python
loop.slow_callback_duration = 0.1
```

---

# 12. Debugging Multithreaded Applications

Threads introduce:

- Race conditions
- Deadlocks
- Data corruption
- Timing-dependent bugs

## Example Race Condition

```python
import threading

counter = 0


def increment():
    global counter

    for _ in range(100000):
        counter += 1


threads = [
    threading.Thread(target=increment)
    for _ in range(4)
]

for t in threads:
    t.start()

for t in threads:
    t.join()

print(counter)
```

Expected:

```text
400000
```

Actual:

Potentially lower due to race conditions.

## Fix with Locks

```python
lock = threading.Lock()


def increment():
    global counter

    for _ in range(100000):
        with lock:
            counter += 1
```

## Thread Dumps

```python
import threading

for thread in threading.enumerate():
    print(thread)
```

---

# 13. Debugging Multiprocessing Systems

Processes are harder to debug because memory is isolated.

## Common Problems

- Pickling errors
- Zombie processes
- Deadlocks
- IPC failures

## Example Pickling Error

```python
from multiprocessing import Pool


def main():
    def worker(x):
        return x * 2

    with Pool() as pool:
        print(pool.map(worker, [1, 2, 3]))


main()
```

Fails because nested functions are not pickleable.

Fix:

```python
def worker(x):
    return x * 2
```

## Logging Process IDs

```python
import os

print(os.getpid())
```

---

# 14. Debugging Memory Leaks

Python uses garbage collection, but memory leaks still occur.

## Common Causes

- Global caches
- Circular references
- Unclosed resources
- Retained large objects
- Event listeners

## Using `tracemalloc`

```python
import tracemalloc

tracemalloc.start()

# Application logic

snapshot = tracemalloc.take_snapshot()

for stat in snapshot.statistics('lineno')[:10]:
    print(stat)
```

## Object Reference Inspection

```python
import gc

objects = gc.get_objects()
print(len(objects))
```

## Memory Profiling

Install:

```bash
pip install memory-profiler
```

Usage:

```python
from memory_profiler import profile


@profile
def load_data():
    data = [x for x in range(10_000_000)]
    return data
```

Run:

```bash
python -m memory_profiler app.py
```

---

# 15. Profiling and Performance Debugging

Performance debugging differs from functional debugging.

## Using `cProfile`

```python
import cProfile


def slow_function():
    total = 0

    for i in range(10_000_000):
        total += i


cProfile.run('slow_function()')
```

## Example Output

```text
ncalls  tottime  percall  cumtime
```

## Line Profiling

Install:

```bash
pip install line-profiler
```

Usage:

```python
@profile
def expensive():
    pass
```

Run:

```bash
kernprof -l app.py
python -m line_profiler app.py.lprof
```

## Flame Graphs

Useful for CPU bottlenecks.

Tools:

- py-spy
- scalene
- viztracer

Example:

```bash
py-spy top --pid 1234
```

---

# 16. Debugging Network Applications

## Common Problems

- Timeouts
- Connection resets
- DNS failures
- SSL issues
- Retry storms

## HTTP Request Logging

```python
import requests

response = requests.get(
    "https://example.com",
    timeout=5
)
```

Always set timeouts.

## Inspect Raw Requests

```python
import http.client

http.client.HTTPConnection.debuglevel = 1
```

## Using Packet Capture Tools

Useful tools:

- Wireshark
- tcpdump
- mitmproxy

---

# 17. Debugging Database Issues

## Common Problems

- Slow queries
- Connection leaks
- Deadlocks
- Transaction issues
- ORM inefficiencies

## SQL Logging with SQLAlchemy

```python
from sqlalchemy import create_engine

engine = create_engine(
    DATABASE_URL,
    echo=True
)
```

## Detect N+1 Queries

Bad:

```python
for user in users:
    print(user.posts)
```

May trigger one query per user.

## Measure Query Duration

```python
import time

start = time.perf_counter()

query()

elapsed = time.perf_counter() - start
print(elapsed)
```

---

# 18. Debugging Web Applications

## Common Sources of Bugs

- Middleware order
- Session handling
- Serialization
- Authentication
- Caching

## Flask Debug Mode

```python
app.run(debug=True)
```

## Django Debugging

```python
DEBUG = True
```

Never enable debug mode in production.

## Request Inspection

```python
from flask import request

print(request.headers)
print(request.json)
```

## Middleware Logging

```python
@app.before_request
def log_request():
    print(request.path)
```

---

# 19. Debugging APIs

## Validate Inputs

Use schema validation.

Example with Pydantic:

```python
from pydantic import BaseModel


class UserRequest(BaseModel):
    name: str
    age: int
```

## Trace Request IDs

```python
import uuid

request_id = str(uuid.uuid4())
```

Attach IDs to:

- Logs
- Responses
- Metrics

## Capture Response Payloads

Be careful with:

- Secrets
- Tokens
- PII

---

# 20. Debugging Tests

Tests are debugging tools.

## Use Descriptive Assertions

Bad:

```python
assert result
```

Better:

```python
assert result == expected
```

## Pytest Verbose Mode

```bash
pytest -vv
```

## Stop on First Failure

```bash
pytest -x
```

## Debug Failed Tests

```bash
pytest --pdb
```

Automatically enters debugger on failure.

## Print Captured Output

```bash
pytest -s
```

## Using Fixtures for Isolation

```python
import pytest


@pytest.fixture
def user():
    return {
        "name": "Alice"
    }
```

---

# 21. Exception Handling Strategies

## Anti-Pattern: Swallowing Exceptions

Bad:

```python
try:
    dangerous()
except:
    pass
```

This destroys debugging information.

## Better

```python
try:
    dangerous()
except Exception as exc:
    logger.exception("Operation failed")
    raise
```

## Custom Exceptions

```python
class PaymentError(Exception):
    pass
```

## Exception Chaining

```python
try:
    connect()
except TimeoutError as exc:
    raise PaymentError("Gateway timeout") from exc
```

Preserves original traceback.

---

# 22. Observability and Production Debugging

Modern debugging depends heavily on observability.

## Three Pillars

### Logs

Discrete events.

### Metrics

Numerical measurements.

Examples:

- CPU
- Memory
- Latency
- Error rate

### Traces

Track requests across systems.

## OpenTelemetry

Industry standard for tracing.

Install:

```bash
pip install opentelemetry-sdk
```

## Correlation IDs

Every request should include:

```text
X-Request-ID
```

Useful for tracing distributed systems.

---

# 23. Post-Mortem Debugging

Post-mortem debugging investigates crashes after they happen.

## Example

```python
import pdb
import traceback


try:
    crash()
except Exception:
    traceback.print_exc()
    pdb.post_mortem()
```

You can inspect:

- Local variables
- Stack frames
- Object state

---

# 24. Core Dumps and Crash Analysis

Useful when:

- Python crashes
- Native extensions fail
- Segmentation faults occur

## Enable Core Dumps

Linux:

```bash
ulimit -c unlimited
```

## Analyze with GDB

```bash
gdb python core
```

Useful commands:

```text
bt
```

Shows native stack traces.

---

# 25. Using `faulthandler`

Python includes a fault handler module.

## Enable Globally

```python
import faulthandler

faulthandler.enable()
```

## Dump Tracebacks on Signal

```python
import signal
import faulthandler

faulthandler.register(signal.SIGUSR1)
```

Then:

```bash
kill -USR1 <pid>
```

Useful for hung production processes.

---

# 26. Debugging Native Extensions

Problems involving:

- C extensions
- NumPy internals
- Segmentation faults

Require native debugging.

## Use Debug Builds

```bash
python-dbg
```

## GDB Python Extensions

```bash
gdb --args python app.py
```

Useful commands:

```text
py-bt
```

Shows Python stack traces.

---

# 27. Remote Debugging

Useful for:

- Containers
- Cloud environments
- Kubernetes
- Production systems

## Remote Debugging with debugpy

Install:

```bash
pip install debugpy
```

Example:

```python
import debugpy


debugpy.listen(("0.0.0.0", 5678))
print("Waiting for debugger")
debugpy.wait_for_client()
```

Then attach from your IDE.

---

# 28. Time Travel and Replay Debugging

Replay debugging records execution.

## Tools

- rr
- UndoDB
- Replay.io

Benefits:

- Step backward in time
- Inspect historical state
- Deterministic replay

Especially useful for:

- Race conditions
- Heisenbugs
- Production crashes

---

# 29. Debugging Containers and Kubernetes

## Inspect Running Containers

```bash
docker exec -it container_name bash
```

## View Logs

```bash
docker logs container_name
```

## Kubernetes Logs

```bash
kubectl logs pod_name
```

## Execute Into Pod

```bash
kubectl exec -it pod_name -- bash
```

## Important Production Practices

- Use structured logs
- Include request IDs
- Export metrics
- Enable tracing

---

# 30. Common Debugging Anti-Patterns

## Random Changes

Changing code without understanding the problem.

## Ignoring Logs

Logs often already contain the answer.

## Broad Exception Handlers

```python
except Exception:
    pass
```

Destroys observability.

## Debugging in Production Without Isolation

Can worsen incidents.

## Overusing Print Statements

Use proper logging.

---

# 31. Building a Reproducible Debugging Workflow

## Recommended Workflow

### Step 1: Reproduce

Create minimal reproducible example.

### Step 2: Observe

Inspect:

- Logs
- Metrics
- Traces
- Inputs

### Step 3: Isolate

Reduce system complexity.

### Step 4: Instrument

Add:

- Logging
- Profiling
- Tracing
- Assertions

### Step 5: Validate

Confirm root cause.

### Step 6: Prevent Regression

Add:

- Tests
- Monitoring
- Alerts

---

# 32. Recommended Tools

## Debuggers

| Tool | Purpose |
|---|---|
| pdb | Built-in debugger |
| ipdb | IPython debugger |
| debugpy | Remote debugging |

## Profilers

| Tool | Purpose |
|---|---|
| cProfile | CPU profiling |
| py-spy | Production sampling |
| scalene | CPU + memory profiling |

## Memory Tools

| Tool | Purpose |
|---|---|
| tracemalloc | Allocation tracking |
| memory-profiler | Memory analysis |
| objgraph | Reference inspection |

## Observability Tools

| Tool | Purpose |
|---|---|
| OpenTelemetry | Distributed tracing |
| Prometheus | Metrics |
| Grafana | Visualization |
| Sentry | Error tracking |

---

# 33. Practical Real-World Debugging Scenarios

## Scenario 1: API Latency Spike

### Symptoms

- Slow requests
- High CPU
- Timeouts

### Investigation

1. Inspect metrics
2. Profile CPU
3. Trace requests
4. Inspect database queries

### Root Cause

N+1 ORM queries.

### Fix

Use eager loading.

---

## Scenario 2: Memory Leak in Worker Service

### Symptoms

- Increasing memory usage
- OOM kills

### Investigation

1. Enable tracemalloc
2. Compare snapshots
3. Inspect retained objects

### Root Cause

Global cache never evicted.

### Fix

Implement bounded cache.

---

## Scenario 3: Async Deadlock

### Symptoms

- Service hangs
- No CPU usage

### Investigation

1. Dump stack traces
2. Inspect event loop
3. Check pending tasks

### Root Cause

Await cycle causing deadlock.

### Fix

Refactor task dependency flow.

---

## Scenario 4: Production Crash

### Symptoms

- Segmentation fault
- Python process exits

### Investigation

1. Enable core dumps
2. Analyze with GDB
3. Inspect native stack

### Root Cause

Buggy C extension.

### Fix

Upgrade dependency.

---

# 34. Conclusion

Advanced debugging is a core engineering skill.

Strong debuggers:

- Think systematically
- Instrument aggressively
- Observe carefully
- Preserve evidence
- Understand systems deeply

Python provides excellent debugging capabilities:

- Interactive debuggers
- Profilers
- Tracing systems
- Memory analysis tools
- Production observability tooling

As systems become larger and more distributed, debugging becomes increasingly about:

- Observability
- Tracing
- Metrics
- Correlation
- Reproducibility
