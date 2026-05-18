# Python Algorithm Practice – Sliding Window & Array Problems

---

# 1. Longest Repeating Character Replacement

## Question

You are given a string `s` and an integer `k`.

You can replace at most `k` characters in the string with any uppercase English letter.

Return the length of the longest substring containing the same letter after performing at most `k` replacements.

### Example

```python
Input: s = "AABABBA", k = 1
Output: 4
```

Explanation:

Replace one `'A'` with `'B'` → `"AABBBBA"`

Longest repeating substring length = `4`

---

## Solution (Sliding Window)

### Idea

Use a sliding window and track:

- Frequency of characters
- Most frequent character in the current window

If:

```python
window_size - most_frequent_character > k
```

then the window is invalid and must shrink.

### Python Solution

```python
from collections import defaultdict

def characterReplacement(s: str, k: int) -> int:
    count = defaultdict(int)

    left = 0
    max_freq = 0
    result = 0

    for right in range(len(s)):
        count[s[right]] += 1

        max_freq = max(max_freq, count[s[right]])

        while (right - left + 1) - max_freq > k:
            count[s[left]] -= 1
            left += 1

        result = max(result, right - left + 1)

    return result
```

### Time Complexity

```python
O(n)
```

### Space Complexity

```python
O(1)
```

---

# 2. Minimum Window Substring

## Question

Given two strings `s` and `t`, return the minimum window substring of `s` such that every character in `t` is included in the window.

If no such substring exists, return an empty string.

### Example

```python
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```

---

## Solution (Sliding Window + Hash Maps)

### Idea

- Use two hash maps:
  - One for required characters
  - One for current window counts
- Expand the right pointer until all characters are included
- Shrink from the left to minimize the window

### Python Solution

```python
from collections import Counter

def minWindow(s: str, t: str) -> str:
    if not t:
        return ""

    target = Counter(t)
    window = {}

    have = 0
    need = len(target)

    left = 0
    result = [-1, -1]
    result_length = float("inf")

    for right in range(len(s)):
        char = s[right]
        window[char] = 1 + window.get(char, 0)

        if char in target and window[char] == target[char]:
            have += 1

        while have == need:
            if (right - left + 1) < result_length:
                result = [left, right]
                result_length = right - left + 1

            window[s[left]] -= 1

            if s[left] in target and window[s[left]] < target[s[left]]:
                have -= 1

            left += 1

    left, right = result

    return s[left:right + 1] if result_length != float("inf") else ""
```

### Time Complexity

```python
O(n)
```

### Space Complexity

```python
O(n)
```

---

# 3. Container With Most Water

## Question

You are given an integer array `height`.

Each element represents a vertical line.

Find two lines that together with the x-axis form a container containing the most water.

Return the maximum amount of water.

### Example

```python
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
```

---

## Solution (Two Pointers)

### Idea

- Start with pointers at both ends
- Calculate area:

```python
width * min(height[left], height[right])
```

- Move the shorter line inward because moving the taller one cannot improve area.

### Python Solution

```python
def maxArea(height):
    left = 0
    right = len(height) - 1

    max_water = 0

    while left < right:
        width = right - left

        current_area = width * min(height[left], height[right])

        max_water = max(max_water, current_area)

        if height[left] < height[right]:
            left += 1
        else:
            right -= 1

    return max_water
```

### Time Complexity

```python
O(n)
```

### Space Complexity

```python
O(1)
```

---

# 4. 3Sum

## Question

Given an integer array `nums`, return all unique triplets:

```python
[a, b, c]
```

such that:

```python
a + b + c == 0
```

### Example

```python
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

---

## Solution (Sorting + Two Pointers)

### Idea

1. Sort the array
2. Fix one number
3. Use two pointers to find pairs

Skip duplicates to avoid repeated triplets.

### Python Solution

```python
def threeSum(nums):
    nums.sort()

    result = []

    for i in range(len(nums)):
        if i > 0 and nums[i] == nums[i - 1]:
            continue

        left = i + 1
        right = len(nums) - 1

        while left < right:
            total = nums[i] + nums[left] + nums[right]

            if total > 0:
                right -= 1

            elif total < 0:
                left += 1

            else:
                result.append([nums[i], nums[left], nums[right]])

                left += 1

                while left < right and nums[left] == nums[left - 1]:
                    left += 1

    return result
```

### Time Complexity

```python
O(n²)
```

### Space Complexity

```python
O(1)
```

---

# 5. 4Sum

## Question

Given an integer array `nums` and an integer `target`, return all unique quadruplets:

```python
[a, b, c, d]
```

such that:

```python
a + b + c + d == target
```

### Example

```python
Input: nums = [1,0,-1,0,-2,2], target = 0
Output:
[
  [-2,-1,1,2],
  [-2,0,0,2],
  [-1,0,0,1]
]
```

---

## Solution (Sorting + Nested Two Pointers)

### Idea

- Sort array
- Fix two numbers
- Use two pointers for remaining pair

### Python Solution

```python
def fourSum(nums, target):
    nums.sort()

    result = []
    n = len(nums)

    for i in range(n):
        if i > 0 and nums[i] == nums[i - 1]:
            continue

        for j in range(i + 1, n):
            if j > i + 1 and nums[j] == nums[j - 1]:
                continue

            left = j + 1
            right = n - 1

            while left < right:
                total = (
                    nums[i]
                    + nums[j]
                    + nums[left]
                    + nums[right]
                )

                if total < target:
                    left += 1

                elif total > target:
                    right -= 1

                else:
                    result.append([
                        nums[i],
                        nums[j],
                        nums[left],
                        nums[right]
                    ])

                    left += 1
                    right -= 1

                    while left < right and nums[left] == nums[left - 1]:
                        left += 1

                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1

    return result
```

### Time Complexity

```python
O(n³)
```

### Space Complexity

```python
O(1)
```

---

# 6. Remove Duplicates from Sorted Array

## Question

Given a sorted integer array `nums`, remove duplicates in-place such that each unique element appears only once.

Return the number of unique elements.

### Example

```python
Input: nums = [1,1,2]
Output: 2
```

Modified array:

```python
[1,2,_]
```

---

## Solution (Two Pointers)

### Idea

- Use one pointer for reading
- Use another pointer for writing unique values

### Python Solution

```python
def removeDuplicates(nums):
    if not nums:
        return 0

    write = 1

    for read in range(1, len(nums)):
        if nums[read] != nums[read - 1]:
            nums[write] = nums[read]
            write += 1

    return write
```

### Time Complexity

```python
O(n)
```

### Space Complexity

```python
O(1)
```

---

# Summary Table

| Problem | Technique | Time Complexity |
|---|---|---|
| Longest Repeating Character Replacement | Sliding Window | O(n) |
| Minimum Window Substring | Sliding Window + Hash Map | O(n) |
| Container With Most Water | Two Pointers | O(n) |
| 3Sum | Sorting + Two Pointers | O(n²) |
| 4Sum | Sorting + Two Pointers | O(n³) |
| Remove Duplicates from Sorted Array | Two Pointers | O(n) |

---
