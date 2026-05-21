# Dynamic Programming Algorithms

---

# 1. Climbing Stairs

## Question

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb:
- 1 step
- 2 steps

Return the number of distinct ways to climb to the top.

### Example

```python
Input: n = 5
Output: 8
```

---

## Solution

This is a classic Dynamic Programming problem.

To reach step `n`, we can:
- come from step `n-1`
- come from step `n-2`

So:

```python
ways(n) = ways(n-1) + ways(n-2)
```

This is identical to the Fibonacci sequence.

---

## Python Solution

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n

        a, b = 1, 2

        for _ in range(3, n + 1):
            a, b = b, a + b

        return b
```

---

## Complexity

- Time: `O(n)`
- Space: `O(1)`

---

# 2. House Robber

## Question

You are a robber planning to rob houses along a street.

Each house has some money.

Adjacent houses cannot be robbed on the same night.

Return the maximum amount of money you can rob.

### Example

```python
Input: nums = [2,7,9,3,1]
Output: 12
```

---

## Solution

For each house:
- rob it → skip previous house
- skip it → keep previous max

Transition:

```python
dp[i] = max(dp[i-1], dp[i-2] + nums[i])
```

---

## Python Solution

```python
class Solution:
    def rob(self, nums):
        rob1, rob2 = 0, 0

        for num in nums:
            newRob = max(rob1 + num, rob2)
            rob1 = rob2
            rob2 = newRob

        return rob2
```

---

## Complexity

- Time: `O(n)`
- Space: `O(1)`

---

# 3. Coin Change

## Question

You are given coins of different denominations and an integer amount.

Return the fewest number of coins needed to make up that amount.

If impossible, return `-1`.

### Example

```python
Input: coins = [1,2,5], amount = 11
Output: 3
```

Explanation:
`11 = 5 + 5 + 1`

---

## Solution

We use Dynamic Programming.

Define:

```python
dp[i] = minimum coins needed for amount i
```

Transition:

```python
dp[i] = min(dp[i], 1 + dp[i - coin])
```

---

## Python Solution

```python
class Solution:
    def coinChange(self, coins, amount):
        dp = [amount + 1] * (amount + 1)
        dp[0] = 0

        for i in range(1, amount + 1):
            for coin in coins:
                if i - coin >= 0:
                    dp[i] = min(dp[i], 1 + dp[i - coin])

        return dp[amount] if dp[amount] != amount + 1 else -1
```

---

## Complexity

- Time: `O(amount * number_of_coins)`
- Space: `O(amount)`

---

# 4. Longest Increasing Subsequence

## Question

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

### Example

```python
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
```

Explanation:
The LIS is `[2,3,7,101]`.

---

## Solution

Dynamic Programming approach:

```python
dp[i] = length of LIS ending at index i
```

For every pair `(j, i)`:

```python
if nums[j] < nums[i]:
    dp[i] = max(dp[i], dp[j] + 1)
```

---

## Python Solution

```python
class Solution:
    def lengthOfLIS(self, nums):
        if not nums:
            return 0

        dp = [1] * len(nums)

        for i in range(len(nums)):
            for j in range(i):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[i], dp[j] + 1)

        return max(dp)
```

---

## Complexity

- Time: `O(n^2)`
- Space: `O(n)`

---

# 5. Longest Common Subsequence

## Question

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

### Example

```python
Input: text1 = "abcde"
Input: text2 = "ace"
Output: 3
```

Explanation:
The LCS is `"ace"`.

---

## Solution

We use a 2D DP table.

Define:

```python
dp[i][j] = LCS length between text1[i:] and text2[j:]
```

Transition:

- If characters match:

```python
1 + dp[i+1][j+1]
```

- Otherwise:

```python
max(dp[i+1][j], dp[i][j+1])
```

---

## Python Solution

```python
class Solution:
    def longestCommonSubsequence(self, text1, text2):
        m, n = len(text1), len(text2)

        dp = [[0] * (n + 1) for _ in range(m + 1)]

        for i in range(m - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                if text1[i] == text2[j]:
                    dp[i][j] = 1 + dp[i + 1][j + 1]
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j + 1])

        return dp[0][0]
```

---

## Complexity

- Time: `O(m * n)`
- Space: `O(m * n)`

---

# 6. Edit Distance

## Question

Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` into `word2`.

Allowed operations:
- Insert
- Delete
- Replace

### Example

```python
Input: word1 = "horse"
Input: word2 = "ros"
Output: 3
```

---

## Solution

We use Dynamic Programming.

Define:

```python
dp[i][j] = minimum operations to convert word1[i:] to word2[j:]
```

Transitions:
- Characters match → move both pointers
- Otherwise:
  - insert
  - delete
  - replace

Take minimum + 1.

---

## Python Solution

```python
class Solution:
    def minDistance(self, word1, word2):
        m, n = len(word1), len(word2)

        dp = [[0] * (n + 1) for _ in range(m + 1)]

        for i in range(m + 1):
            dp[i][n] = m - i

        for j in range(n + 1):
            dp[m][j] = n - j

        for i in range(m - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                if word1[i] == word2[j]:
                    dp[i][j] = dp[i + 1][j + 1]
                else:
                    dp[i][j] = 1 + min(
                        dp[i + 1][j],     # delete
                        dp[i][j + 1],     # insert
                        dp[i + 1][j + 1]  # replace
                    )

        return dp[0][0]
```

---

## Complexity

- Time: `O(m * n)`
- Space: `O(m * n)`

---
