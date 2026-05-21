# Sorting Algorithms in Python

## Introduction

Sorting algorithms are fundamental techniques in computer science used to arrange data in a particular order, usually ascending or descending.

Efficient sorting is important because many other algorithms become faster or easier after data is sorted.

In this tutorial, you will learn:

- What sorting algorithms are
- Time and space complexity
- Stable vs unstable sorting
- Comparison-based sorting
- Python implementations
- When to use each algorithm

---

# Table of Contents

1. Bubble Sort
2. Selection Sort
3. Insertion Sort
4. Merge Sort
5. Quick Sort
6. Heap Sort
7. Counting Sort
8. Radix Sort
9. Bucket Sort
10. Python Built-in Sorting
11. Complexity Comparison Table
12. Best Practices
13. Practice Problems

---

# 1. Bubble Sort

## Question

Implement Bubble Sort in Python.

Bubble Sort repeatedly swaps adjacent elements if they are in the wrong order.

---

## Explanation

The algorithm compares neighboring elements and pushes larger elements toward the end of the list.

### Steps

1. Compare adjacent elements
2. Swap if left element is larger
3. Repeat until array becomes sorted

---

## Python Solution

```python

def bubble_sort(arr):
    n = len(arr)

    for i in range(n):
        swapped = False

        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True

        if not swapped:
            break

    return arr


nums = [5, 1, 4, 2, 8]
print(bubble_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(n) |
| Average | O(n²) |
| Worst | O(n²) |

Space Complexity: O(1)

---

# 2. Selection Sort

## Question

Implement Selection Sort in Python.

Selection Sort repeatedly selects the minimum element and places it in the correct position.

---

## Explanation

The algorithm divides the array into:

- Sorted portion
- Unsorted portion

It repeatedly selects the smallest value from the unsorted section.

---

## Python Solution

```python

def selection_sort(arr):
    n = len(arr)

    for i in range(n):
        min_index = i

        for j in range(i + 1, n):
            if arr[j] < arr[min_index]:
                min_index = j

        arr[i], arr[min_index] = arr[min_index], arr[i]

    return arr


nums = [64, 25, 12, 22, 11]
print(selection_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(n²) |
| Average | O(n²) |
| Worst | O(n²) |

Space Complexity: O(1)

---

# 3. Insertion Sort

## Question

Implement Insertion Sort in Python.

Insertion Sort builds the sorted array one element at a time.

---

## Explanation

Each new element is inserted into its correct position among previously sorted elements.

---

## Python Solution

```python

def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        arr[j + 1] = key

    return arr


nums = [12, 11, 13, 5, 6]
print(insertion_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(n) |
| Average | O(n²) |
| Worst | O(n²) |

Space Complexity: O(1)

---

# 4. Merge Sort

## Question

Implement Merge Sort in Python.

Merge Sort uses the divide-and-conquer approach.

---

## Explanation

### Steps

1. Divide the array into halves
2. Recursively sort both halves
3. Merge sorted halves

---

## Python Solution

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


nums = [38, 27, 43, 3, 9, 82, 10]
print(merge_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n log n) |

Space Complexity: O(n)

---

# 5. Quick Sort

## Question

Implement Quick Sort in Python.

Quick Sort selects a pivot and partitions the array around it.

---

## Explanation

### Steps

1. Choose pivot
2. Partition smaller/larger elements
3. Recursively sort partitions

---

## Python Solution

```python

def quick_sort(arr):
    if len(arr) <= 1:
        return arr

    pivot = arr[len(arr) // 2]

    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]

    return quick_sort(left) + middle + quick_sort(right)


nums = [10, 7, 8, 9, 1, 5]
print(quick_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n²) |

Space Complexity: O(log n)

---

# 6. Heap Sort

## Question

Implement Heap Sort in Python.

Heap Sort uses a binary heap data structure.

---

## Explanation

### Steps

1. Build max heap
2. Swap root with last element
3. Heapify remaining heap

---

## Python Solution

```python

def heapify(arr, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2

    if left < n and arr[left] > arr[largest]:
        largest = left

    if right < n and arr[right] > arr[largest]:
        largest = right

    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)


def heap_sort(arr):
    n = len(arr)

    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)

    return arr


nums = [12, 11, 13, 5, 6, 7]
print(heap_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n log n) |

Space Complexity: O(1)

---

# 7. Counting Sort

## Question

Implement Counting Sort in Python.

Counting Sort works for integers within a limited range.

---

## Explanation

Instead of comparisons, it counts occurrences of each value.

---

## Python Solution

```python

def counting_sort(arr):
    max_val = max(arr)
    count = [0] * (max_val + 1)

    for num in arr:
        count[num] += 1

    result = []

    for i, freq in enumerate(count):
        result.extend([i] * freq)

    return result


nums = [4, 2, 2, 8, 3, 3, 1]
print(counting_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(n + k) |
| Average | O(n + k) |
| Worst | O(n + k) |

Space Complexity: O(k)

Where `k` is the range of numbers.

---

# 8. Radix Sort

## Question

Implement Radix Sort in Python.

Radix Sort sorts numbers digit by digit.

---

## Explanation

The algorithm processes digits from least significant to most significant.

---

## Python Solution

```python

def counting_sort_exp(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10

    for num in arr:
        index = (num // exp) % 10
        count[index] += 1

    for i in range(1, 10):
        count[i] += count[i - 1]

    for i in range(n - 1, -1, -1):
        index = (arr[i] // exp) % 10
        output[count[index] - 1] = arr[i]
        count[index] -= 1

    for i in range(n):
        arr[i] = output[i]


def radix_sort(arr):
    max_num = max(arr)
    exp = 1

    while max_num // exp > 0:
        counting_sort_exp(arr, exp)
        exp *= 10

    return arr


nums = [170, 45, 75, 90, 802, 24, 2, 66]
print(radix_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(nk) |
| Average | O(nk) |
| Worst | O(nk) |

Space Complexity: O(n + k)

---

# 9. Bucket Sort

## Question

Implement Bucket Sort in Python.

Bucket Sort distributes elements into buckets and sorts each bucket individually.

---

## Explanation

Best used when data is uniformly distributed.

---

## Python Solution

```python

def bucket_sort(arr):
    bucket_count = len(arr)
    buckets = [[] for _ in range(bucket_count)]

    for num in arr:
        index = int(num * bucket_count)
        buckets[index].append(num)

    result = []

    for bucket in buckets:
        result.extend(sorted(bucket))

    return result


nums = [0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68]
print(bucket_sort(nums))
```

---

## Complexity

| Case | Time |
|---|---|
| Best | O(n + k) |
| Average | O(n + k) |
| Worst | O(n²) |

Space Complexity: O(n)

---

# 10. Python Built-in Sorting

Python provides highly optimized sorting functions.

---

## sorted()

Returns a new sorted list.

```python
nums = [5, 2, 9, 1]
print(sorted(nums))
```

---

## list.sort()

Sorts the list in place.

```python
nums = [5, 2, 9, 1]
nums.sort()
print(nums)
```

---

## Sorting with Key

```python
words = ["apple", "banana", "kiwi"]
print(sorted(words, key=len))
```

---

## Reverse Sorting

```python
nums = [1, 5, 3, 2]
print(sorted(nums, reverse=True))
```

---

## Timsort

Python internally uses Timsort.

Properties:

- Stable
- Hybrid of Merge Sort and Insertion Sort
- Very efficient on partially sorted data

Complexity:

| Case | Time |
|---|---|
| Best | O(n) |
| Average | O(n log n) |
| Worst | O(n log n) |

---

# 11. Complexity Comparison Table

| Algorithm | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | Yes |
| Radix Sort | O(nk) | O(nk) | O(nk) | O(n+k) | Yes |
| Bucket Sort | O(n+k) | O(n+k) | O(n²) | O(n) | Depends |

---

# 12. Best Practices

## Use Python Built-in Sort When Possible

Python's built-in sorting is highly optimized.

---

## Choose Algorithms Based on Input

- Small arrays → Insertion Sort
- Large datasets → Merge Sort or Quick Sort
- Limited integer ranges → Counting Sort
- Nearly sorted data → Timsort

---

## Understand Stability

Stable sorting preserves the order of equal elements.

This matters for:

- Multi-level sorting
- Database records
- UI tables

---

# 13. Practice Problems

Try solving these problems:

1. Sort Colors
2. Merge Intervals
3. Kth Largest Element
4. Top K Frequent Elements
5. Sort Characters by Frequency
6. Relative Sort Array
7. Largest Number
8. Wiggle Sort
9. Meeting Rooms
10. Merge Sorted Arrays
