# Trees in Python

## Table of Contents

1. Introduction to Trees
2. Tree Terminology
3. Why Trees Matter
4. Types of Trees
5. Building a Basic Tree in Python
6. Binary Trees
7. Binary Search Trees (BST)
8. Balanced Trees
9. AVL Trees
10. Red-Black Trees
11. Heaps
12. Trie (Prefix Trees)
13. Segment Trees
14. Fenwick Trees (Binary Indexed Trees)
15. Tree Traversal Algorithms
16. Depth-First Search (DFS)
17. Breadth-First Search (BFS)
18. Recursive vs Iterative Tree Algorithms
19. Tree Serialization
20. Common Interview Problems
21. Tree Complexity Analysis
22. Real-World Applications
23. Best Practices
24. Advanced Exercises
25. Summary

---

# 1. Introduction to Trees

A **tree** is a hierarchical data structure consisting of nodes connected by edges.

Unlike lists or arrays, trees represent hierarchical relationships naturally.

Examples:

- File systems
- HTML DOM
- Database indexes
- AI decision trees
- Compilers
- Network routing
- Game engines

A tree starts with a single node called the **root**.

Each node can have child nodes.

Trees are extremely important because they:

- Provide efficient searching
- Enable hierarchical organization
- Support logarithmic operations
- Power databases and operating systems

---

# 2. Tree Terminology

## Core Terms

| Term | Description |
|---|---|
| Root | Top node of the tree |
| Parent | Node containing children |
| Child | Descendant node |
| Leaf | Node without children |
| Edge | Connection between nodes |
| Height | Maximum depth of tree |
| Depth | Distance from root |
| Subtree | Smaller tree inside a tree |
| Sibling | Nodes with same parent |

## Example Structure

```text
        A
       / \
      B   C
     / \   \
    D   E   F
```

- A = root
- D/E/F = leaves
- B/C = children of A
- D/E = siblings

---

# 3. Why Trees Matter

Trees solve problems where:

- Fast lookup is required
- Data is hierarchical
- Ordered traversal is important
- Memory efficiency matters

## Performance Benefits

Balanced trees often provide:

| Operation | Time Complexity |
|---|---|
| Search | O(log n) |
| Insert | O(log n) |
| Delete | O(log n) |

This is significantly faster than linear search in lists.

---

# 4. Types of Trees

## Common Tree Structures

| Tree Type | Purpose |
|---|---|
| Binary Tree | General hierarchical structure |
| Binary Search Tree | Ordered searching |
| AVL Tree | Self-balancing BST |
| Red-Black Tree | Balanced search tree |
| Heap | Priority queues |
| Trie | String searching |
| Segment Tree | Range queries |
| Fenwick Tree | Prefix sums |
| B-Tree | Databases/filesystems |
| N-ary Tree | Multiple children per node |

---

# 5. Building a Basic Tree in Python

## Simple Node Class

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.children = []

    def add_child(self, child):
        self.children.append(child)
```

## Creating a Tree

```python
root = TreeNode("A")
child1 = TreeNode("B")
child2 = TreeNode("C")

root.add_child(child1)
root.add_child(child2)
```

## Visual Representation

```text
    A
   / \
  B   C
```

---

# 6. Binary Trees

A **binary tree** is a tree where each node has at most two children.

## Binary Tree Node

```python
class BinaryTreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```

## Building a Binary Tree

```python
root = BinaryTreeNode(1)
root.left = BinaryTreeNode(2)
root.right = BinaryTreeNode(3)
root.left.left = BinaryTreeNode(4)
root.left.right = BinaryTreeNode(5)
```

## Structure

```text
        1
       / \
      2   3
     / \
    4   5
```

---

# 7. Binary Search Trees (BST)

A BST maintains ordering:

- Left subtree values < parent
- Right subtree values > parent

## BST Implementation

```python
class BSTNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

    def insert(self, value):
        if value < self.value:
            if self.left is None:
                self.left = BSTNode(value)
            else:
                self.left.insert(value)
        else:
            if self.right is None:
                self.right = BSTNode(value)
            else:
                self.right.insert(value)
```

## Example

```python
root = BSTNode(10)
root.insert(5)
root.insert(15)
root.insert(3)
root.insert(7)
```

## Result

```text
        10
       /  \
      5    15
     / \
    3   7
```

---

# 8. Balanced Trees

A tree becomes inefficient if heavily skewed.

## Bad Case

```text
1
 \
  2
   \
    3
     \
      4
```

This behaves like a linked list.

Complexity degrades to:

| Operation | Complexity |
|---|---|
| Search | O(n) |
| Insert | O(n) |

Balanced trees avoid this problem.

---

# 9. AVL Trees

AVL trees automatically rebalance themselves.

## Balance Factor

```text
balance = height(left) - height(right)
```

Allowed values:

- -1
- 0
- 1

## Rotations

AVL trees use:

- Left rotation
- Right rotation
- Left-right rotation
- Right-left rotation

## Simple Rotation Example

### Before

```text
    30
   /
  20
 /
10
```

### After Right Rotation

```text
    20
   /  \
 10   30
```

## Simplified AVL Node

```python
class AVLNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
        self.height = 1
```

---

# 10. Red-Black Trees

Red-Black trees are self-balancing BSTs.

Each node has a color:

- Red
- Black

## Properties

1. Root is black
2. Red nodes cannot have red children
3. Every path must contain same number of black nodes

## Why They Matter

Used heavily in:

- Linux kernel
- Java TreeMap
- C++ STL map/set

They provide guaranteed:

```text
O(log n)
```

operations.

---

# 11. Heaps

A heap is a complete binary tree.

## Min Heap Property

Parent <= children

## Max Heap Property

Parent >= children

## Python heapq

```python
import heapq

heap = []

heapq.heappush(heap, 10)
heapq.heappush(heap, 5)
heapq.heappush(heap, 20)

print(heapq.heappop(heap))
```

Output:

```python
5
```

## Use Cases

- Priority queues
- Task scheduling
- Dijkstra's algorithm
- Event systems

---

# 12. Trie (Prefix Trees)

Tries store strings efficiently.

## Structure

```text
cat
car
cart
```

Shared prefixes reduce memory duplication.

## Trie Node

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.end_of_word = False
```

## Trie Class

```python
class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root

        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()

            node = node.children[char]

        node.end_of_word = True
```

## Applications

- Autocomplete
- Spell checking
- Search engines
- Dictionaries

---

# 13. Segment Trees

Segment trees answer range queries efficiently.

## Example Problems

- Sum of array range
- Minimum value in range
- Maximum value in range

## Complexity

| Operation | Complexity |
|---|---|
| Build | O(n) |
| Query | O(log n) |
| Update | O(log n) |

## Simplified Segment Tree

```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.build(arr, 1, 0, self.n - 1)

    def build(self, arr, node, start, end):
        if start == end:
            self.tree[node] = arr[start]
        else:
            mid = (start + end) // 2
            self.build(arr, 2 * node, start, mid)
            self.build(arr, 2 * node + 1, mid + 1, end)
            self.tree[node] = (
                self.tree[2 * node] + self.tree[2 * node + 1]
            )
```

---

# 14. Fenwick Trees (Binary Indexed Trees)

Fenwick trees efficiently compute prefix sums.

## Complexity

| Operation | Complexity |
|---|---|
| Update | O(log n) |
| Prefix Sum | O(log n) |

## Implementation

```python
class FenwickTree:
    def __init__(self, size):
        self.size = size
        self.tree = [0] * (size + 1)

    def update(self, index, delta):
        while index <= self.size:
            self.tree[index] += delta
            index += index & -index

    def query(self, index):
        total = 0

        while index > 0:
            total += self.tree[index]
            index -= index & -index

        return total
```

---

# 15. Tree Traversal Algorithms

Traversal means visiting every node.

## Main Traversals

| Traversal | Order |
|---|---|
| Preorder | Root → Left → Right |
| Inorder | Left → Root → Right |
| Postorder | Left → Right → Root |
| Level Order | Breadth-first |

---

# 16. Depth-First Search (DFS)

DFS explores deeply before backtracking.

## Recursive Inorder Traversal

```python
def inorder(node):
    if node:
        inorder(node.left)
        print(node.value)
        inorder(node.right)
```

## Preorder

```python
def preorder(node):
    if node:
        print(node.value)
        preorder(node.left)
        preorder(node.right)
```

## Postorder

```python
def postorder(node):
    if node:
        postorder(node.left)
        postorder(node.right)
        print(node.value)
```

---

# 17. Breadth-First Search (BFS)

BFS explores level by level.

## Level Order Traversal

```python
from collections import deque


def bfs(root):
    queue = deque([root])

    while queue:
        node = queue.popleft()
        print(node.value)

        if node.left:
            queue.append(node.left)

        if node.right:
            queue.append(node.right)
```

---

# 18. Recursive vs Iterative Tree Algorithms

## Recursive Advantages

- Cleaner code
- Easier to understand
- Natural tree representation

## Recursive Disadvantages

- Stack overflow risk
- Extra memory usage

## Iterative Advantages

- Better for very deep trees
- More memory control

## Iterative DFS

```python

def iterative_dfs(root):
    stack = [root]

    while stack:
        node = stack.pop()
        print(node.value)

        if node.right:
            stack.append(node.right)

        if node.left:
            stack.append(node.left)
```

---

# 19. Tree Serialization

Serialization converts a tree into storable/transmittable data.

## Example

```text
        1
       / \
      2   3
```

Serialized:

```python
[1, 2, 3]
```

## Recursive Serialization

```python

def serialize(root):
    result = []

    def dfs(node):
        if not node:
            result.append(None)
            return

        result.append(node.value)
        dfs(node.left)
        dfs(node.right)

    dfs(root)
    return result
```

---

# 20. Common Interview Problems

## Important Problems

### Easy

- Maximum depth
- Tree traversal
- Invert binary tree
- Same tree
- Balanced tree check

### Medium

- Lowest common ancestor
- Validate BST
- Serialize tree
- Path sum
- Level order traversal

### Hard

- Recover BST
- Binary tree cameras
- Diameter optimization
- Segment tree problems

---

# 21. Tree Complexity Analysis

## Binary Search Tree

| Operation | Average | Worst |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

## AVL / Red-Black Trees

| Operation | Complexity |
|---|---|
| Search | O(log n) |
| Insert | O(log n) |
| Delete | O(log n) |

## Heap

| Operation | Complexity |
|---|---|
| Insert | O(log n) |
| Extract Min | O(log n) |
| Peek | O(1) |

---

# 22. Real-World Applications

## Databases

B-Trees power:

- MySQL indexes
- PostgreSQL indexes
- Filesystems

## Web Browsers

DOM trees represent HTML.

## AI

Decision trees are used in:

- Classification
- Machine learning
- Random forests

## Operating Systems

Trees organize:

- Processes
- Directories
- Schedulers

---

# 23. Best Practices

## Use Dataclasses

```python
from dataclasses import dataclass


@dataclass
class Node:
    value: int
    left: 'Node' = None
    right: 'Node' = None
```

## Avoid Excessive Recursion

Use iterative methods for huge trees.

## Keep Trees Balanced

Prefer:

- AVL Trees
- Red-Black Trees
- B-Trees

## Use Existing Libraries

Python provides:

- heapq
- bisect
- networkx

---

# 24. Advanced Exercises

## Beginner

1. Implement preorder traversal
2. Count leaf nodes
3. Find tree height
4. Invert a binary tree

## Intermediate

1. Validate BST
2. Find lowest common ancestor
3. Implement heap manually
4. Serialize/deserialize tree

## Advanced

1. Implement AVL tree
2. Implement Red-Black tree
3. Build a Trie autocomplete engine
4. Implement Segment Tree with lazy propagation
5. Create a filesystem simulator

---

# 25. Summary

Trees are among the most important data structures in computer science.

You learned:

- Tree fundamentals
- Binary trees
- BSTs
- Balanced trees
- Heaps
- Tries
- Segment trees
- Fenwick trees
- DFS/BFS traversal
- Complexity analysis
- Real-world applications
