# Binary Tree Algorithms in Python

## 1. Maximum Depth of Binary Tree

### Question
Given the `root` of a binary tree, return its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

### Example
```text
Input:
        3
       / \
      9  20
         / \
        15  7

Output: 3
```

---

### Solution

### Approach
Use Depth-First Search (DFS).

For every node:
- Compute the depth of the left subtree
- Compute the depth of the right subtree
- Return `1 + max(left_depth, right_depth)`

### Python Code
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def maxDepth(self, root):
        if not root:
            return 0

        left_depth = self.maxDepth(root.left)
        right_depth = self.maxDepth(root.right)

        return 1 + max(left_depth, right_depth)
```

### Time Complexity
- `O(n)` — visit each node once

### Space Complexity
- `O(h)` — recursion stack height

---

# 2. Same Tree / Subtree Check

## Question
Given two binary trees, determine if they are the same.

Two trees are the same if:
- They are structurally identical
- Nodes contain the same values

---

## Example
```text
Tree 1:       Tree 2:

    1             1
   / \           / \
  2   3         2   3

Output: True
```

---

## Solution

### Approach
Use recursion:
- If both nodes are `None`, return `True`
- If only one is `None`, return `False`
- If values differ, return `False`
- Recursively compare left and right subtrees

---

### Python Code
```python
class Solution:
    def isSameTree(self, p, q):
        if not p and not q:
            return True

        if not p or not q:
            return False

        if p.val != q.val:
            return False

        return (
            self.isSameTree(p.left, q.left)
            and self.isSameTree(p.right, q.right)
        )
```

### Time Complexity
- `O(n)`

### Space Complexity
- `O(h)`

---

# 3. Lowest Common Ancestor

## Question
Given a binary tree and two nodes `p` and `q`, return their lowest common ancestor (LCA).

The LCA is the lowest node that has both `p` and `q` as descendants.

---

## Example
```text
        3
       / \
      5   1
     / \ / \
    6  2 0  8

LCA of 5 and 1 = 3
```

---

## Solution

### Approach
Use recursion:
- If current node is `None`, return `None`
- If current node is `p` or `q`, return it
- Search left and right subtrees
- If both sides return nodes, current node is the LCA

---

### Python Code
```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if not root or root == p or root == q:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root

        return left if left else right
```

### Time Complexity
- `O(n)`

### Space Complexity
- `O(h)`

---

# 4. Binary Tree Level Order Traversal

## Question
Given the root of a binary tree, return its level order traversal.

Return values level-by-level from left to right.

---

## Example
```text
Input:
        3
       / \
      9  20
         / \
        15  7

Output:
[
  [3],
  [9, 20],
  [15, 7]
]
```

---

## Solution

### Approach
Use Breadth-First Search (BFS) with a queue.

Steps:
1. Add root to queue
2. Process nodes level by level
3. Store values for each level

---

### Python Code
```python
from collections import deque


class Solution:
    def levelOrder(self, root):
        if not root:
            return []

        result = []
        queue = deque([root])

        while queue:
            level_size = len(queue)
            level = []

            for _ in range(level_size):
                node = queue.popleft()
                level.append(node.val)

                if node.left:
                    queue.append(node.left)

                if node.right:
                    queue.append(node.right)

            result.append(level)

        return result
```

### Time Complexity
- `O(n)`

### Space Complexity
- `O(n)`

---

# 5. Validate Binary Search Tree

## Question
Given the root of a binary tree, determine if it is a valid Binary Search Tree (BST).

BST Rules:
- Left subtree values must be smaller
- Right subtree values must be larger
- Both subtrees must also be BSTs

---

## Example
```text
        2
       / \
      1   3

Output: True
```

---

## Solution

### Approach
Use recursion with value boundaries:
- Each node must lie within `(min_val, max_val)`
- Left subtree updates max bound
- Right subtree updates min bound

---

### Python Code
```python
class Solution:
    def isValidBST(self, root):

        def validate(node, low, high):
            if not node:
                return True

            if not (low < node.val < high):
                return False

            return (
                validate(node.left, low, node.val)
                and validate(node.right, node.val, high)
            )

        return validate(root, float("-inf"), float("inf"))
```

### Time Complexity
- `O(n)`

### Space Complexity
- `O(h)`

---

# Summary of Patterns

| Problem | Main Pattern |
|---|---|
| Maximum Depth | DFS |
| Same Tree | Recursive Comparison |
| Lowest Common Ancestor | Tree Recursion |
| Level Order Traversal | BFS / Queue |
| Validate BST | DFS + Bounds |
