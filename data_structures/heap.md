# Python heapq Module — Priority Queues and Heaps

The built-in Python module heapq provides an implementation of the **heap queue algorithm**, also known as a **priority queue**.

A heap is a specialized binary tree where:
- The **smallest element** is always at the root (heap[0])
- Insertions and removals are efficient:
  - Push: O(log n)
  - Pop smallest: O(log n)
  - Peek smallest: O(1)
Python’s heapq implements a **min-heap**.

# 1. Creating a Heap
```python
import heapq

nums = [5, 3, 8, 1, 2]

heapq.heapify(nums)

print(nums)
```
Output:
```python
[1, 2, 8, 3, 5]
```
The list is rearranged into heap order.

# 2. Basic Heap Operations
`heappush()`

Adds an element while maintaining heap order.

```python
import heapq

heap = []

heapq.heappush(heap, 5)
heapq.heappush(heap, 2)
heapq.heappush(heap, 8)

print(heap)
```
Output:
```python
[2, 5, 8]
```
`heappop()`

Removes and returns the smallest element.
```python
smallest = heapq.heappop(heap)

print(smallest)
print(heap)
```
Output:
```python
2
[5, 8]
```
### Peek Smallest Element
```python
print(heap[0])
```
Output:
```python
5
```
Accessing index `0` is `O(1)`.

# 3. Using Heap as a Priority Queue
A priority queue processes elements by priority.

Example: task scheduling.
```python
import heapq

tasks = []

heapq.heappush(tasks, (2, "Write report"))
heapq.heappush(tasks, (1, "Fix critical bug"))
heapq.heappush(tasks, (3, "Read emails"))

while tasks:
    priority, task = heapq.heappop(tasks)
    print(priority, task)
```
Output:
```python
1 Fix critical bug
2 Write report
3 Read emails
```
The tuple is compared lexicographically:
 - first by priority
 - then by task name if priorities match

# 4. Max Heap in Python
`heapq` only supports min-heaps directly.

To simulate a max-heap, store negative values.
```python
import heapq

nums = [5, 1, 9, 3]

max_heap = []

for n in nums:
    heapq.heappush(max_heap, -n)

print(-heapq.heappop(max_heap))
print(-heapq.heappop(max_heap))
```
Output:
```python
9
5
```
# 5. `heappushpop()`
Pushes then pops in one efficient operation.
```python
import heapq

heap = [2, 4, 6]
heapq.heapify(heap)

result = heapq.heappushpop(heap, 1)

print(result)
print(heap)
```
Output:
```python
1
[2, 4, 6]
```
Useful when maintaining a fixed-size heap.

# 6. `heapreplace()`
Pops smallest first, then pushes new item.
```python
import heapq

heap = [2, 4, 6]
heapq.heapify(heap)

result = heapq.heapreplace(heap, 10)

print(result)
print(heap)
```
Output:
```python
2
[4, 10, 6]
```
Difference from `heappushpop()`:
- `heapreplace()` always removes first
- `heappushpop()` may return the pushed item

# 7. Largest and Smallest Elements
`nsmallest()`
```python
import heapq

nums = [8, 1, 4, 2, 10]

print(heapq.nsmallest(3, nums))
```
Output:
```python
[1, 2, 4]
```
`nlargest()`
```python
print(heapq.nlargest(2, nums))
```
Output:
```python
[10, 8]
```
# 8. Merge Sorted Iterables
`heapq.merge()` efficiently merges sorted lists.`
```python
import heapq

a = [1, 4, 7]
b = [2, 5, 8]
c = [3, 6, 9]

merged = heapq.merge(a, b, c)

print(list(merged))
```
Output:
```python
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```
Efficient for large sorted streams.

# 9. Common Interview Problems Using Heap
### Kth Largest Element

```python
import heapq

nums = [3, 2, 1, 5, 6, 4]
k = 2

result = heapq.nlargest(k, nums)[-1]

print(result)
```
Output:
```python
5
```
### Top K Frequent Elements

```python
from collections import Counter
import heapq

nums = [1,1,1,2,2,3]

freq = Counter(nums)

result = heapq.nlargest(
    2,
    freq.keys(),
    key=freq.get
)

print(result)
```
Output:
```python
[1, 2]
```
# 10. Heap Internals
Python heaps are stored in a list.

For index `i`:
- Left child → `2*i + 1`
- Right child → `2*i + 2`
- Parent → `(i - 1) // 2`

Example heap:
```python
heap = [1, 3, 2, 7, 6, 4]
```
Tree structure:
```
        1
      /   \
     3     2
    / \   /
   7   6 4
```
# 11. Time Complexity
| Operation     | Complexity   |
| ------------- | ------------ |
| `heapify()`   | `O(n)`       |
| `heappush()`  | `O(log n)`   |
| `heappop()`   | `O(log n)`   |
| Peek smallest | `O(1)`       |
| `nlargest()`  | `O(n log k)` |
| `nsmallest()` | `O(n log k)` |

# 12. When to Use `heapq`
Use heaps when you need:
- Priority queues
- Efficient min/max retrieval
- Top-K problems
- Scheduling systems
- Dijkstra’s algorithm
- A* pathfinding
- Streaming data processing

# 13. Real-World Example — Task Scheduler

```python
import heapq

jobs = []

heapq.heappush(jobs, (3, "Low priority"))
heapq.heappush(jobs, (1, "Urgent"))
heapq.heappush(jobs, (2, "Medium"))

while jobs:
    priority, job = heapq.heappop(jobs)
    print(f"Processing: {job}")
```
Output:
```python
Processing: Urgent
Processing: Medium
Processing: Low priority
```
# 14. Common Pitfalls
### 1. Heap Is NOT Fully Sorted
```python
heap = [1, 3, 2, 7, 6]
```
This is valid because only parent-child order matters.
### 2. Tuples Must Be Comparable
This fails:
```python
heapq.heappush(heap, (1, object()))
```
if two priorities are equal.

Solution:
```python
counter = 0
heapq.heappush(heap, (priority, counter, item))
```
# 15. Summary
| Function        | Purpose                |
| --------------- | ---------------------- |
| `heapify()`     | Convert list to heap   |
| `heappush()`    | Insert item            |
| `heappop()`     | Remove smallest        |
| `heappushpop()` | Push then pop          |
| `heapreplace()` | Pop then push          |
| `nlargest()`    | Largest K elements     |
| `nsmallest()`   | Smallest K elements    |
| `merge()`       | Merge sorted iterables |

