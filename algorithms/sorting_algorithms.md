# Sorting Algorithms

## 1. Sort Colors

### Question
Given an array `nums` with `n` objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent.

Use the integers:
- `0` → red
- `1` → white
- `2` → blue

You must solve this problem without using the library sort function.

### Solution
```python
from typing import List

class Solution:
    def sortColors(self, nums: List[int]) -> None:
        left = 0
        current = 0
        right = len(nums) - 1

        while current <= right:
            if nums[current] == 0:
                nums[left], nums[current] = nums[current], nums[left]
                left += 1
                current += 1

            elif nums[current] == 2:
                nums[right], nums[current] = nums[current], nums[right]
                right -= 1

            else:
                current += 1
```

### Explanation
This uses the Dutch National Flag Algorithm:
- `left` tracks position for `0`
- `right` tracks position for `2`
- `current` scans the array

Time Complexity: `O(n)`  
Space Complexity: `O(1)`

---

# 2. Merge Intervals

### Question
Given an array of intervals where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals and return the resulting intervals.

### Solution
```python
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x: x[0])

        merged = []

        for interval in intervals:
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)
            else:
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

### Explanation
1. Sort intervals by starting value
2. Compare each interval with the last merged interval
3. Merge overlapping intervals

Time Complexity: `O(n log n)`  
Space Complexity: `O(n)`

---

# 3. Kth Largest Element

### Question
Given an integer array `nums` and an integer `k`, return the `kth` largest element in the array.

### Solution
```python
import heapq
from typing import List

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        return heapq.nlargest(k, nums)[-1]
```

### Alternative Heap Solution
```python
import heapq
from typing import List

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        min_heap = []

        for num in nums:
            heapq.heappush(min_heap, num)

            if len(min_heap) > k:
                heapq.heappop(min_heap)

        return min_heap[0]
```

### Explanation
A min-heap of size `k` keeps track of the `k` largest elements.

Time Complexity: `O(n log k)`  
Space Complexity: `O(k)`

---

# 4. Top K Frequent Elements

### Question
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

### Solution
```python
from collections import Counter
from typing import List

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)

        return [num for num, freq in count.most_common(k)]
```

### Alternative Bucket Sort Solution
```python
from collections import Counter
from typing import List

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)

        freq = [[] for _ in range(len(nums) + 1)]

        for num, c in count.items():
            freq[c].append(num)

        result = []

        for i in range(len(freq) - 1, 0, -1):
            for num in freq[i]:
                result.append(num)

                if len(result) == k:
                    return result
```

### Explanation
- Count frequency of each element
- Use bucket sort or heap to retrieve the top `k`

Time Complexity: `O(n)` using bucket sort  
Space Complexity: `O(n)`

---

# 5. Sort Characters by Frequency

### Question
Given a string `s`, sort it in decreasing order based on the frequency of characters.

### Solution
```python
from collections import Counter

class Solution:
    def frequencySort(self, s: str) -> str:
        count = Counter(s)

        sorted_chars = sorted(count.items(), key=lambda x: x[1], reverse=True)

        result = []

        for char, freq in sorted_chars:
            result.append(char * freq)

        return "".join(result)
```

### Alternative Using most_common()
```python
from collections import Counter

class Solution:
    def frequencySort(self, s: str) -> str:
        count = Counter(s)

        return "".join(char * freq for char, freq in count.most_common())
```

### Explanation
- Count occurrences of each character
- Sort characters by descending frequency
- Rebuild the string

Time Complexity: `O(n log n)`  
Space Complexity: `O(n)`
