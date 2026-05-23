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



