# Big O Notation

## Table of Contents

1. Introduction to Algorithm Analysis
2. Why Big O Matters
3. Time Complexity vs Space Complexity
4. Understanding Growth Rates
5. Common Complexity Classes
6. Visualizing Complexity Intuition
7. Rules for Calculating Big O
8. Analyzing Python Operations
9. Complexity of Python Data Structures
10. Complexity of Python Built-in Functions
11. Nested Loops and Combinations
12. Recursion and Recurrence Relations
13. Divide and Conquer Complexity
14. Amortized Analysis
15. Logarithmic Complexity Deep Dive
16. Dynamic Programming Complexity
17. Graph Algorithm Complexity
18. Sorting Algorithm Complexity
19. Space Complexity in Depth
20. Best, Average, and Worst Case
21. Tradeoffs in Real Systems
22. Big O Misconceptions
23. Practical Optimization Strategies
24. Interview-Style Examples
25. Advanced Practice Problems
26. Complexity Cheat Sheet
27. Final Thoughts

---

# 1. Introduction to Algorithm Analysis

Big O notation describes how the runtime or memory usage of an algorithm grows as the input size increases.

It does **not** measure exact execution time.

Instead, it measures:

- Scalability
- Growth rate
- Relative efficiency
- Performance trends

For example:

- An algorithm taking `10n` operations grows linearly.
- Another taking `n²` operations grows quadratically.

Even if the quadratic algorithm is initially faster for small inputs, the linear algorithm will eventually outperform it for large enough input sizes.

---

# 2. Why Big O Matters

Big O is essential because modern software often processes:

- Millions of records
- Massive graphs
- Real-time streams
- Large databases
- High-frequency requests

A poorly chosen algorithm can become unusable at scale.

## Example

Suppose:

| Input Size | O(n) | O(n²) |
|---|---|---|
| 100 | 100 | 10,000 |
| 1,000 | 1,000 | 1,000,000 |
| 1,000,000 | 1,000,000 | 1,000,000,000,000 |

The quadratic solution becomes catastrophically expensive.

---

# 3. Time Complexity vs Space Complexity

## Time Complexity

Measures how execution time grows.

Example:

```python
for x in arr:
    print(x)
```

This is O(n).

---

## Space Complexity

Measures additional memory usage.

Example:

```python
new_arr = []

for x in arr:
    new_arr.append(x * 2)
```

This uses O(n) extra space.

---

# 4. Understanding Growth Rates

Big O focuses on asymptotic growth.

As input grows very large:

- Constants matter less
- Growth rate matters most

## Example

```python
def example(arr):
    for x in arr:
        print(x)

    for x in arr:
        print(x)
```

Runtime:

```text
n + n = 2n
```

Big O:

```text
O(n)
```

We drop constants.

---

# 5. Common Complexity Classes

## O(1) — Constant Time

Runtime never changes with input size.

```python
value = arr[0]
```

Examples:

- Hash table lookup (average case)
- Stack push/pop
- Array indexing

---

## O(log n) — Logarithmic Time

Input shrinks each iteration.

Example: Binary search.

```python
def binary_search(arr, target):
    left = 0
    right = len(arr) - 1

    while left <= right:
        mid = (left + right) // 2

        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

Each iteration halves the search space.

---

## O(n) — Linear Time

Runtime grows proportionally.

```python
for x in arr:
    print(x)
```

---

## O(n log n)

Common in efficient sorting algorithms.

Examples:

- Merge sort
- Heap sort
- Timsort (Python sort)

---

## O(n²) — Quadratic Time

Usually nested loops.

```python
for i in arr:
    for j in arr:
        print(i, j)
```

---

## O(2ⁿ) — Exponential Time

Common in brute-force recursion.

```python
def fib(n):
    if n <= 1:
        return n

    return fib(n - 1) + fib(n - 2)
```

This becomes extremely slow.

---

## O(n!) — Factorial Time

Generates permutations.

```python
from itertools import permutations
```

Very expensive.

---

# 6. Visualizing Complexity Intuition

From best to worst:

```text
O(1)
O(log n)
O(n)
O(n log n)
O(n²)
O(n³)
O(2ⁿ)
O(n!)
```

A small increase in complexity class can create enormous performance differences.

---

# 7. Rules for Calculating Big O

## Rule 1: Drop Constants

```python
for x in arr:
    print(x)

for y in arr:
    print(y)
```

Complexity:

```text
O(2n) → O(n)
```

---

## Rule 2: Keep Dominant Terms

```text
O(n² + n + 100)
```

Becomes:

```text
O(n²)
```

---

## Rule 3: Different Inputs Use Different Variables

```python
for a in arr1:
    print(a)

for b in arr2:
    print(b)
```

Complexity:

```text
O(a + b)
```

---

## Rule 4: Nested Loops Multiply

```python
for i in arr:
    for j in arr:
        print(i, j)
```

Complexity:

```text
O(n²)
```

---

# 8. Analyzing Python Operations

## List Indexing

```python
arr[5]
```

Complexity:

```text
O(1)
```

---

## List Append

```python
arr.append(x)
```

Average:

```text
O(1)
```

Worst case:

```text
O(n)
```

Because resizing may occur.

---

## List Insert at Beginning

```python
arr.insert(0, x)
```

Complexity:

```text
O(n)
```

Elements must shift.

---

## Dictionary Lookup

```python
value = d[key]
```

Average:

```text
O(1)
```

Worst case:

```text
O(n)
```

---

# 9. Complexity of Python Data Structures

| Operation | List | Dict | Set | Deque |
|---|---|---|---|---|
| Access | O(1) | O(1) | — | O(1) |
| Append | O(1) | — | O(1) | O(1) |
| Insert Front | O(n) | — | — | O(1) |
| Remove | O(n) | O(1) | O(1) | O(1) |
| Search | O(n) | O(1) | O(1) | O(n) |

---

# 10. Complexity of Python Built-in Functions

## Sorting

```python
arr.sort()
```

Complexity:

```text
O(n log n)
```

Python uses Timsort.

---

## Membership Test

### List

```python
x in arr
```

Complexity:

```text
O(n)
```

### Set

```python
x in my_set
```

Complexity:

```text
O(1)
```

---

## String Concatenation

Repeated concatenation is expensive.

```python
s = ""

for x in arr:
    s += x
```

Complexity:

```text
O(n²)
```

Better:

```python
"".join(arr)
```

Complexity:

```text
O(n)
```

---

# 11. Nested Loops and Combinations

Nested loops are not always quadratic.

## Example 1

```python
for i in range(n):
    for j in range(n):
        pass
```

Complexity:

```text
O(n²)
```

---

## Example 2

```python
i = 1

while i < n:
    i *= 2
```

Complexity:

```text
O(log n)
```

---

## Example 3

```python
for i in range(n):
    j = 1

    while j < n:
        j *= 2
```

Complexity:

```text
O(n log n)
```

---

# 12. Recursion and Recurrence Relations

Recursive algorithms often require recurrence analysis.

## Example: Merge Sort

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2

    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)
```

Recurrence:

```text
T(n) = 2T(n/2) + O(n)
```

Result:

```text
O(n log n)
```

---

# 13. Divide and Conquer Complexity

Divide and conquer algorithms:

1. Split problem
2. Solve subproblems
3. Combine results

Examples:

- Merge sort
- Quick sort
- Binary search
- FFT

---

## Master Theorem

General recurrence:

```text
T(n) = aT(n/b) + f(n)
```

Where:

- `a` = number of subproblems
- `b` = reduction factor
- `f(n)` = merge cost

Common results:

| Recurrence | Complexity |
|---|---|
| T(n)=T(n/2)+1 | O(log n) |
| T(n)=2T(n/2)+n | O(n log n) |
| T(n)=2T(n/2)+1 | O(n) |

---

# 14. Amortized Analysis

Some operations are occasionally expensive but cheap on average.

## Dynamic Array Append

Appending to a Python list:

```python
arr.append(x)
```

Sometimes resizing occurs.

But over many operations:

```text
Amortized O(1)
```

This is a critical concept.

---

# 15. Logarithmic Complexity Deep Dive

Logarithmic algorithms repeatedly reduce problem size.

## Binary Search

```python
n → n/2 → n/4 → n/8
```

Number of steps:

```text
log₂(n)
```

---

## Balanced Binary Trees

Search operations:

- AVL tree
- Red-black tree
- B-tree

Typically:

```text
O(log n)
```

---

# 16. Dynamic Programming Complexity

Dynamic programming optimizes overlapping subproblems.

## Fibonacci

### Naive Recursion

```python
def fib(n):
    if n <= 1:
        return n

    return fib(n - 1) + fib(n - 2)
```

Complexity:

```text
O(2ⁿ)
```

---

### Dynamic Programming Version

```python
def fib(n):
    dp = [0, 1]

    for i in range(2, n + 1):
        dp.append(dp[-1] + dp[-2])

    return dp[n]
```

Complexity:

```text
O(n)
```

Huge improvement.

---

# 17. Graph Algorithm Complexity

## Breadth-First Search (BFS)

```python
from collections import deque


def bfs(graph, start):
    visited = set()
    queue = deque([start])

    while queue:
        node = queue.popleft()

        if node in visited:
            continue

        visited.add(node)

        for neighbor in graph[node]:
            queue.append(neighbor)
```

Complexity:

```text
O(V + E)
```

Where:

- `V` = vertices
- `E` = edges

---

## Dijkstra's Algorithm

Using a heap:

```text
O((V + E) log V)
```

---

# 18. Sorting Algorithm Complexity

| Algorithm | Best | Average | Worst |
|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) |
| Selection Sort | O(n²) | O(n²) | O(n²) |
| Insertion Sort | O(n) | O(n²) | O(n²) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) |
| Timsort | O(n) | O(n log n) | O(n log n) |

---

# 19. Space Complexity in Depth

Space analysis includes:

- Variables
- Data structures
- Recursion stack
- Temporary allocations

---

## Recursive Stack Example

```python
def factorial(n):
    if n == 0:
        return 1

    return n * factorial(n - 1)
```

Time:

```text
O(n)
```

Space:

```text
O(n)
```

Due to recursive call stack.

---

# 20. Best, Average, and Worst Case

Algorithms may behave differently depending on input.

## Example: Linear Search

```python
def search(arr, target):
    for i, value in enumerate(arr):
        if value == target:
            return i
```

### Best Case

Target first element:

```text
O(1)
```

### Worst Case

Target absent or last:

```text
O(n)
```

### Average Case

```text
O(n)
```

---

# 21. Tradeoffs in Real Systems

Big O is not everything.

Real systems care about:

- Cache locality
- Constants
- Memory overhead
- Parallelism
- CPU architecture
- Branch prediction
- IO latency

Sometimes an O(n²) algorithm outperforms O(n log n) for small datasets.

Example:

Insertion sort is frequently used inside faster hybrid sorts.

---

# 22. Big O Misconceptions

## Misconception 1: Big O Measures Exact Speed

False.

It measures growth rate.

---

## Misconception 2: O(1) Is Always Fast

False.

A large constant may still be expensive.

---

## Misconception 3: Worst Case Is Everything

False.

Average case often matters more.

---

## Misconception 4: Nested Loops Always Mean O(n²)

False.

Depends on loop behavior.

Example:

```python
for i in range(n):
    j = i

    while j > 0:
        j //= 2
```

Complexity:

```text
O(n log n)
```

---

# 23. Practical Optimization Strategies

## Choose the Right Data Structure

Using a set instead of a list can reduce lookup:

```text
O(n) → O(1)
```

---

## Avoid Repeated Work

Memoization and caching are powerful.

---

## Use Built-ins

Python built-ins are heavily optimized in C.

Examples:

- `sorted`
- `set`
- `dict`
- `heapq`
- `bisect`

---

## Reduce Nested Iteration

Hash maps frequently replace nested loops.

### Example

Naive Two Sum:

```python
def two_sum(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
```

Complexity:

```text
O(n²)
```

Optimized:

```python
def two_sum(nums, target):
    seen = {}

    for i, num in enumerate(nums):
        diff = target - num

        if diff in seen:
            return [seen[diff], i]

        seen[num] = i
```

Complexity:

```text
O(n)
```

---

# 24. Interview-Style Examples

## Example 1

```python
for i in range(n):
    print(i)
```

Answer:

```text
O(n)
```

---

## Example 2

```python
for i in range(n):
    for j in range(n):
        print(i, j)
```

Answer:

```text
O(n²)
```

---

## Example 3

```python
i = n

while i > 1:
    i //= 2
```

Answer:

```text
O(log n)
```

---

## Example 4

```python
for i in range(n):
    j = 1

    while j < n:
        j *= 2
```

Answer:

```text
O(n log n)
```

---

## Example 5

```python
for i in range(n):
    for j in range(i, n):
        print(i, j)
```

Analysis:

```text
n + (n-1) + (n-2) + ... + 1
```

Result:

```text
O(n²)
```

---

# 25. Advanced Practice Problems

Try analyzing these:

1. Trie insertion/search
2. Segment tree queries
3. Union-find operations
4. DFS recursive space complexity
5. LRU cache operations
6. Heap insertion/removal
7. Matrix multiplication
8. Floyd-Warshall algorithm
9. Bellman-Ford algorithm
10. KMP string matching

---

# 26. Complexity Cheat Sheet

## Common Complexities

| Complexity | Description |
|---|---|
| O(1) | Constant |
| O(log n) | Logarithmic |
| O(n) | Linear |
| O(n log n) | Linearithmic |
| O(n²) | Quadratic |
| O(n³) | Cubic |
| O(2ⁿ) | Exponential |
| O(n!) | Factorial |

---

## Python Collection Cheat Sheet

| Operation | Complexity |
|---|---|
| List append | O(1) amortized |
| List pop() | O(1) |
| List insert(0,x) | O(n) |
| Dict lookup | O(1) average |
| Set lookup | O(1) average |
| Heap push/pop | O(log n) |
| Sort | O(n log n) |

---

# 27. Final Thoughts

Understanding Big O notation is foundational for:

- Algorithms
- System design
- Technical interviews
- High-performance software
- Competitive programming
- Backend engineering
- Machine learning infrastructure

The goal is not merely memorization.

The real skill is learning how to:

1. Recognize patterns
2. Estimate scalability
3. Identify bottlenecks
4. Choose appropriate data structures
5. Optimize intelligently

As you solve more algorithmic problems, complexity analysis becomes intuitive.

The best way to improve is:

- Read code
- Analyze complexity manually
- Benchmark implementations
- Solve many algorithm problems
- Compare alternative approaches

Big O notation is ultimately about understanding how computation scales — and that understanding is one of the most important skills in computer science.
