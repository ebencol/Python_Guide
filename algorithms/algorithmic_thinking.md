# Algorithmic Thinking in Python

## Table of Contents

1. Introduction to Algorithmic Thinking
2. Time Complexity and Big-O Analysis
3. Python Techniques for Algorithms
4. Recursion and Backtracking
5. Sorting Algorithms
6. Searching Algorithms
7. Sliding Window Algorithms
8. Two Pointer Techniques
9. Prefix Sum and Difference Arrays
10. Hashing and Frequency Counting
11. Stack and Queue Algorithms
12. Heap Algorithms and Priority Queues
13. Linked List Algorithms
14. Tree Algorithms
15. Graph Algorithms
16. Dynamic Programming
17. Greedy Algorithms
18. Bit Manipulation Algorithms
19. String Algorithms
20. Advanced Problem-Solving Strategies
21. Competitive Programming Tips
22. Practice Problems
23. Final Advice

---

# 1. Introduction to Algorithmic Thinking

Algorithms are step-by-step procedures for solving problems efficiently.

A strong algorithmic mindset focuses on:

- Breaking problems into smaller parts
- Recognizing patterns
- Choosing appropriate data structures
- Optimizing time and memory usage
- Writing clean and maintainable solutions

## Problem-Solving Workflow

### Step 1: Understand the Problem

Ask:

- What are the inputs?
- What are the outputs?
- Are there constraints?
- Are there edge cases?

### Step 2: Solve Brute Force First

Start simple.

Brute force often reveals:

- Hidden patterns
- Optimization opportunities
- Necessary data structures

### Step 3: Optimize

Look for:

- Repeated calculations
- Unnecessary loops
- Better data structures

### Step 4: Analyze Complexity

Always evaluate:

- Time complexity
- Space complexity

---

# 2. Time Complexity and Big-O Analysis

## Common Complexities

| Complexity | Name | Example |
|---|---|---|
| O(1) | Constant | Dictionary lookup |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Single loop |
| O(n log n) | Linearithmic | Merge sort |
| O(n²) | Quadratic | Nested loops |
| O(2ⁿ) | Exponential | Naive recursion |
| O(n!) | Factorial | Permutations |

## Example: Complexity Comparison

### O(n)

```python
for x in arr:
    print(x)
```

### O(n²)

```python
for i in arr:
    for j in arr:
        print(i, j)
```

## Important Complexity Rules

### Sequential Operations

```python
O(n) + O(n) = O(n)
```

### Nested Loops

```python
O(n) * O(n) = O(n²)
```

### Ignore Constants

```python
O(2n) => O(n)
```

---

# 3. Python Techniques for Algorithms

## List Comprehensions

```python
squares = [x * x for x in range(10)]
```

## Enumerate

```python
for index, value in enumerate(arr):
    print(index, value)
```

## Zip

```python
for a, b in zip(list1, list2):
    print(a, b)
```

## Collections Module

### Counter

```python
from collections import Counter

freq = Counter("banana")
print(freq)
```

### defaultdict

```python
from collections import defaultdict

graph = defaultdict(list)
```

### deque

```python
from collections import deque

queue = deque([1, 2, 3])
queue.append(4)
queue.popleft()
```

## Heapq

```python
import heapq

nums = [5, 2, 8, 1]
heapq.heapify(nums)

print(heapq.heappop(nums))
```

---

# 4. Recursion and Backtracking

## Recursion Fundamentals

A recursive function calls itself.

### Example: Factorial

```python
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)
```

## Recursion Components

Every recursive solution needs:

1. Base case
2. Recursive case

## Common Recursive Problems

- Tree traversal
- DFS
- Backtracking
- Divide and conquer

---

## Backtracking

Backtracking explores all possible solutions.

### Template

```python
def backtrack(path, choices):
    if goal_reached(path):
        result.append(path[:])
        return

    for choice in choices:
        make_choice(choice)
        backtrack(path, choices)
        undo_choice(choice)
```

---

## Example: Generate Permutations

```python
def permute(nums):
    result = []

    def backtrack(path, used):
        if len(path) == len(nums):
            result.append(path[:])
            return

        for i in range(len(nums)):
            if used[i]:
                continue

            used[i] = True
            path.append(nums[i])

            backtrack(path, used)

            path.pop()
            used[i] = False

    backtrack([], [False] * len(nums))
    return result
```

---

# 5. Sorting Algorithms

## Bubble Sort

### Idea

Repeatedly swap adjacent elements.

### Complexity

- Time: O(n²)
- Space: O(1)

```python
def bubble_sort(arr):
    n = len(arr)

    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]

    return arr
```

---

## Merge Sort

### Idea

Divide array into halves.

### Complexity

- Time: O(n log n)
- Space: O(n)

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2

    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)


def merge(left, right):
    result = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])

    return result
```

---

## Quick Sort

### Complexity

- Average: O(n log n)
- Worst: O(n²)

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr

    pivot = arr[len(arr) // 2]

    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]

    return quick_sort(left) + middle + quick_sort(right)
```

---

# 6. Searching Algorithms

## Linear Search

```python
def linear_search(arr, target):
    for i, value in enumerate(arr):
        if value == target:
            return i
    return -1
```

---

## Binary Search

Requires sorted array.

### Complexity

- Time: O(log n)

```python
def binary_search(arr, target):
    left = 0
    right = len(arr) - 1

    while left <= right:
        mid = (left + right) // 2

        if arr[mid] == target:
            return mid

        if arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

---

# 7. Sliding Window Algorithms

Sliding window optimizes subarray problems.

## Example: Maximum Sum Subarray of Size K

```python
def max_sum_subarray(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum

    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]
        max_sum = max(max_sum, window_sum)

    return max_sum
```

### Complexity

- Time: O(n)

---

## Variable Window Example

### Longest Substring Without Repeating Characters

```python
def longest_unique_substring(s):
    seen = set()
    left = 0
    max_len = 0

    for right in range(len(s)):
        while s[right] in seen:
            seen.remove(s[left])
            left += 1

        seen.add(s[right])
        max_len = max(max_len, right - left + 1)

    return max_len
```

---

# 8. Two Pointer Techniques

Useful for sorted arrays.

## Example: Pair Sum

```python
def pair_sum(arr, target):
    left = 0
    right = len(arr) - 1

    while left < right:
        current = arr[left] + arr[right]

        if current == target:
            return [left, right]

        if current < target:
            left += 1
        else:
            right -= 1

    return []
```

---

# 9. Prefix Sum and Difference Arrays

## Prefix Sum

### Idea

Store cumulative sums.

```python
def prefix_sum(arr):
    prefix = [0]

    for num in arr:
        prefix.append(prefix[-1] + num)

    return prefix
```

### Range Query

```python
sum_range = prefix[right + 1] - prefix[left]
```

---

# 10. Hashing and Frequency Counting

## Frequency Counting

```python
from collections import Counter

nums = [1, 2, 2, 3, 3, 3]
count = Counter(nums)
```

---

## Example: Two Sum

```python
def two_sum(nums, target):
    lookup = {}

    for i, num in enumerate(nums):
        complement = target - num

        if complement in lookup:
            return [lookup[complement], i]

        lookup[num] = i
```

### Complexity

- Time: O(n)
- Space: O(n)

---

# 11. Stack and Queue Algorithms

## Stack

### LIFO Structure

```python
stack = []
stack.append(1)
stack.pop()
```

---

## Example: Valid Parentheses

```python
def is_valid(s):
    stack = []
    mapping = {")": "(", "]": "[", "}": "{"}

    for char in s:
        if char in mapping:
            top = stack.pop() if stack else '#'

            if mapping[char] != top:
                return False
        else:
            stack.append(char)

    return not stack
```

---

## Queue

```python
from collections import deque

queue = deque()
queue.append(1)
queue.popleft()
```

---

# 12. Heap Algorithms and Priority Queues

## Min Heap

```python
import heapq

heap = []
heapq.heappush(heap, 5)
heapq.heappush(heap, 1)
heapq.heappush(heap, 10)

print(heapq.heappop(heap))
```

---

## Top K Elements

```python
import heapq


def top_k(nums, k):
    return heapq.nlargest(k, nums)
```

---

## Kth Largest Element

```python
import heapq


def kth_largest(nums, k):
    return heapq.nlargest(k, nums)[-1]
```

---

# 13. Linked List Algorithms

## Node Structure

```python
class ListNode:
    def __init__(self, value=0, next=None):
        self.value = value
        self.next = next
```

---

## Reverse Linked List

```python
def reverse_list(head):
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

## Detect Cycle

### Floyd's Algorithm

```python
def has_cycle(head):
    slow = fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

        if slow == fast:
            return True

    return False
```

---

# 14. Tree Algorithms

## Binary Tree Node

```python
class TreeNode:
    def __init__(self, value=0, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right
```

---

## DFS Traversals

### Inorder

```python
def inorder(root):
    if not root:
        return

    inorder(root.left)
    print(root.value)
    inorder(root.right)
```

---

## BFS Traversal

```python
from collections import deque


def bfs(root):
    if not root:
        return

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

## Lowest Common Ancestor

```python
def lca(root, p, q):
    if not root or root == p or root == q:
        return root

    left = lca(root.left, p, q)
    right = lca(root.right, p, q)

    if left and right:
        return root

    return left or right
```

---

# 15. Graph Algorithms

## Graph Representation

```python
from collections import defaultdict

graph = defaultdict(list)

graph[0].append(1)
graph[1].append(2)
```

---

## Depth First Search

```python
def dfs(graph, node, visited):
    visited.add(node)

    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

---

## Breadth First Search

```python
from collections import deque


def bfs(graph, start):
    visited = set([start])
    queue = deque([start])

    while queue:
        node = queue.popleft()
        print(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

---

## Dijkstra's Algorithm

```python
import heapq


def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0

    heap = [(0, start)]

    while heap:
        current_distance, current_node = heapq.heappop(heap)

        if current_distance > distances[current_node]:
            continue

        for neighbor, weight in graph[current_node]:
            distance = current_distance + weight

            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(heap, (distance, neighbor))

    return distances
```

---

# 16. Dynamic Programming

Dynamic Programming solves overlapping subproblems.

## DP Recognition Checklist

Use DP when:

- Problems have overlapping subproblems
- Problems have optimal substructure
- Brute force recursion repeats work

---

## Fibonacci: Naive Recursion

```python
def fib(n):
    if n <= 1:
        return n

    return fib(n - 1) + fib(n - 2)
```

### Complexity

- O(2ⁿ)

---

## Fibonacci with Memoization

```python
def fib(n, memo={}):
    if n in memo:
        return memo[n]

    if n <= 1:
        return n

    memo[n] = fib(n - 1, memo) + fib(n - 2, memo)

    return memo[n]
```

### Complexity

- O(n)

---

## Bottom-Up DP

```python
def fib(n):
    if n <= 1:
        return n

    dp = [0] * (n + 1)
    dp[1] = 1

    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]

    return dp[n]
```

---

## Example: Coin Change

```python
def coin_change(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0

    for coin in coins:
        for x in range(coin, amount + 1):
            dp[x] = min(dp[x], dp[x - coin] + 1)

    return dp[amount] if dp[amount] != float('inf') else -1
```

---

# 17. Greedy Algorithms

Greedy algorithms choose locally optimal solutions.

## Example: Activity Selection

```python
def activity_selection(intervals):
    intervals.sort(key=lambda x: x[1])

    count = 0
    end = float('-inf')

    for start, finish in intervals:
        if start >= end:
            count += 1
            end = finish

    return count
```

---

# 18. Bit Manipulation Algorithms

## Basic Operators

| Operator | Meaning |
|---|---|
| & | AND |
| | | OR |
| ^ | XOR |
| ~ | NOT |
| << | Left Shift |
| >> | Right Shift |

---

## Check Even/Odd

```python
def is_even(n):
    return (n & 1) == 0
```

---

## Count Set Bits

```python
def count_bits(n):
    count = 0

    while n:
        count += n & 1
        n >>= 1

    return count
```

---

# 19. String Algorithms

## Reverse String

```python
def reverse_string(s):
    return s[::-1]
```

---

## Palindrome Check

```python
def is_palindrome(s):
    return s == s[::-1]
```

---

## Anagram Check

```python
from collections import Counter


def is_anagram(a, b):
    return Counter(a) == Counter(b)
```

---

## Longest Common Prefix

```python
def longest_common_prefix(strs):
    if not strs:
        return ""

    prefix = strs[0]

    for s in strs[1:]:
        while not s.startswith(prefix):
            prefix = prefix[:-1]

    return prefix
```

---

# 20. Advanced Problem-Solving Strategies

## Pattern Recognition

Common patterns:

| Pattern | Typical Problems |
|---|---|
| Sliding Window | Subarrays/substrings |
| Two Pointers | Sorted arrays |
| Binary Search | Monotonic conditions |
| DFS/BFS | Trees/graphs |
| Dynamic Programming | Optimization |
| Greedy | Interval scheduling |
| Heap | Top K problems |
| Backtracking | Permutations/combinations |

---

## Ask Key Questions

### Does order matter?

- Yes → permutations
- No → combinations

### Is input sorted?

- Yes → two pointers/binary search

### Are repeated calculations occurring?

- Yes → dynamic programming

### Need shortest path?

- Unweighted graph → BFS
- Weighted graph → Dijkstra

---

# 21. Competitive Programming Tips

## Improve Speed

### Fast Input

```python
import sys
input = sys.stdin.readline
```

---

## Avoid Common Mistakes

### Mutable Default Arguments

Bad:

```python
def func(arr=[]):
    pass
```

Good:

```python
def func(arr=None):
    if arr is None:
        arr = []
```

---

## Use Built-In Functions

Python built-ins are optimized.

Examples:

```python
sum(arr)
max(arr)
min(arr)
sorted(arr)
```

---

# 22. Practice Problems

## Easy

1. Two Sum
2. Valid Parentheses
3. Maximum Subarray
4. Merge Sorted Arrays
5. Binary Search

---

## Medium

1. Longest Substring Without Repeating Characters
2. Group Anagrams
3. Coin Change
4. Number of Islands
5. Top K Frequent Elements

---

## Hard

1. Word Ladder
2. Median of Two Sorted Arrays
3. N-Queens
4. Trapping Rain Water
5. Regular Expression Matching

---

# 23. Final Advice

## Best Learning Strategy

### 1. Learn Patterns

Focus on:

- Sliding window
- DFS/BFS
- Dynamic programming
- Greedy algorithms
- Graph traversal

---

### 2. Solve Problems Daily

Consistency matters more than intensity.

---

### 3. Revisit Problems

A problem truly becomes learned when:

- You can solve it again later
- You can explain it clearly
- You can recognize its pattern elsewhere

---

### 4. Analyze Other Solutions

Study:

- Cleaner implementations
- Better optimizations
- Alternative approaches

---

### 5. Build Algorithm Intuition

Ask yourself:

- Why does this work?
- Why is this efficient?
- What are the tradeoffs?

---

# Recommended Platforms

- LeetCode
- HackerRank
- Codeforces
- AtCoder
- Codewars

---

# Recommended Python Libraries for Algorithms

| Library | Purpose |
|---|---|
| collections | Efficient containers |
| heapq | Priority queues |
| bisect | Binary search utilities |
| itertools | Combinatorics |
| math | Mathematical functions |
| functools | Memoization |

---

# Final Thoughts

Mastering algorithms requires:

- Repetition
- Pattern recognition
- Curiosity
- Consistent practice

The goal is not memorizing solutions.

The goal is understanding how to think algorithmically.
