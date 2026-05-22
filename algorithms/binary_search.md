# Binary Search Algorithms

## 1. Binary Search (Classic)

### Question
Given a sorted array of integers `nums` and an integer `target`, return the index of `target` if it exists in the array. Otherwise, return `-1`.

You must write an algorithm with **O(log n)** runtime complexity.

### Example
```python
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
```

---

## Solution

### Idea
Binary Search works only on **sorted arrays**.

At each step:
1. Find the middle element.
2. Compare it with the target.
3. If the target is smaller, search the left half.
4. If the target is larger, search the right half.

This cuts the search space in half every iteration.

---

### Python Solution
```python
def binary_search(nums, target):
    left = 0
    right = len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        elif nums[mid] < target:
            left = mid + 1

        else:
            right = mid - 1

    return -1
```

---

### Step-by-Step Example
```python
nums = [-1,0,3,5,9,12]
target = 9
```

| Left | Right | Mid | nums[mid] | Action |
|---|---|---|---|---|
| 0 | 5 | 2 | 3 | Search right |
| 3 | 5 | 4 | 9 | Found target |

---

### Time Complexity
- **O(log n)**

### Space Complexity
- **O(1)**

---

# 2. Search in Rotated Sorted Array

### Question
There is an integer array `nums` sorted in ascending order, but rotated at some pivot.

Search for `target`. If found, return its index. Otherwise return `-1`.

You must achieve **O(log n)** runtime complexity.

### Example
```python
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

---

## Solution

### Key Observation
Even though the array is rotated:
- One half is always sorted.

We can determine:
- Whether the left half is sorted
- Or the right half is sorted

Then decide which side contains the target.

---

### Python Solution
```python
def search_rotated(nums, target):
    left = 0
    right = len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        # Left half is sorted
        if nums[left] <= nums[mid]:

            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1

        # Right half is sorted
        else:

            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1

    return -1
```

---

### Time Complexity
- **O(log n)**

### Space Complexity
- **O(1)**

---

# 3. Find Minimum in Rotated Sorted Array

### Question
Suppose an array sorted in ascending order is rotated between `1` and `n` times.

Find the minimum element.

You must write an algorithm with **O(log n)** runtime complexity.

### Example
```python
Input: nums = [3,4,5,1,2]
Output: 1
```

---

## Solution

### Key Insight
In a rotated sorted array:
- The minimum value is where the rotation happened.
- Compare `nums[mid]` with `nums[right]`.

Rules:
- If `nums[mid] > nums[right]`
  → minimum is in the right half.
- Else
  → minimum is in the left half including `mid`.

---

### Python Solution
```python
def find_min(nums):
    left = 0
    right = len(nums) - 1

    while left < right:
        mid = (left + right) // 2

        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            right = mid

    return nums[left]
```

---

### Time Complexity
- **O(log n)**

### Space Complexity
- **O(1)**

---

# 4. Median of Two Sorted Arrays (Hard)

### Question
Given two sorted arrays `nums1` and `nums2` of size `m` and `n`, return the median of the two sorted arrays.

The overall runtime complexity must be **O(log (m+n))**.

### Example
```python
Input: nums1 = [1,3], nums2 = [2]
Output: 2.0
```

---

## Solution

### Main Idea
We partition both arrays such that:
- Left partition contains half the elements
- Every element on the left is smaller than every element on the right

Binary search is performed on the smaller array.

---

### Python Solution
```python
def findMedianSortedArrays(nums1, nums2):

    # Ensure nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    x = len(nums1)
    y = len(nums2)

    left = 0
    right = x

    while left <= right:

        partitionX = (left + right) // 2
        partitionY = (x + y + 1) // 2 - partitionX

        maxLeftX = float("-inf") if partitionX == 0 else nums1[partitionX - 1]
        minRightX = float("inf") if partitionX == x else nums1[partitionX]

        maxLeftY = float("-inf") if partitionY == 0 else nums2[partitionY - 1]
        minRightY = float("inf") if partitionY == y else nums2[partitionY]

        # Correct partition
        if maxLeftX <= minRightY and maxLeftY <= minRightX:

            # Even total length
            if (x + y) % 2 == 0:
                return (
                    max(maxLeftX, maxLeftY) +
                    min(minRightX, minRightY)
                ) / 2

            # Odd total length
            else:
                return max(maxLeftX, maxLeftY)

        elif maxLeftX > minRightY:
            right = partitionX - 1

        else:
            left = partitionX + 1
```

---

### Time Complexity
- **O(log(min(m, n)))**

### Space Complexity
- **O(1)**

---

# Summary Table

| Problem | Technique | Time Complexity |
|---|---|---|
| Binary Search | Standard Binary Search | O(log n) |
| Search in Rotated Array | Modified Binary Search | O(log n) |
| Find Minimum in Rotated Array | Binary Search | O(log n) |
| Median of Two Sorted Arrays | Partition Binary Search | O(log(min(m,n))) |
