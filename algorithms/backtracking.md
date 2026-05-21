# Backtracking Algorithms in Python

## 1. Subsets / Power Set

### Question
Given a list of unique integers `nums`, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the subsets in any order.

### Example
```python
Input: nums = [1,2,3]
Output:
[
  [],
  [1],
  [2],
  [3],
  [1,2],
  [1,3],
  [2,3],
  [1,2,3]
]
```

---

### Solution
```python
from typing import List

class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        result = []
        subset = []

        def backtrack(index):
            if index == len(nums):
                result.append(subset[:])
                return

            # Include nums[index]
            subset.append(nums[index])
            backtrack(index + 1)

            # Exclude nums[index]
            subset.pop()
            backtrack(index + 1)

        backtrack(0)
        return result
```

### Explanation
- Each number has two choices:
  - Include it
  - Exclude it
- This creates a decision tree.
- Time Complexity: `O(2^n)`

---

# 2. Permutations

## Question
Given an array `nums` of distinct integers, return all possible permutations.

### Example
```python
Input: nums = [1,2,3]

Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

---

## Solution
```python
from typing import List

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []

        def backtrack(path, remaining):
            if not remaining:
                result.append(path[:])
                return

            for i in range(len(remaining)):
                backtrack(
                    path + [remaining[i]],
                    remaining[:i] + remaining[i+1:]
                )

        backtrack([], nums)
        return result
```

### Explanation
- Choose one number at a time.
- Recursively build permutations using remaining numbers.
- Time Complexity: `O(n!)`

---

# 3. Combination Sum

## Question
Given an array of distinct integers `candidates` and a target integer `target`,
return all unique combinations where the chosen numbers sum to `target`.

You may use the same number unlimited times.

### Example
```python
Input: candidates = [2,3,6,7], target = 7

Output:
[
  [2,2,3],
  [7]
]
```

---

## Solution
```python
from typing import List

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []

        def backtrack(start, path, total):
            if total == target:
                result.append(path[:])
                return

            if total > target:
                return

            for i in range(start, len(candidates)):
                path.append(candidates[i])

                backtrack(
                    i,
                    path,
                    total + candidates[i]
                )

                path.pop()

        backtrack(0, [], 0)
        return result
```

### Explanation
- We try adding each candidate repeatedly.
- If sum exceeds target, stop exploring.
- Use `start` index to avoid duplicates.
- Time Complexity: Exponential in worst case.

---

# 4. N-Queens

## Question
The n-queens puzzle is the problem of placing `n` queens on an `n x n`
chessboard so that no two queens attack each other.

Return all distinct solutions.

### Example
```python
Input: n = 4

Output:
[
 [".Q..",
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",
  "Q...",
  "...Q",
  ".Q.."]
]
```

---

## Solution
```python
from typing import List

class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        result = []
        board = [["."] * n for _ in range(n)]

        cols = set()
        pos_diag = set()   # r + c
        neg_diag = set()   # r - c

        def backtrack(row):
            if row == n:
                copy = ["".join(r) for r in board]
                result.append(copy)
                return

            for col in range(n):
                if (
                    col in cols or
                    (row + col) in pos_diag or
                    (row - col) in neg_diag
                ):
                    continue

                cols.add(col)
                pos_diag.add(row + col)
                neg_diag.add(row - col)
                board[row][col] = "Q"

                backtrack(row + 1)

                cols.remove(col)
                pos_diag.remove(row + col)
                neg_diag.remove(row - col)
                board[row][col] = "."

        backtrack(0)
        return result
```

### Explanation
- Queens attack:
  - Same column
  - Positive diagonal `(row + col)`
  - Negative diagonal `(row - col)`
- Use sets for constant-time conflict checks.
- Time Complexity: Approximately `O(N!)`

---

# 5. Word Search

## Question
Given an `m x n` grid of characters `board` and a string `word`,
return `True` if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells
(horizontal or vertical). The same cell may not be used more than once.

### Example
```python
Input:
board = [
  ["A","B","C","E"],
  ["S","F","C","S"],
  ["A","D","E","E"]
]

word = "ABCCED"

Output: True
```

---

## Solution
```python
from typing import List

class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows = len(board)
        cols = len(board[0])

        visited = set()

        def backtrack(r, c, index):
            if index == len(word):
                return True

            if (
                r < 0 or c < 0 or
                r >= rows or c >= cols or
                word[index] != board[r][c] or
                (r, c) in visited
            ):
                return False

            visited.add((r, c))

            res = (
                backtrack(r + 1, c, index + 1) or
                backtrack(r - 1, c, index + 1) or
                backtrack(r, c + 1, index + 1) or
                backtrack(r, c - 1, index + 1)
            )

            visited.remove((r, c))

            return res

        for r in range(rows):
            for c in range(cols):
                if backtrack(r, c, 0):
                    return True

        return False
```

### Explanation
- Start DFS from every cell.
- Recursively explore adjacent cells.
- Use a `visited` set to avoid reusing cells.
- Backtrack after each recursive call.
- Time Complexity: `O(m * n * 4^L)`
  - `L` = length of the word.

---

# Summary

| Algorithm | Main Technique | Time Complexity |
|---|---|---|
| Subsets | Include / Exclude Backtracking | `O(2^n)` |
| Permutations | Recursive Choice Building | `O(n!)` |
| Combination Sum | Recursive Combination Search | Exponential |
| N-Queens | Constraint Backtracking | `O(N!)` |
| Word Search | DFS + Backtracking | `O(m * n * 4^L)` |
