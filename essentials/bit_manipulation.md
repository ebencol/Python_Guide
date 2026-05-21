# Advanced Bit Manipulation in Python

# Table of Contents

1. Introduction
2. Binary Number Fundamentals
3. Bitwise Operators in Python
4. Understanding Signed Integers
5. Common Bit Manipulation Techniques
6. Advanced Bit Tricks
7. Bit Masks
8. XOR Applications
9. Working with Sets Using Bits
10. Bit Manipulation in Algorithms
11. Dynamic Programming with Bitmasking
12. Competitive Programming Techniques
13. Real-World Applications
14. Time Complexity Analysis
15. Common Pitfalls
16. Practice Problems
17. Conclusion

---

# 1. Introduction

Bit manipulation is the process of directly working with binary representations of numbers using bitwise operators.

Because computers store data in binary, bit manipulation often allows:
- Faster algorithms
- Lower memory usage
- Elegant mathematical solutions

Bit manipulation is heavily used in:
- Competitive programming
- Cryptography
- Operating systems
- Embedded systems
- Networking
- Graphics programming

---

# 2. Binary Number Fundamentals

Binary uses only:
- `0`
- `1`

Each position represents a power of 2.

Example:

```text
Decimal: 13

Binary:
1101

Positions:
8 4 2 1
1 1 0 1
```

Calculation:

```text
8 + 4 + 0 + 1 = 13
```

---

# 3. Bitwise Operators in Python

| Operator | Name | Example |
|---|---|---|
| `&` | AND | `a & b` |
| `|` | OR | `a | b` |
| `^` | XOR | `a ^ b` |
| `~` | NOT | `~a` |
| `<<` | Left Shift | `a << 2` |
| `>>` | Right Shift | `a >> 2` |

---

## 3.1 AND Operator (`&`)

A bit becomes `1` only if both bits are `1`.

```python
a = 12  # 1100
b = 10  # 1010

print(a & b)
```

Output:

```python
8
```

Explanation:

```text
1100
1010
----
1000
```

---

## 3.2 OR Operator (`|`)

A bit becomes `1` if at least one bit is `1`.

```python
print(12 | 10)
```

Output:

```python
14
```

---

## 3.3 XOR Operator (`^`)

A bit becomes `1` if bits are different.

```python
print(12 ^ 10)
```

Output:

```python
6
```

---

## 3.4 NOT Operator (`~`)

Flips all bits.

```python
print(~5)
```

Output:

```python
-6
```

Explanation:

Python uses two's complement representation.

---

## 3.5 Left Shift (`<<`)

Shifts bits left.

Equivalent to multiplying by powers of 2.

```python
print(5 << 1)
```

Output:

```python
10
```

---

## 3.6 Right Shift (`>>`)

Shifts bits right.

Equivalent to floor division by powers of 2.

```python
print(20 >> 2)
```

Output:

```python
5
```

---

# 4. Understanding Signed Integers

Python integers are not fixed-width like C/C++ integers.

However, negative numbers still conceptually use two's complement.

Example:

```python
print(bin(-5))
```

Output:

```python
-0b101
```

Useful trick:

```python
print((-5) & 0xff)
```

Output:

```python
251
```

This masks the number to 8 bits.

---

# 5. Common Bit Manipulation Techniques

---

## 5.1 Check if Number is Even

```python
def is_even(n):
    return (n & 1) == 0
```

---

## 5.2 Check if Number is Power of Two

Important property:

```text
Power of two:
100000

Minus one:
011111

AND result:
000000
```

Code:

```python
def is_power_of_two(n):
    return n > 0 and (n & (n - 1)) == 0
```

---

## 5.3 Get the i-th Bit

```python
def get_bit(n, i):
    return (n >> i) & 1
```

---

## 5.4 Set the i-th Bit

```python
def set_bit(n, i):
    return n | (1 << i)
```

---

## 5.5 Clear the i-th Bit

```python
def clear_bit(n, i):
    return n & ~(1 << i)
```

---

## 5.6 Toggle the i-th Bit

```python
def toggle_bit(n, i):
    return n ^ (1 << i)
```

---

# 6. Advanced Bit Tricks

---

## 6.1 Remove Lowest Set Bit

```python
n = n & (n - 1)
```

Example:

```python
n = 12  # 1100
n = n & (n - 1)

print(n)
```

Output:

```python
8
```

---

## 6.2 Isolate Lowest Set Bit

```python
lowest = n & -n
```

Example:

```python
n = 12  # 1100

print(n & -n)
```

Output:

```python
4
```

---

## 6.3 Count Set Bits

### Method 1: Built-in

```python
n.bit_count()
```

### Method 2: Brian Kernighan Algorithm

```python
def count_bits(n):
    count = 0

    while n:
        n &= n - 1
        count += 1

    return count
```

Time Complexity:

```text
O(number of set bits)
```

---

## 6.4 Swap Two Numbers Using XOR

```python
a ^= b
b ^= a
a ^= b
```

Modern Python recommendation:

```python
a, b = b, a
```

---

# 7. Bit Masks

Bit masks allow compact representation of states.

Example:

```python
permissions = 0

READ = 1 << 0
WRITE = 1 << 1
EXECUTE = 1 << 2
```

Grant permissions:

```python
permissions |= READ
permissions |= WRITE
```

Check permission:

```python
if permissions & READ:
    print("Can read")
```

Remove permission:

```python
permissions &= ~WRITE
```

---

# 8. XOR Applications

---

## 8.1 Find Single Number

Problem:
Every number appears twice except one.

```python
def single_number(nums):
    result = 0

    for num in nums:
        result ^= num

    return result
```

Example:

```python
nums = [4,1,2,1,2]

print(single_number(nums))
```

Output:

```python
4
```

---

## 8.2 Missing Number

```python
def missing_number(nums):
    xor_all = 0

    for i in range(len(nums) + 1):
        xor_all ^= i

    for num in nums:
        xor_all ^= num

    return xor_all
```

---

# 9. Working with Sets Using Bits

Bitmasks can represent subsets.

Example:

```python
A = 1 << 0
B = 1 << 1
C = 1 << 2
```

Set containing A and C:

```python
mask = A | C
```

Check membership:

```python
if mask & A:
    print("A exists")
```

---

# 10. Bit Manipulation in Algorithms

---

## 10.1 Generate All Subsets

```python
def subsets(nums):
    n = len(nums)
    result = []

    for mask in range(1 << n):
        subset = []

        for i in range(n):
            if mask & (1 << i):
                subset.append(nums[i])

        result.append(subset)

    return result
```

Example:

```python
print(subsets([1,2,3]))
```

---

## 10.2 Gray Code

Gray code changes only one bit at a time.

Formula:

```python
gray = n ^ (n >> 1)
```

Implementation:

```python
def gray_code(n):
    result = []

    for i in range(1 << n):
        result.append(i ^ (i >> 1))

    return result
```

---

# 11. Dynamic Programming with Bitmasking

Bitmask DP is used when:
- `n <= 20`
- States represent subsets

---

## Example: Traveling Salesman Problem

```python
from functools import lru_cache

INF = float('inf')

def tsp(dist):
    n = len(dist)

    @lru_cache(None)
    def dp(mask, pos):

        if mask == (1 << n) - 1:
            return dist[pos][0]

        ans = INF

        for city in range(n):
            if not mask & (1 << city):
                ans = min(
                    ans,
                    dist[pos][city] +
                    dp(mask | (1 << city), city)
                )

        return ans

    return dp(1, 0)
```

Time Complexity:

```text
O(2^n * n^2)
```

---

# 12. Competitive Programming Techniques

---

## 12.1 Enumerating Submasks

```python
submask = mask

while submask:
    print(submask)
    submask = (submask - 1) & mask
```

Very important optimization.

---

## 12.2 Fast Multiplication/Division

```python
x << k
```

Equivalent to:

```python
x * (2 ** k)
```

And:

```python
x >> k
```

Equivalent to:

```python
x // (2 ** k)
```

---

## 12.3 Compress Boolean Arrays

Instead of:

```python
visited = [False] * 32
```

Use:

```python
visited = 0
```

Memory efficient.

---

# 13. Real-World Applications

---

## Networking

IP masks:

```text
255.255.255.0
```

use bitwise operations.

---

## Cryptography

Encryption algorithms heavily use:
- XOR
- Shifts
- Rotations

---

## Graphics Programming

RGBA colors are packed into integers.

Example:

```text
0xRRGGBBAA
```

---

## Operating Systems

Permission bits:

```text
rwxr-xr-x
```

are bit masks.

---

# 14. Time Complexity Analysis

| Operation | Complexity |
|---|---|
| AND / OR / XOR | O(1) |
| Shift | O(1) |
| Count Bits | O(number of set bits) |
| Subset Generation | O(2^n) |
| Bitmask DP | Usually O(2^n * n) |

---

# 15. Common Pitfalls

---

## Pitfall 1: Negative Numbers

Python handles negatives differently than fixed-width languages.

Example:

```python
print(~5)
```

Output:

```python
-6
```

---

## Pitfall 2: Infinite Integer Size

Python integers are arbitrary precision.

This differs from C/C++ overflow behavior.

---

## Pitfall 3: Operator Precedence

Incorrect:

```python
if n & 1 == 0:
```

Correct:

```python
if (n & 1) == 0:
```

---

# 16. Practice Problems

---

## Easy

1. Single Number
2. Number of 1 Bits
3. Power of Two
4. Missing Number
5. Counting Bits

---

## Medium

1. Subsets
2. Maximum XOR of Two Numbers
3. Gray Code
4. Sum of Two Integers
5. UTF-8 Validation

---

## Hard

1. Traveling Salesman Problem
2. Shortest Hamiltonian Path
3. Minimum XOR Pair
4. Parallel Courses II
5. Beautiful Arrangement

---

# 17. Conclusion

Bit manipulation is one of the most powerful tools in algorithm design.

Key benefits:
- Speed
- Memory efficiency
- Elegant mathematical solutions

Core ideas to master:
- AND / OR / XOR
- Bit masks
- Subset enumeration
- XOR tricks
- Bitmask dynamic programming

Once comfortable with these techniques, many difficult algorithmic problems become significantly easier.

---

# Additional Exercises

Try implementing:

1. Binary Trie
2. Fast Exponentiation
3. Hamming Distance
4. Bitwise Sieve of Eratosthenes
5. Maximum XOR Subarray
6. SOS DP
7. Meet in the Middle Algorithms

---

# Recommended Learning Path

1. Binary Basics
2. Bitwise Operators
3. Bit Tricks
4. XOR Problems
5. Subset Enumeration
6. Bitmask DP
7. Advanced Competitive Programming

