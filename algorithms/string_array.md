# Python String and Array Algorithm Practice Problems & Solutions

---

# 1. Two Sum

## Question

Given an array of integers `nums` and an integer `target`, return the **indices** of the two numbers such that they add up to `target`.

### Example

```python
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
```

---

## Solution

### Approach

Use a hash map (dictionary) to store previously seen numbers and their indices.

- For each number:
  - Calculate the complement:

```python
complement = target - nums[i]
```

  - Check if the complement already exists in the dictionary.
  - If yes, return the indices.

### Time Complexity

- **O(n)**

### Python Code

```python
def two_sum(nums, target):
    seen = {}

    for i, num in enumerate(nums):
        complement = target - num

        if complement in seen:
            return [seen[complement], i]

        seen[num] = i

    return []
```

---

# 2. Best Time to Buy and Sell Stock

## Question

You are given an array `prices` where `prices[i]` is the price of a stock on day `i`.

Find the maximum profit you can achieve by buying one stock and selling one stock later.

### Example

```python
Input: prices = [7,1,5,3,6,4]
Output: 5
```

---

## Solution

### Approach

Track:

- The minimum price seen so far
- The maximum profit possible

### Time Complexity

- **O(n)**

### Python Code

```python
def max_profit(prices):
    min_price = float('inf')
    max_profit = 0

    for price in prices:
        min_price = min(min_price, price)

        profit = price - min_price
        max_profit = max(max_profit, profit)

    return max_profit
```

---

# 3. Contains Duplicate

## Question

Given an integer array `nums`, return `True` if any value appears at least twice.

### Example

```python
Input: nums = [1,2,3,1]
Output: True
```

---

## Solution

### Approach

Use a set to track seen numbers.

If a number already exists in the set, return `True`.

### Time Complexity

- **O(n)**

### Python Code

```python
def contains_duplicate(nums):
    seen = set()

    for num in nums:
        if num in seen:
            return True

        seen.add(num)

    return False
```

---

# 4. Product of Array Except Self

## Question

Given an integer array `nums`, return an array `answer` such that:

```python
answer[i] = product of all elements except nums[i]
```

Do not use division.

### Example

```python
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

---

## Solution

### Approach

Use:

- Prefix products
- Suffix products

### Time Complexity

- **O(n)**

### Python Code

```python
def product_except_self(nums):
    n = len(nums)
    result = [1] * n

    prefix = 1
    for i in range(n):
        result[i] = prefix
        prefix *= nums[i]

    suffix = 1
    for i in range(n - 1, -1, -1):
        result[i] *= suffix
        suffix *= nums[i]

    return result
```

---

# 5. Maximum Subarray (Kadane’s Algorithm)

## Question

Find the contiguous subarray with the largest sum.

### Example

```python
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
```

Subarray:

```python
[4, -1, 2, 1]
```

---

## Solution

### Approach

Kadane’s Algorithm:

At each position:

```python
current_sum = max(num, current_sum + num)
```

Track the maximum sum seen so far.

### Time Complexity

- **O(n)**

### Python Code

```python
def max_subarray(nums):
    current_sum = nums[0]
    max_sum = nums[0]

    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)

    return max_sum
```

---

# 6. Merge Intervals

## Question

Given an array of intervals where:

```python
intervals[i] = [start, end]
```

Merge all overlapping intervals.

### Example

```python
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
```

---

## Solution

### Approach

1. Sort intervals by start time
2. Compare with the last merged interval
3. Merge if overlapping

### Time Complexity

- **O(n log n)** (sorting)

### Python Code

```python
def merge_intervals(intervals):
    intervals.sort(key=lambda x: x[0])

    merged = [intervals[0]]

    for current in intervals[1:]:
        last = merged[-1]

        if current[0] <= last[1]:
            last[1] = max(last[1], current[1])
        else:
            merged.append(current)

    return merged
```

---

# 7. Longest Substring Without Repeating Characters

## Question

Given a string `s`, find the length of the longest substring without repeating characters.

### Example

```python
Input: "abcabcbb"
Output: 3
```

Substring:

```python
"abc"
```

---

## Solution

### Approach

Use the Sliding Window technique.

- Expand the window
- Remove duplicates when needed

### Time Complexity

- **O(n)**

### Python Code

```python
def length_of_longest_substring(s):
    char_set = set()

    left = 0
    max_length = 0

    for right in range(len(s)):
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1

        char_set.add(s[right])
        max_length = max(max_length, right - left + 1)

    return max_length
```

---

# 8. Valid Anagram

## Question

Given two strings `s` and `t`, return `True` if `t` is an anagram of `s`.

### Example

```python
Input: s = "anagram", t = "nagaram"
Output: True
```

---

## Solution

### Approach

Count character frequencies using dictionaries.

### Time Complexity

- **O(n)**

### Python Code

```python
from collections import Counter

def is_anagram(s, t):
    return Counter(s) == Counter(t)
```

---

# 9. Group Anagrams

## Question

Given an array of strings `strs`, group the anagrams together.

### Example

```python
Input: ["eat","tea","tan","ate","nat","bat"]

Output:
[
    ["eat","tea","ate"],
    ["tan","nat"],
    ["bat"]
]
```

---

## Solution

### Approach

Use sorted strings as dictionary keys.

Example:

```python
"eat" -> "aet"
"tea" -> "aet"
```

### Time Complexity

- **O(n * k log k)**

Where:

- `n` = number of strings
- `k` = maximum string length

### Python Code

```python
from collections import defaultdict

def group_anagrams(strs):
    groups = defaultdict(list)

    for word in strs:
        key = ''.join(sorted(word))
        groups[key].append(word)

    return list(groups.values())
```

---

# Summary Table

| Algorithm | Main Technique | Time Complexity |
|---|---|---|
| Two Sum | Hash Map | O(n) |
| Best Time to Buy and Sell Stock | Greedy | O(n) |
| Contains Duplicate | Set | O(n) |
| Product of Array Except Self | Prefix/Suffix Products | O(n) |
| Maximum Subarray | Kadane’s Algorithm | O(n) |
| Merge Intervals | Sorting + Merging | O(n log n) |
| Longest Substring Without Repeating Characters | Sliding Window | O(n) |
| Valid Anagram | Hash Map / Counter | O(n) |
| Group Anagrams | Hash Map + Sorting | O(n * k log k) |
