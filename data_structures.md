# Python Data Structures

Python provides several built-in data structures that make it easy to organize, process, and manipulate data efficiently.

The most important Python data structures are:

1. Lists
2. Tuples
3. Sets
4. Dictionaries
5. Strings
6. Collections module structures
7. Advanced structures (stack, queue, heap, deque)

---

# 1. Lists

A **list** is an ordered, mutable collection.

## Creating Lists

```python
numbers = [1, 2, 3, 4]
mixed = [1, "hello", 3.14, True]
empty = []
```

## Accessing Elements

```python
numbers = [10, 20, 30]

print(numbers[0])   # 10
print(numbers[-1])  # 30
```

## Slicing

```python
nums = [0, 1, 2, 3, 4, 5]

print(nums[1:4])   # [1, 2, 3]
print(nums[:3])    # [0, 1, 2]
print(nums[::2])   # [0, 2, 4]
```

## Modifying Lists

```python
nums = [1, 2, 3]

nums.append(4)
nums.insert(1, 10)
nums.remove(2)

print(nums)
```

## Useful List Methods

| Method | Description |
|---|---|
| append(x) | Add item |
| extend(iterable) | Add multiple items |
| insert(i, x) | Insert at position |
| remove(x) | Remove first occurrence |
| pop() | Remove and return item |
| clear() | Remove all items |
| sort() | Sort list |
| reverse() | Reverse list |

## List Comprehensions

Very Pythonic way to create lists.

```python
squares = [x * x for x in range(10)]

print(squares)
```

Conditional comprehension:

```python
evens = [x for x in range(20) if x % 2 == 0]
```

## Nested Lists

```python
matrix = [
    [1, 2],
    [3, 4]
]

print(matrix[1][0])  # 3
```

---

# 2. Tuples

A **tuple** is ordered but immutable.

## Creating Tuples

```python
point = (10, 20)
single = (5,)
empty = ()
```

## Accessing Elements

```python
print(point[0])
```

## Tuple Unpacking

```python
x, y = point

print(x)
print(y)
```

## Why Use Tuples?

- Immutable
- Faster than lists
- Can be dictionary keys
- Good for fixed data

---

# 3. Sets

A **set** is unordered and stores unique elements.

## Creating Sets

```python
numbers = {1, 2, 3}
empty = set()
```

## Set Operations

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)  # Union
print(a & b)  # Intersection
print(a - b)  # Difference
```

---

# 4. Dictionaries

A **dictionary** stores key-value pairs.

## Creating Dictionaries

```python
person = {
    "name": "Alice",
    "age": 30
}
```

## Accessing Values

```python
print(person["name"])
print(person.get("age"))
```

## Looping Through Dictionaries

```python
for key, value in person.items():
    print(key, value)
```

---

# 5. Strings as Data Structures

Strings are immutable sequences.

## Basic Operations

```python
text = "Python"

print(text[0])
print(text[-1])
```

## f-Strings

```python
name = "Alice"
age = 30

print(f"{name} is {age} years old")
```

---

# 6. Stack Data Structure

A stack follows **LIFO** (Last In, First Out).

```python
stack = []

stack.append(1)
stack.append(2)

print(stack.pop())
```

---

# 7. Queue Data Structure

A queue follows **FIFO** (First In, First Out).

```python
from collections import deque

queue = deque()

queue.append(1)
queue.append(2)
queue.append(3)
queue.append(4)

queue.appendleft(0)

print(queue.pop())     # LIFO
print(queue.popleft()) # FIFO

Output:
4
0
```

---

# 8. Deque

```python
from collections import deque

d = deque([1, 2, 3])

d.appendleft(0)
d.append(4)

print(d)
```

---

# 9. Counter

```python
from collections import Counter

text = "banana"

counter = Counter(text)

print(counter)
```

---

# 10. defaultdict

```python
from collections import defaultdict

d = defaultdict(int)

d["a"] += 1

print(d)
```

---

# 11. NamedTuple

```python
from collections import namedtuple

Point = namedtuple("Point", ["x", "y"])

p = Point(10, 20)

print(p.x)
```

---

# 12. Heap Queue (Priority Queue)

```python
import heapq

nums = [5, 1, 8, 3]

heapq.heapify(nums)

print(heapq.heappop(nums))
```

---

# 13. Arrays

```python
import array

nums = array.array('i', [1, 2, 3])
```

---

# 14. Linked Lists

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

---

# 15. Trees

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```

---

# 16. Graphs

```python
graph = {
    "A": ["B", "C"],
    "B": ["D"],
    "C": [],
    "D": []
}
```

---

# 17. Time Complexity Overview

| Structure | Access | Insert | Delete |
|---|---|---|---|
| List | O(1) | O(n) | O(n) |
| Tuple | O(1) | Immutable | Immutable |
| Set | O(1) avg | O(1) avg | O(1) avg |
| Dict | O(1) avg | O(1) avg | O(1) avg |
| Deque | O(1) ends | O(1) ends | O(1) ends |

---

# 18. Best Practices

## Prefer Comprehensions

```python
squares = [x*x for x in range(10)]
```

## Use `.get()` for Safe Access

```python
value = my_dict.get("key", 0)
```

## Use `enumerate()`

```python
for index, value in enumerate(items):
    print(index, value)
```

---

# 19. Summary

Core structures:
- List → ordered mutable sequence
- Tuple → immutable sequence
- Set → unique unordered collection
- Dictionary → key-value mapping

Advanced structures from `collections` and `heapq` help solve more complex problems efficiently.
