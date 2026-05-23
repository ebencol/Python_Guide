# Linked Lists in Python

## Table of Contents

1. Introduction
2. What Is a Linked List?
3. Memory Model and Internal Mechanics
4. Types of Linked Lists
5. Singly Linked Lists
6. Doubly Linked Lists
7. Circular Linked Lists
8. Sentinel Nodes
9. Complexity Analysis
10. Linked Lists vs Python Built-in Structures
11. Implementing a Generic Linked List
12. Iterators and Pythonic Design
13. Recursive Linked List Algorithms
14. Common Interview Problems
15. Advanced Algorithms on Linked Lists
16. Real-World Use Cases
17. Performance Considerations
18. Common Pitfalls
19. Best Practices
20. Exercises
21. Conclusion

---

# 1. Introduction

Linked lists are one of the most fundamental data structures in computer science. Although Python developers often rely on built-in structures like `list`, `deque`, and `dict`, understanding linked lists is essential for:

- Understanding memory-efficient data organization
- Learning pointer/reference manipulation
- Solving technical interview problems
- Designing advanced data structures
- Building efficient queue and cache systems
- Understanding low-level system behavior

This tutorial explores linked lists from beginner concepts to advanced algorithmic techniques.

---

# 2. What Is a Linked List?

A linked list is a linear data structure where elements are connected using references.

Unlike Python lists, linked list elements are not stored contiguously in memory.

Each element is called a **node**.

A node usually contains:

- Data
- A reference to another node

Example:

```text
[10 | next] -> [20 | next] -> [30 | None]
```

---

# 3. Memory Model and Internal Mechanics

## Python References

Python does not expose raw pointers like C or C++.

Instead, objects reference other objects.

Example:

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None
```

Each `Node` object lives independently in memory.

The `next` variable stores a reference to another node.

---

## Visualization

```text
node1 ──► node2 ──► node3 ──► None
```

This enables:

- Dynamic memory allocation
- Efficient insertions/deletions
- Flexible resizing

But also causes:

- Poor cache locality
- More memory overhead
- Slower random access

---

# 4. Types of Linked Lists

## Singly Linked List

Each node points to the next node.

```text
A -> B -> C
```

---

## Doubly Linked List

Each node stores:

- Previous node
- Next node

```text
None <- A <-> B <-> C -> None
```

---

## Circular Linked List

Last node points back to the head.

```text
A -> B -> C
^         |
|_________|
```

---

# 5. Singly Linked Lists

## Basic Node Implementation

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None
```

---

## Building a Linked List Class

```python
class LinkedList:
    def __init__(self):
        self.head = None
```

---

## Append Operation

```python
class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, value):
        new_node = Node(value)

        if self.head is None:
            self.head = new_node
            return

        current = self.head

        while current.next:
            current = current.next

        current.next = new_node
```

---

## Traversal

```python
def display(self):
    current = self.head

    while current:
        print(current.value, end=" -> ")
        current = current.next

    print("None")
```

---

## Full Example

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None


class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, value):
        new_node = Node(value)

        if self.head is None:
            self.head = new_node
            return

        current = self.head

        while current.next:
            current = current.next

        current.next = new_node

    def display(self):
        current = self.head

        while current:
            print(current.value, end=" -> ")
            current = current.next

        print("None")


ll = LinkedList()
ll.append(10)
ll.append(20)
ll.append(30)

ll.display()
```

Output:

```text
10 -> 20 -> 30 -> None
```

---

# 6. Doubly Linked Lists

## Node Structure

```python
class DoublyNode:
    def __init__(self, value):
        self.value = value
        self.prev = None
        self.next = None
```

---

## Benefits

- Bidirectional traversal
- Easier deletion
- Efficient backtracking

---

## Drawbacks

- More memory usage
- More pointer updates
- More implementation complexity

---

## Doubly Linked List Implementation

```python
class DoublyLinkedList:
    def __init__(self):
        self.head = None

    def append(self, value):
        new_node = DoublyNode(value)

        if self.head is None:
            self.head = new_node
            return

        current = self.head

        while current.next:
            current = current.next

        current.next = new_node
        new_node.prev = current
```

---

# 7. Circular Linked Lists

## Characteristics

- No `None` at the end
- Last node references the first node
- Efficient cyclic traversal

---

## Use Cases

- Operating system schedulers
- Multiplayer game turns
- Music playlists
- Round-robin algorithms

---

## Circular Linked List Example

```python
class CircularLinkedList:
    def __init__(self):
        self.head = None

    def append(self, value):
        new_node = Node(value)

        if not self.head:
            self.head = new_node
            new_node.next = self.head
            return

        current = self.head

        while current.next != self.head:
            current = current.next

        current.next = new_node
        new_node.next = self.head
```

---

# 8. Sentinel Nodes

A sentinel node is a dummy node used to simplify edge cases.

## Advantages

- Cleaner insertion logic
- Cleaner deletion logic
- Fewer special cases

---

## Example

```python
class LinkedList:
    def __init__(self):
        self.head = Node(None)
```

This dummy node never stores actual user data.

---

# 9. Complexity Analysis

| Operation | Singly | Doubly |
|---|---|---|
| Access | O(n) | O(n) |
| Insert at Head | O(1) | O(1) |
| Insert at Tail | O(n)* | O(n)* |
| Delete Head | O(1) | O(1) |
| Delete Tail | O(n) | O(1)** |
| Search | O(n) | O(n) |

\* unless tail pointer exists

\** with tail pointer

---

# 10. Linked Lists vs Python Built-in Structures

| Structure | Random Access | Insert Front | Insert Middle | Memory Efficiency |
|---|---|---|---|---|
| list | O(1) | O(n) | O(n) | Good |
| deque | O(n) | O(1) | O(n) | Better |
| linked list | O(n) | O(1) | O(1)* | Poor |

\* if node reference already known

---

# 11. Implementing a Generic Linked List

## Type Hints with Generics

```python
from typing import Generic, TypeVar, Optional

T = TypeVar("T")


class Node(Generic[T]):
    def __init__(self, value: T):
        self.value: T = value
        self.next: Optional["Node[T]"] = None
```

---

## Benefits

- Better IDE support
- Improved readability
- Stronger static analysis

---

# 12. Iterators and Pythonic Design

## Making the List Iterable

```python
class LinkedList:
    def __init__(self):
        self.head = None

    def __iter__(self):
        current = self.head

        while current:
            yield current.value
            current = current.next
```

---

## Example

```python
for value in ll:
    print(value)
```

---

## Implementing __len__

```python
def __len__(self):
    count = 0
    current = self.head

    while current:
        count += 1
        current = current.next

    return count
```

---

## Implementing __repr__

```python
def __repr__(self):
    return " -> ".join(str(x) for x in self)
```

---

# 13. Recursive Linked List Algorithms

## Recursive Traversal

```python
def recursive_print(node):
    if node is None:
        return

    print(node.value)
    recursive_print(node.next)
```

---

## Recursive Reverse

```python
def reverse_recursive(node):
    if node is None or node.next is None:
        return node

    new_head = reverse_recursive(node.next)

    node.next.next = node
    node.next = None

    return new_head
```

---

# 14. Common Interview Problems

# Reverse a Linked List

## Iterative Solution

```python
def reverse(head):
    prev = None
    current = head

    while current:
        nxt = current.next
        current.next = prev
        prev = current
        current = nxt

    return prev
```

---

## Visualization

Before:

```text
1 -> 2 -> 3 -> None
```

After:

```text
3 -> 2 -> 1 -> None
```

---

# Detect a Cycle

## Floyd's Tortoise and Hare Algorithm

```python
def has_cycle(head):
    slow = head
    fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

        if slow == fast:
            return True

    return False
```

---

## Complexity

| Metric | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |

---

# Find Middle Node

```python
def middle_node(head):
    slow = head
    fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    return slow
```

---

# Merge Two Sorted Lists

```python
def merge(l1, l2):
    dummy = Node(0)
    tail = dummy

    while l1 and l2:
        if l1.value < l2.value:
            tail.next = l1
            l1 = l1.next
        else:
            tail.next = l2
            l2 = l2.next

        tail = tail.next

    tail.next = l1 or l2

    return dummy.next
```

---

# Remove N-th Node from End

```python
def remove_nth_from_end(head, n):
    dummy = Node(0)
    dummy.next = head

    slow = dummy
    fast = dummy

    for _ in range(n + 1):
        fast = fast.next

    while fast:
        slow = slow.next
        fast = fast.next

    slow.next = slow.next.next

    return dummy.next
```

---

# 15. Advanced Algorithms on Linked Lists

# Merge Sort on Linked Lists

Linked lists are ideal for merge sort because insertion operations are efficient.

---

## Split List

```python
def split(head):
    slow = head
    fast = head.next

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    middle = slow.next
    slow.next = None

    return head, middle
```

---

## Merge Sort

```python
def merge_sort(head):
    if head is None or head.next is None:
        return head

    left, right = split(head)

    left = merge_sort(left)
    right = merge_sort(right)

    return merge(left, right)
```

---

## Complexity

| Metric | Complexity |
|---|---|
| Time | O(n log n) |
| Space | O(log n) |

---

# Palindrome Linked List

```python
def is_palindrome(head):
    slow = fast = head
    stack = []

    while fast and fast.next:
        stack.append(slow.value)
        slow = slow.next
        fast = fast.next.next

    if fast:
        slow = slow.next

    while slow:
        if stack.pop() != slow.value:
            return False

        slow = slow.next

    return True
```

---

# Intersection of Two Lists

```python
def get_intersection(headA, headB):
    if not headA or not headB:
        return None

    a = headA
    b = headB

    while a != b:
        a = a.next if a else headB
        b = b.next if b else headA

    return a
```

---

# 16. Real-World Use Cases

## Browser Navigation

Doubly linked lists allow:

- Back navigation
- Forward navigation

---

## LRU Cache

A common implementation combines:

- Hash map
- Doubly linked list

This enables:

- O(1) lookup
- O(1) insertion
- O(1) deletion

---

## Undo/Redo Systems

Applications:

- Text editors
- Image editors
- IDEs

---

## Music and Video Playlists

Circular linked lists allow seamless looping.

---

## Operating System Scheduling

Round-robin schedulers often use circular linked lists.

---

# 17. Performance Considerations

## Cache Locality

Python lists outperform linked lists in many real-world workloads because:

- Lists are contiguous in memory
- CPU cache performance is better

Linked lists suffer from pointer chasing.

---

## Memory Overhead

Every node stores:

- Object metadata
- References
- Additional bookkeeping

This increases memory usage significantly.

---

## Python-Specific Reality

In Python, linked lists are mainly educational unless:

- You need constant-time insertions/deletions
- You already have node references
- You need custom graph-like structures

Otherwise:

- `list`
- `collections.deque`

are usually faster.

---

# 18. Common Pitfalls

## Losing References

Incorrect:

```python
current = current.next
current.next = new_node
```

You may lose access to nodes.

---

## Infinite Loops

Circular structures can accidentally create infinite traversal.

Always verify termination conditions.

---

## Incorrect Pointer Order

When reversing:

```python
current.next = prev
```

must happen after saving:

```python
nxt = current.next
```

---

## Memory Leaks in Other Languages

Python has garbage collection.

But in C/C++:

- Dangling pointers
- Memory leaks
- Double frees

are major risks.

---

# 19. Best Practices

## Use Sentinel Nodes

They simplify algorithms significantly.

---

## Keep Tail References

Tail pointers enable:

- O(1) append
- Efficient queue operations

---

## Make Classes Iterable

Implement:

- `__iter__`
- `__len__`
- `__repr__`

for Pythonic behavior.

---

## Separate Node and List Logic

Avoid mixing traversal logic into node classes.

---

## Use Type Hints

This improves:

- Readability
- Maintainability
- Tooling support

---

# 20. Exercises

## Beginner

1. Implement prepend()
2. Implement delete()
3. Count nodes
4. Search for a value
5. Reverse a list

---

## Intermediate

1. Detect a cycle
2. Find the middle node
3. Merge two sorted lists
4. Remove duplicates
5. Rotate a linked list

---

## Advanced

1. Implement LRU Cache
2. Flatten a multilevel linked list
3. Clone a linked list with random pointers
4. Implement XOR linked list
5. Build a skip list

---

# 21. Conclusion

Linked lists are foundational data structures that teach essential concepts:

- Dynamic memory structures
- Reference manipulation
- Algorithmic thinking
- Pointer-based problem solving

Although Python’s built-in structures are often more practical, mastering linked lists provides a strong foundation for:

- Data structure design
- Systems programming
- Technical interviews
- Advanced algorithm development

The most valuable skills from linked lists are not just implementation details, but understanding:

- How references work
- How memory structures behave
- How algorithms manipulate relationships between objects
