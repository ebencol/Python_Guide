# Python Collections and Data Structures

## Table of Contents

1. Introduction
2. Python Lists
3. Python Tuples
4. Python Sets
5. Python Dictionaries
6. Python `deque`
7. Python `heapq`
8. Choosing the Right Data Structure
9. Best Practices
10. Summary

---

# 1. Introduction

Python provides several built-in and standard-library data structures that help developers efficiently store, organize, and manipulate data.

Understanding when and how to use each structure is essential for writing efficient and maintainable Python code.

This tutorial covers:

- Lists
- Tuples
- Sets
- Dictionaries
- `collections.deque`
- `heapq`

Each section includes:

- Core concepts
- Common operations
- Time complexity considerations
- Real-world use cases
- Practical examples

---

# 2. Python Lists

## What is a List?

A list is an ordered, mutable collection that allows duplicate values.

Lists are one of the most commonly used data structures in Python.

```python
numbers = [1, 2, 3, 4, 5]
```

## Characteristics

- Ordered
- Mutable
- Allows duplicates
- Supports mixed data types
- Indexed

---

## Creating Lists

```python
empty_list = []

fruits = ["apple", "banana", "orange"]

mixed = [1, "hello", 3.14, True]
```

---

## Accessing Elements

```python
fruits = ["apple", "banana", "orange"]

print(fruits[0])
print(fruits[-1])
```

Output:

```python
apple
orange
```

---

## Modifying Lists

```python
fruits = ["apple", "banana"]

fruits.append("orange")
fruits.insert(1, "mango")
fruits.remove("banana")

print(fruits)
```

Output:

```python
['apple', 'mango', 'orange']
```

---

## List Slicing

```python
numbers = [0, 1, 2, 3, 4, 5]

print(numbers[1:4])
print(numbers[:3])
print(numbers[::2])
```

---

## List Comprehensions

List comprehensions provide a concise way to create lists.

```python
squares = [x * x for x in range(10)]

print(squares)
```

---

## Common List Methods

| Method | Description |
|---|---|
| append() | Add item to end |
| extend() | Add multiple items |
| insert() | Insert at position |
| remove() | Remove first occurrence |
| pop() | Remove by index |
| sort() | Sort list |
| reverse() | Reverse list |
| clear() | Remove all items |

---

## Time Complexity

| Operation | Complexity |
|---|---|
| Access by index | O(1) |
| Append | O(1) average |
| Insert at beginning | O(n) |
| Search | O(n) |
| Delete | O(n) |

---

## Use Cases for Lists

### 1. Storing Ordered Data

```python
tasks = ["email", "meeting", "coding"]
```

### 2. Iterating Through Data

```python
for task in tasks:
    print(task)
```

### 3. Stack Implementation

```python
stack = []

stack.append(1)
stack.append(2)

print(stack.pop())
```

### 4. Dynamic Collections

Lists are useful when the size changes frequently.

---

## Example: Shopping Cart

```python
cart = []

cart.append("Laptop")
cart.append("Mouse")
cart.append("Keyboard")

print("Cart Items:")

for item in cart:
    print(item)
```

---

# 3. Python Tuples

## What is a Tuple?

A tuple is an ordered, immutable collection.

```python
coordinates = (10, 20)
```

## Characteristics

- Ordered
- Immutable
- Allows duplicates
- Faster than lists for fixed data
- Hashable if elements are immutable

---

## Creating Tuples

```python
point = (4, 5)

single = (10,)

empty = ()
```

---

## Accessing Tuple Elements

```python
colors = ("red", "green", "blue")

print(colors[1])
```

---

## Tuple Packing and Unpacking

```python
person = ("Alice", 25)

name, age = person

print(name)
print(age)
```

---

## Immutability

```python
numbers = (1, 2, 3)

# numbers[0] = 10
# This would raise an error
```

---

## Time Complexity

| Operation | Complexity |
|---|---|
| Access | O(1) |
| Search | O(n) |
| Iteration | O(n) |

---

## Use Cases for Tuples

### 1. Fixed Configuration Data

```python
database_config = (
    "localhost",
    5432,
    "admin"
)
```

### 2. Returning Multiple Values

```python
def get_user():
    return ("Alice", 30)

name, age = get_user()
```

### 3. Dictionary Keys

```python
locations = {
    (10, 20): "Home",
    (30, 40): "Office"
}
```

### 4. Protecting Data from Modification

Tuples ensure that values cannot accidentally change.

---

## Example: RGB Color

```python
rgb = (255, 128, 0)

print(f"Red: {rgb[0]}")
print(f"Green: {rgb[1]}")
print(f"Blue: {rgb[2]}")
```

---

# 4. Python Sets

## What is a Set?

A set is an unordered collection of unique elements.

```python
numbers = {1, 2, 3}
```

## Characteristics

- Unordered
- Mutable
- No duplicates
- Fast membership testing

---

## Creating Sets

```python
empty_set = set()

fruits = {"apple", "banana", "orange"}
```

---

## Adding and Removing Elements

```python
fruits.add("mango")
fruits.remove("banana")
```

---

## Set Operations

### Union

```python
A = {1, 2, 3}
B = {3, 4, 5}

print(A | B)
```

### Intersection

```python
print(A & B)
```

### Difference

```python
print(A - B)
```

### Symmetric Difference

```python
print(A ^ B)
```

---

## Membership Testing

```python
users = {"alice", "bob", "charlie"}

print("alice" in users)
```

---

## Time Complexity

| Operation | Complexity |
|---|---|
| Add | O(1) average |
| Remove | O(1) average |
| Membership Test | O(1) average |
| Union | O(len(A) + len(B)) |

---

## Use Cases for Sets

### 1. Removing Duplicates

```python
numbers = [1, 2, 2, 3, 4, 4]

unique = set(numbers)

print(unique)
```

### 2. Fast Membership Checks

```python
blocked_users = {"spam1", "spam2"}

if "spam1" in blocked_users:
    print("Blocked")
```

### 3. Comparing Collections

```python
frontend = {"HTML", "CSS", "JavaScript"}
backend = {"Python", "JavaScript"}

print(frontend & backend)
```

### 4. Tag Systems

Sets are useful for storing unique tags.

---

## Example: Removing Duplicate Emails

```python
emails = [
    "a@example.com",
    "b@example.com",
    "a@example.com"
]

unique_emails = set(emails)

print(unique_emails)
```

---

# 5. Python Dictionaries

## What is a Dictionary?

A dictionary stores key-value pairs.

```python
student = {
    "name": "Alice",
    "age": 22
}
```

## Characteristics

- Mutable
- Key-value structure
- Fast lookups
- Keys must be unique and hashable

---

## Creating Dictionaries

```python
empty = {}

user = {
    "username": "admin",
    "active": True
}
```

---

## Accessing Values

```python
print(user["username"])
print(user.get("email"))
```

---

## Adding and Updating Values

```python
user["email"] = "admin@example.com"
user["active"] = False
```

---

## Removing Values

```python
del user["active"]

email = user.pop("email")
```

---

## Iterating Through Dictionaries

```python
for key, value in user.items():
    print(key, value)
```

---

## Dictionary Comprehension

```python
squares = {x: x * x for x in range(5)}

print(squares)
```

---

## Common Dictionary Methods

| Method | Description |
|---|---|
| get() | Safe retrieval |
| keys() | All keys |
| values() | All values |
| items() | Key-value pairs |
| update() | Merge dictionaries |
| pop() | Remove key |
| clear() | Remove all items |

---

## Time Complexity

| Operation | Complexity |
|---|---|
| Lookup | O(1) average |
| Insert | O(1) average |
| Delete | O(1) average |
| Iteration | O(n) |

---

## Use Cases for Dictionaries

### 1. Configuration Storage

```python
config = {
    "host": "localhost",
    "port": 8080
}
```

### 2. Counting Items

```python
text = "hello world"

counts = {}

for char in text:
    counts[char] = counts.get(char, 0) + 1

print(counts)
```

### 3. Caching Data

```python
cache = {}
```

### 4. JSON-like Structures

```python
user = {
    "name": "Alice",
    "skills": ["Python", "SQL"]
}
```

---

## Example: Student Grades

```python
grades = {
    "Alice": 90,
    "Bob": 85,
    "Charlie": 95
}

for student, score in grades.items():
    print(f"{student}: {score}")
```

---

# 6. Python deque

## What is deque?

`deque` stands for double-ended queue.

It is part of the `collections` module.

```python
from collections import deque
```

A `deque` allows efficient insertion and removal from both ends.

---

## Why Use deque Instead of List?

Lists are efficient for appending at the end but slow for inserting/removing at the beginning.

`deque` solves this problem.

---

## Creating a deque

```python
from collections import deque

queue = deque([1, 2, 3])
```

---

## Adding Elements

```python
queue.append(4)
queue.appendleft(0)

print(queue)
```

---

## Removing Elements

```python
queue.pop()
queue.popleft()
```

---

## Rotating a deque

```python
queue = deque([1, 2, 3, 4])

queue.rotate(1)

print(queue)
```

---

## Time Complexity

| Operation | Complexity |
|---|---|
| append() | O(1) |
| appendleft() | O(1) |
| pop() | O(1) |
| popleft() | O(1) |

---

## Use Cases for deque

### 1. Queue Implementation

```python
from collections import deque

queue = deque()

queue.append("task1")
queue.append("task2")

print(queue.popleft())
```

### 2. Breadth-First Search (BFS)

```python
from collections import deque

graph = {
    'A': ['B', 'C'],
    'B': ['D'],
    'C': [],
    'D': []
}

queue = deque(['A'])
visited = set()

while queue:
    node = queue.popleft()

    if node not in visited:
        print(node)
        visited.add(node)
        queue.extend(graph[node])
```

### 3. Sliding Window Problems

`deque` is commonly used in algorithms involving moving windows.

### 4. Undo/Redo Systems

Efficient front and back operations make it suitable for history management.

---

## Example: Browser History

```python
from collections import deque

history = deque(maxlen=5)

history.append("google.com")
history.append("github.com")
history.append("python.org")

print(history)
```

---

# 7. Python heapq

## What is heapq?

`heapq` is a module that implements a binary heap.

A heap is a specialized tree-based structure.

Python's `heapq` implements a min-heap.

```python
import heapq
```

---

## Creating a Heap

```python
import heapq

numbers = [4, 1, 7, 3]

heapq.heapify(numbers)

print(numbers)
```

---

## Pushing Elements

```python
heapq.heappush(numbers, 2)
```

---

## Popping Smallest Element

```python
smallest = heapq.heappop(numbers)

print(smallest)
```

---

## Finding Largest and Smallest Items

```python
nums = [5, 2, 9, 1, 7]

print(heapq.nsmallest(2, nums))
print(heapq.nlargest(2, nums))
```

---

## Priority Queue Example

```python
import heapq

tasks = []

heapq.heappush(tasks, (2, "Write report"))
heapq.heappush(tasks, (1, "Fix bug"))
heapq.heappush(tasks, (3, "Attend meeting"))

while tasks:
    priority, task = heapq.heappop(tasks)
    print(priority, task)
```

---

## Time Complexity

| Operation | Complexity |
|---|---|
| heappush() | O(log n) |
| heappop() | O(log n) |
| heapify() | O(n) |
| Peek smallest | O(1) |

---

## Use Cases for heapq

### 1. Priority Queues

Used in:

- Task schedulers
- Operating systems
- Job queues

### 2. Dijkstra's Algorithm

Shortest-path algorithms commonly use heaps.

### 3. Real-Time Ranking Systems

```python
top_scores = []
```

### 4. Streaming Data

Finding top-k largest or smallest values efficiently.

---

## Example: Task Scheduler

```python
import heapq

jobs = []

heapq.heappush(jobs, (1, "Critical Fix"))
heapq.heappush(jobs, (5, "Code Cleanup"))
heapq.heappush(jobs, (2, "Deploy Update"))

while jobs:
    priority, job = heapq.heappop(jobs)
    print(f"Processing: {job}")
```

---

# 8. Choosing the Right Data Structure

| Data Structure | Ordered | Mutable | Unique Values | Best For |
|---|---|---|---|---|
| List | Yes | Yes | No | General collections |
| Tuple | Yes | No | No | Fixed data |
| Set | No | Yes | Yes | Fast membership testing |
| Dictionary | Yes* | Yes | Keys only | Key-value mapping |
| deque | Yes | Yes | No | Fast queue operations |
| heapq | Partial | Yes | No | Priority queues |

*Python dictionaries preserve insertion order starting from Python 3.7.

---

# 9. Best Practices

## Use Lists When

- Order matters
- Frequent iteration is needed
- You need indexing

## Use Tuples When

- Data should not change
- Returning multiple values
- Using data as dictionary keys

## Use Sets When

- Uniqueness matters
- Fast lookup is required
- Performing set operations

## Use Dictionaries When

- Fast key-based access is needed
- Representing structured data
- Counting or grouping values

## Use deque When

- Building queues
- Frequent front insertions/removals
- Implementing BFS

## Use heapq When

- Managing priorities
- Tracking top-k values
- Scheduling tasks

---

# 10. Summary

Python provides powerful built-in and standard-library data structures.

Choosing the correct structure improves:

- Performance
- Readability
- Maintainability
- Scalability

## Quick Summary

| Structure | Main Strength |
|---|---|
| List | Flexible ordered collection |
| Tuple | Immutable fixed data |
| Set | Fast uniqueness checks |
| Dictionary | Fast key-value access |
| deque | Efficient queue operations |
| heapq | Efficient priority handling |

Mastering these structures is essential for:

- Backend development
- Data engineering
- Algorithms
- Automation
- Machine learning
- System design

---

# Final Notes

Practice is the best way to learn data structures.

Try:

- Building mini-projects
- Solving algorithm problems
- Benchmarking performance
- Refactoring code using different structures

The more you use these structures, the more intuitive they become.
