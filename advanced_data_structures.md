# Linked Lists, Trees, and Graphs

## Table of Contents

1. Introduction
2. Linked Lists
   - What is a Linked List?
   - Types of Linked Lists
   - Implementing Linked Lists in Python
   - Common Operations
   - Time Complexity
   - Use Cases
3. Trees
   - What is a Tree?
   - Terminology
   - Types of Trees
   - Binary Tree Implementation
   - Binary Search Tree (BST)
   - Tree Traversal Algorithms
   - Time Complexity
   - Use Cases
4. Graphs
   - What is a Graph?
   - Terminology
   - Types of Graphs
   - Graph Representations
   - Graph Traversal Algorithms
   - Shortest Path Algorithms
   - Time Complexity
   - Use Cases
5. Comparison of Linked Lists, Trees, and Graphs
6. Practical Project Ideas
7. Summary

---

# 1. Introduction

Data structures are ways of organizing and storing data efficiently. Choosing the correct data structure can significantly improve the performance and maintainability of software.

This tutorial focuses on three important non-linear and dynamic data structures:

- Linked Lists
- Trees
- Graphs

These structures are widely used in:

- Operating systems
- Databases
- Artificial intelligence
- Social networks
- File systems
- Compilers
- Web applications
- Networking systems

---

# 2. Linked Lists

## What is a Linked List?

A linked list is a linear data structure where each element is stored in a separate object called a node.

Each node contains:

1. Data
2. Reference (pointer) to the next node

Unlike Python lists, linked lists do not store elements in contiguous memory.

## Visual Representation

```text
[10 | *] -> [20 | *] -> [30 | None]
```

---

## Types of Linked Lists

### 1. Singly Linked List

Each node points to the next node.

```text
A -> B -> C
```

### 2. Doubly Linked List

Each node points to both previous and next nodes.

```text
None <- A <-> B <-> C -> None
```

### 3. Circular Linked List

The last node points back to the first node.

```text
A -> B -> C -> A
```

---

## Implementing a Singly Linked List in Python

### Step 1: Create a Node Class

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

---

### Step 2: Create the Linked List Class

```python
class LinkedList:
    def __init__(self):
        self.head = None
```

---

## Insert Operations

### Insert at Beginning

```python
class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_beginning(self, data):
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
```

### Example

```python
ll = LinkedList()
ll.insert_at_beginning(10)
ll.insert_at_beginning(20)
```

Result:

```text
20 -> 10
```

---

### Insert at End

```python
def insert_at_end(self, data):
    new_node = Node(data)

    if not self.head:
        self.head = new_node
        return

    current = self.head

    while current.next:
        current = current.next

    current.next = new_node
```

---

## Traversing a Linked List

```python
def display(self):
    current = self.head

    while current:
        print(current.data, end=" -> ")
        current = current.next

    print("None")
```

### Example

```python
ll = LinkedList()
ll.insert_at_end(1)
ll.insert_at_end(2)
ll.insert_at_end(3)

ll.display()
```

Output:

```text
1 -> 2 -> 3 -> None
```

---

## Delete a Node

```python
def delete_node(self, key):
    current = self.head

    if current and current.data == key:
        self.head = current.next
        current = None
        return

    prev = None

    while current and current.data != key:
        prev = current
        current = current.next

    if current is None:
        return

    prev.next = current.next
    current = None
```

---

## Searching in a Linked List

```python
def search(self, key):
    current = self.head

    while current:
        if current.data == key:
            return True
        current = current.next

    return False
```

---

## Time Complexity of Linked Lists

| Operation | Complexity |
|---|---|
| Insert at Beginning | O(1) |
| Insert at End | O(n) |
| Delete | O(n) |
| Search | O(n) |
| Access by Index | O(n) |

---

## Advantages of Linked Lists

- Dynamic size
- Efficient insertion/deletion
- Memory efficient for unknown data sizes

## Disadvantages

- Slow random access
- Extra memory for pointers
- More complex than arrays

---

## Real-World Use Cases of Linked Lists

### 1. Music Playlist

Songs can be linked sequentially.

```text
Song A -> Song B -> Song C
```

### 2. Browser History

Doubly linked lists allow:

- Back navigation
- Forward navigation

### 3. Undo/Redo Systems

Applications like:

- Text editors
- Photoshop
- IDEs

use linked structures to manage actions.

### 4. Hash Tables

Collision handling often uses linked lists.

---

## Example: Browser History Using Doubly Linked List

```python
class BrowserPage:
    def __init__(self, url):
        self.url = url
        self.prev = None
        self.next = None
```

This structure allows moving backward and forward efficiently.

---

# 3. Trees

## What is a Tree?

A tree is a hierarchical data structure consisting of nodes connected by edges.

Unlike linked lists, trees branch into multiple directions.

## Example Structure

```text
        A
      /   \
     B     C
    / \   / \
   D   E F   G
```

---

## Tree Terminology

| Term | Meaning |
|---|---|
| Root | Top node |
| Parent | Node with children |
| Child | Node connected downward |
| Leaf | Node without children |
| Edge | Connection between nodes |
| Height | Longest path from root |
| Subtree | Smaller tree inside tree |

---

## Types of Trees

### 1. Binary Tree

Each node has at most two children.

### 2. Binary Search Tree (BST)

Rules:

- Left child < Parent
- Right child > Parent

### 3. AVL Tree

Self-balancing BST.

### 4. Heap

Used for priority queues.

### 5. Trie

Used for fast text searching.

---

# Binary Tree Implementation

## Node Class

```python
class TreeNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None
```

---

## Creating a Tree

```python
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)
```

Tree:

```text
      1
     / \
    2   3
   / \
  4   5
```

---

# Tree Traversal Algorithms

Traversal means visiting all nodes.

## 1. Inorder Traversal

Order:

```text
Left -> Root -> Right
```

```python
def inorder(node):
    if node:
        inorder(node.left)
        print(node.data)
        inorder(node.right)
```

---

## 2. Preorder Traversal

Order:

```text
Root -> Left -> Right
```

```python
def preorder(node):
    if node:
        print(node.data)
        preorder(node.left)
        preorder(node.right)
```

---

## 3. Postorder Traversal

Order:

```text
Left -> Right -> Root
```

```python
def postorder(node):
    if node:
        postorder(node.left)
        postorder(node.right)
        print(node.data)
```

---

# Binary Search Tree (BST)

## Why Use BST?

BSTs allow efficient:

- Searching
- Insertion
- Deletion

---

## Insert in BST

```python
class BST:
    def __init__(self):
        self.root = None

    def insert(self, root, key):
        if root is None:
            return TreeNode(key)

        if key < root.data:
            root.left = self.insert(root.left, key)
        else:
            root.right = self.insert(root.right, key)

        return root
```

---

## Search in BST

```python
def search(root, key):
    if root is None or root.data == key:
        return root

    if key < root.data:
        return search(root.left, key)

    return search(root.right, key)
```

---

## Time Complexity of Trees

| Operation | Balanced BST | Worst Case |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

---

## Real-World Use Cases of Trees

### 1. File Systems

Operating systems organize directories as trees.

```text
Root
├── Documents
├── Downloads
└── Pictures
```

### 2. Databases

B-Trees and B+ Trees are used for indexing.

### 3. HTML/XML Parsing

Web pages are represented as DOM trees.

### 4. Autocomplete Systems

Tries enable fast prefix searching.

### 5. Artificial Intelligence

Decision trees are used in machine learning.

---

## Example: Decision Tree Logic

```text
Is temperature > 30?
├── Yes -> Turn on AC
└── No -> Keep AC Off
```

---

# 4. Graphs

## What is a Graph?

A graph is a collection of:

- Vertices (nodes)
- Edges (connections)

Graphs model relationships between objects.

---

## Example Graph

```text
A ---- B
|      |
|      |
C ---- D
```

---

## Graph Terminology

| Term | Meaning |
|---|---|
| Vertex | Node |
| Edge | Connection |
| Directed Graph | One-way edges |
| Undirected Graph | Two-way edges |
| Weighted Graph | Edges have costs |
| Path | Sequence of vertices |
| Cycle | Circular path |

---

## Types of Graphs

### 1. Directed Graph

```text
A -> B -> C
```

### 2. Undirected Graph

```text
A -- B
```

### 3. Weighted Graph

```text
A --5-- B
```

### 4. Cyclic Graph

Contains loops.

### 5. Acyclic Graph

No loops.

---

# Graph Representations

## 1. Adjacency List

Most common representation.

```python
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A', 'D'],
    'D': ['B', 'C']
}
```

---

## 2. Adjacency Matrix

```python
graph_matrix = [
    [0, 1, 1, 0],
    [1, 0, 0, 1],
    [1, 0, 0, 1],
    [0, 1, 1, 0]
]
```

---

# Graph Traversal Algorithms

## Breadth-First Search (BFS)

BFS explores neighbors level by level.

### BFS Implementation

```python
from collections import deque


def bfs(graph, start):
    visited = set()
    queue = deque([start])

    while queue:
        vertex = queue.popleft()

        if vertex not in visited:
            print(vertex)
            visited.add(vertex)
            queue.extend(graph[vertex])
```

---

## Depth-First Search (DFS)

DFS explores as deep as possible.

### DFS Implementation

```python
def dfs(graph, vertex, visited=None):
    if visited is None:
        visited = set()

    visited.add(vertex)
    print(vertex)

    for neighbor in graph[vertex]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

---

# Shortest Path: Dijkstra's Algorithm

Used in weighted graphs.

## Example

```python
import heapq


def dijkstra(graph, start):
    queue = [(0, start)]
    distances = {node: float('inf') for node in graph}
    distances[start] = 0

    while queue:
        current_distance, current_node = heapq.heappop(queue)

        for neighbor, weight in graph[current_node]:
            distance = current_distance + weight

            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(queue, (distance, neighbor))

    return distances
```

---

## Time Complexity of Graph Operations

| Operation | Complexity |
|---|---|
| BFS | O(V + E) |
| DFS | O(V + E) |
| Dijkstra | O((V + E) log V) |

Where:

- V = Vertices
- E = Edges

---

## Real-World Use Cases of Graphs

### 1. Social Networks

Users connected as friends/followers.

```text
Alice -- Bob -- Carol
```

### 2. GPS Navigation

Roads and cities form weighted graphs.

Algorithms find shortest paths.

### 3. Recommendation Systems

Netflix and Amazon use graphs to model:

- Users
- Products
- Interests

### 4. Computer Networks

Routers and switches form graphs.

### 5. Dependency Management

Package managers use directed graphs.

Example:

```text
Flask -> Werkzeug
Flask -> Jinja2
```

---

## Example: Social Network Graph

```python
social_network = {
    'Alice': ['Bob', 'Charlie'],
    'Bob': ['Alice', 'David'],
    'Charlie': ['Alice'],
    'David': ['Bob']
}
```

---

# 5. Comparison of Linked Lists, Trees, and Graphs

| Feature | Linked List | Tree | Graph |
|---|---|---|---|
| Structure | Linear | Hierarchical | Network |
| Connections | Sequential | Parent-child | Arbitrary |
| Traversal | Sequential | DFS/BFS | DFS/BFS |
| Cycles | Rare | Usually no | Common |
| Use Cases | Playlists | File systems | Social networks |

---

# 6. Practical Project Ideas

## Linked List Projects

- Browser history manager
- Music playlist
- Undo/redo system
- Task scheduler

---

## Tree Projects

- File explorer
- Expression parser
- Decision tree AI
- Autocomplete engine

---

## Graph Projects

- Route planner
- Social network analyzer
- Recommendation engine
- Network topology visualizer

---

# 7. Summary

## Linked Lists

Best for:

- Dynamic insertion/deletion
- Sequential processing

Common in:

- Browser history
- Playlists
- Undo systems

---

## Trees

Best for:

- Hierarchical data
- Fast searching

Common in:

- File systems
- Databases
- AI models

---

## Graphs

Best for:

- Relationship modeling
- Network analysis
- Pathfinding

Common in:

- GPS systems
- Social networks
- Recommendation systems

---

These data structures form the foundation of advanced computer science concepts and efficient software design.
