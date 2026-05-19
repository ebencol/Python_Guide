# How to Approach Algorithm Problems in Python

## Introduction

Algorithm problems can feel difficult because the hardest part is often not writing Python code — it is figuring out *how to think* about the problem.

Many beginners try to immediately code a solution. Strong problem solvers do the opposite:

1. Understand the problem deeply
2. Find patterns
3. Break the problem into smaller pieces
4. Choose the right data structure or technique
5. Then write code

This tutorial teaches a repeatable process you can use for almost any coding interview or algorithm challenge.

---

# The Core Mindset

When solving algorithms:

- Do **not** search for the perfect solution immediately
- Start with the simplest possible approach
- Improve step-by-step
- Learn common patterns
- Practice recognizing similarities between problems

The goal is not memorization.

The goal is:

> “When I see a new problem, I know how to think.”

---

# The 7-Step Problem Solving Framework

---

# Step 1 — Understand the Problem Completely

Before coding anything:

## Ask Yourself

- What are the inputs?
- What is the output?
- Are there constraints?
- Is the input sorted?
- Can there be duplicates?
- Are negative numbers allowed?
- What should happen in edge cases?

---

## Example Problem

### Two Sum

Given an array of integers `nums` and an integer `target`,
return indices of two numbers such that they add up to `target`.

---

## Clarify with Examples

```python
nums = [2, 7, 11, 15]
target = 9

Output: [0, 1]
```

Because:

```python
2 + 7 = 9
```

---

# Step 2 — Work Through Examples by Hand

This is the most important step.

Do NOT jump into coding.

Take paper and simulate the solution manually.

---

## Example

```python
nums = [2, 7, 11, 15]
target = 9
```

Ask:

- What do I look for?
- How would I solve this as a human?

You would think:

```text
2 → need 7
7 → need 2
```

That insight leads directly to a hash map solution.

---

# Step 3 — Start With the Brute Force Solution

Always begin with the simplest approach.

Even if it is slow.

This helps you:

- Understand the problem structure
- Build confidence
- Discover optimization opportunities

---

## Example: Brute Force Two Sum

```python
def two_sum(nums, target):
    n = len(nums)

    for i in range(n):
        for j in range(i + 1, n):

            if nums[i] + nums[j] == target:
                return [i, j]
```

---

## Time Complexity

```text
O(n²)
```

Because we check every pair.

---

# Step 4 — Ask Optimization Questions

Now ask:

## Can I avoid repeated work?

## Can I store information?

## Can I use a better data structure?

---

For Two Sum:

Instead of checking all pairs repeatedly:

```text
Store previously seen numbers
```

This leads to a hash map.

---

# Step 5 — Recognize Patterns

Most algorithm problems belong to recurring categories.

Learning patterns is the secret to getting better.

---

# Common Algorithm Patterns

| Pattern | Common Use |
|---|---|
| Hash Map | Fast lookup |
| Two Pointers | Sorted arrays |
| Sliding Window | Substrings/subarrays |
| Stack | Nested structures |
| Queue/BFS | Shortest paths |
| DFS/Recursion | Trees/graphs |
| Binary Search | Sorted search space |
| Dynamic Programming | Overlapping subproblems |
| Greedy | Local optimal decisions |
| Heap/Priority Queue | Top K problems |

---

# Pattern Example: Sliding Window

Problem:

> Longest substring without repeating characters

Clues:

- Contiguous substring
- Need optimal range
- Repeated recalculation is expensive

This suggests:

```text
Sliding Window
```

---

# Step 6 — Write Pseudocode First

Before Python code:

Write logic in plain English.

---

## Example

### Longest Substring Without Repeating Characters

Pseudocode:

```text
Create set for characters
Left pointer = 0

Move right pointer through string

If duplicate found:
    remove left characters until duplicate gone

Update maximum length
```

Now coding becomes easy.

---

# Step 7 — Code Carefully

Translate logic slowly.

Focus on:

- Variable names
- Boundary conditions
- Edge cases
- Pointer movement

---

# Example: Sliding Window Solution

```python
def length_of_longest_substring(s):
    seen = set()

    left = 0
    max_length = 0

    for right in range(len(s)):

        while s[right] in seen:
            seen.remove(s[left])
            left += 1

        seen.add(s[right])

        max_length = max(max_length, right - left + 1)

    return max_length
```

---

# The Most Important Skill: Pattern Recognition

Strong algorithm solvers do not invent solutions from scratch every time.

They recognize:

```text
"I've seen this structure before."
```

---

# Example Pattern Clues

| Clue in Problem | Likely Technique |
|---|---|
| “pair sums” | Hash map |
| “sorted array” | Two pointers / binary search |
| “longest substring” | Sliding window |
| “top k frequent” | Heap |
| “tree traversal” | DFS/BFS |
| “all combinations” | Backtracking |
| “minimum steps/path” | BFS |
| “overlapping calculations” | Dynamic programming |

---

# How to Build Problem Solving Ability

---

# 1. Learn Data Structures Well

You must deeply understand:

- Lists
- Dictionaries
- Sets
- Stacks
- Queues
- Heaps
- Trees
- Graphs

---

# 2. Learn Time Complexity

Always ask:

```text
How many operations does this perform?
```

---

# Big-O Cheat Sheet

| Complexity | Performance |
|---|---|
| O(1) | Excellent |
| O(log n) | Very fast |
| O(n) | Good |
| O(n log n) | Acceptable |
| O(n²) | Slow |
| O(2ⁿ) | Very slow |

---

# 3. Solve Problems by Category

Bad approach:

```text
Random problems every day
```

Better approach:

```text
Week 1 → Hash maps
Week 2 → Sliding window
Week 3 → Trees
Week 4 → Dynamic programming
```

Pattern repetition builds intuition.

---

# 4. Re-solve Problems

Do not solve once and move on.

Re-solve:

- next day
- next week
- next month

This creates long-term memory.

---

# 5. Read Other Solutions

Especially after struggling.

Ask:

- Why is this solution faster?
- What pattern did I miss?
- What insight unlocked the problem?

---

# A Complete Thinking Example

---

# Problem

Find the maximum sum of any subarray of size `k`.

---

## Step 1 — Understand

Input:

```python
nums = [1,2,3,4,5]
k = 2
```

Output:

```python
9
```

Because:

```python
4 + 5 = 9
```

---

# Step 2 — Brute Force

Check every subarray.

```python
def max_sum(nums, k):
    best = 0

    for i in range(len(nums) - k + 1):
        current = sum(nums[i:i+k])
        best = max(best, current)

    return best
```

---

## Complexity

```text
O(n * k)
```

---

# Step 3 — Optimize

Observation:

When window moves:

```text
Remove left value
Add right value
```

This is:

```text
Sliding Window
```

---

# Step 4 — Optimized Solution

```python
def max_sum(nums, k):

    window_sum = sum(nums[:k])
    best = window_sum

    for i in range(k, len(nums)):

        window_sum += nums[i]
        window_sum -= nums[i - k]

        best = max(best, window_sum)

    return best
```

---

## Complexity

```text
O(n)
```

---

# How to Get Unstuck

When completely stuck:

---

# 1. Draw It

Visualize:

- arrays
- pointers
- trees
- graphs

---

# 2. Use Small Inputs

Example:

```python
[1, 2, 3]
```

instead of huge examples.

---

# 3. Ask What Repeats

Repeated work often indicates optimization opportunities.

---

# 4. Simplify the Problem

Ask:

```text
What if input size was 1?
What if array was sorted?
What if duplicates didn’t exist?
```

---

# 5. Think About Data Structures

Ask:

```text
Do I need:
- fast lookup?
- ordering?
- minimum/maximum?
- uniqueness?
```

---

# Common Beginner Mistakes

---

# 1. Coding Too Early

Thinking happens before typing.

---

# 2. Ignoring Edge Cases

Examples:

```python
[]
[1]
duplicates
negative numbers
```

---

# 3. Memorizing Blindly

Memorization without understanding fails quickly.

---

# 4. Never Reviewing Problems

Real learning comes from repetition.

---

# 5. Avoiding Hard Problems

Struggle is part of learning.

---

# Recommended Learning Order

---

# Beginner

- Arrays
- Hash maps
- Strings
- Two pointers
- Sliding window
- Stacks
- Queues

---

# Intermediate

- Linked lists
- Binary search
- Trees
- Heaps
- Recursion
- Backtracking

---

# Advanced

- Graphs
- Dynamic programming
- Tries
- Union Find
- Segment Trees

---

# A Powerful Mental Model

When solving problems:

```text
1. Understand
2. Brute force
3. Find inefficiency
4. Optimize
5. Recognize pattern
6. Implement carefully
7. Test edge cases
```

This process works for nearly every algorithm problem.

---

# Practice Strategy

A strong daily routine:

---

## 1 Problem Easy

Focus on speed and confidence.

---

## 1 Problem Medium

Focus on pattern recognition.

---

## Review Old Problems

This is where real improvement happens.

---

# Final Advice

Algorithm solving is not talent.

It is:

- Pattern recognition
- Structured thinking
- Repetition
- Experience

At first, solutions feel invisible.

After enough practice:

```text
You stop seeing random problems
and start seeing familiar patterns.
```

That is when algorithm solving becomes enjoyable.
