# Stack Algorithms in Python

## 1. Valid Parentheses

### Question
Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['`, and `']'`, determine if the input string is valid.

A string is valid if:
- Open brackets are closed by the same type of brackets.
- Open brackets are closed in the correct order.
- Every closing bracket has a corresponding opening bracket.

### Example
```python
Input: s = "()[]{}"
Output: True
```

```python
Input: s = "(]"
Output: False
```

---

### Solution
```python
def isValid(s: str) -> bool:
    stack = []

    mapping = {
        ')': '(',
        '}': '{',
        ']': '['
    }

    for char in s:
        if char in mapping:
            top = stack.pop() if stack else '#'

            if mapping[char] != top:
                return False
        else:
            stack.append(char)

    return not stack


# Example usage
print(isValid("()[]{}"))   # True
print(isValid("(]"))       # False
```

### Time Complexity
- **O(n)**

### Space Complexity
- **O(n)**

---

# 2. Min Stack

## Question
Design a stack that supports:
- `push(x)`
- `pop()`
- `top()`
- `getMin()`

All operations should run in constant time.

### Example
```python
minStack.push(-2)
minStack.push(0)
minStack.push(-3)

minStack.getMin()  # -3
minStack.pop()
minStack.top()     # 0
minStack.getMin()  # -2
```

---

## Solution
```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val: int) -> None:
        self.stack.append(val)

        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self) -> None:
        if self.stack[-1] == self.min_stack[-1]:
            self.min_stack.pop()

        self.stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]


# Example usage
minStack = MinStack()

minStack.push(-2)
minStack.push(0)
minStack.push(-3)

print(minStack.getMin())  # -3

minStack.pop()

print(minStack.top())     # 0
print(minStack.getMin())  # -2
```

### Time Complexity
- All operations: **O(1)**

### Space Complexity
- **O(n)**

---

# 3. Evaluate Reverse Polish Notation

## Question
Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are:
- `+`
- `-`
- `*`
- `/`

Each operand may be an integer.

### Example
```python
Input: ["2", "1", "+", "3", "*"]
Output: 9

Explanation:
((2 + 1) * 3) = 9
```

---

## Solution
```python
def evalRPN(tokens):
    stack = []

    for token in tokens:

        if token == "+":
            b = stack.pop()
            a = stack.pop()
            stack.append(a + b)

        elif token == "-":
            b = stack.pop()
            a = stack.pop()
            stack.append(a - b)

        elif token == "*":
            b = stack.pop()
            a = stack.pop()
            stack.append(a * b)

        elif token == "/":
            b = stack.pop()
            a = stack.pop()

            # Truncate toward zero
            stack.append(int(a / b))

        else:
            stack.append(int(token))

    return stack[0]


# Example usage
tokens = ["2", "1", "+", "3", "*"]

print(evalRPN(tokens))  # 9
```

### Time Complexity
- **O(n)**

### Space Complexity
- **O(n)**

---

# 4. Daily Temperatures

## Question
Given an array of daily temperatures, return an array such that:

`answer[i]` is the number of days you have to wait after the `i-th` day to get a warmer temperature.

If there is no future day, put `0`.

### Example
```python
Input: [73,74,75,71,69,72,76,73]

Output: [1,1,4,2,1,1,0,0]
```

---

## Solution
```python
def dailyTemperatures(temperatures):
    result = [0] * len(temperatures)

    stack = []

    for i, temp in enumerate(temperatures):

        while stack and temp > temperatures[stack[-1]]:
            prev_index = stack.pop()
            result[prev_index] = i - prev_index

        stack.append(i)

    return result


# Example usage
temps = [73,74,75,71,69,72,76,73]

print(dailyTemperatures(temps))
# [1,1,4,2,1,1,0,0]
```

### Time Complexity
- **O(n)**

### Space Complexity
- **O(n)**

---

# 5. Largest Rectangle in Histogram

## Question
Given an array of integers representing the heights of histogram bars, return the area of the largest rectangle in the histogram.

### Example
```python
Input: [2,1,5,6,2,3]

Output: 10
```

Explanation:
- The largest rectangle has area `10`
- Formed by heights `5` and `6`

---

## Solution
```python
def largestRectangleArea(heights):
    stack = []
    max_area = 0

    heights.append(0)

    for i, h in enumerate(heights):

        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]

            width = i if not stack else i - stack[-1] - 1

            max_area = max(max_area, height * width)

        stack.append(i)

    heights.pop()

    return max_area


# Example usage
heights = [2,1,5,6,2,3]

print(largestRectangleArea(heights))  # 10
```

### Time Complexity
- **O(n)**

### Space Complexity
- **O(n)**

---

# Summary

| Problem | Main Technique |
|---|---|
| Valid Parentheses | Stack Matching |
| Min Stack | Two Stacks |
| Evaluate Reverse Polish Notation | Stack Evaluation |
| Daily Temperatures | Monotonic Stack |
| Largest Rectangle in Histogram | Monotonic Increasing Stack |
