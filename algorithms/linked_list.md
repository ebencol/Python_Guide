# Linked List Algorithms in Python

## Reverse a Linked List

### Question
Given the head of a singly linked list, reverse the list and return the new head.

### Example
```python
Input: 1 -> 2 -> 3 -> 4 -> 5
Output: 5 -> 4 -> 3 -> 2 -> 1
```

### Approach
Use two pointers:
- `prev` to store the previous node
- `current` to traverse the list

For each node:
1. Store the next node temporarily
2. Reverse the pointer
3. Move both pointers forward

### Time Complexity
- **O(n)** — traverse the list once

### Space Complexity
- **O(1)** — in-place reversal

### Solution
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


def reverseList(head):
    prev = None
    current = head

    while current:
        nxt = current.next
        current.next = prev

        prev = current
        current = nxt

    return prev
```

---

# Detect Cycle (Floyd’s Tortoise and Hare)

## Question
Given the head of a linked list, determine if the linked list has a cycle.

### Example
```python
Input: 3 -> 2 -> 0 -> -4
                ^    |
                |____|

Output: True
```

### Approach
Use two pointers:
- `slow` moves one step
- `fast` moves two steps

If there is a cycle, they will eventually meet.

### Time Complexity
- **O(n)**

### Space Complexity
- **O(1)**

### Solution
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


def hasCycle(head):
    slow = head
    fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

        if slow == fast:
            return True

    return False
```

---

# Merge Two Sorted Lists

## Question
Merge two sorted linked lists and return the merged sorted list.

### Example
```python
Input:
list1 = 1 -> 2 -> 4
list2 = 1 -> 3 -> 4

Output:
1 -> 1 -> 2 -> 3 -> 4 -> 4
```

### Approach
Use a dummy node to simplify edge cases.

Compare nodes from both lists:
- Append the smaller node
- Move the pointer forward

At the end, append the remaining nodes.

### Time Complexity
- **O(n + m)**

### Space Complexity
- **O(1)**

### Solution
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


def mergeTwoLists(list1, list2):
    dummy = ListNode()
    tail = dummy

    while list1 and list2:
        if list1.val < list2.val:
            tail.next = list1
            list1 = list1.next
        else:
            tail.next = list2
            list2 = list2.next

        tail = tail.next

    tail.next = list1 if list1 else list2

    return dummy.next
```

---

# Remove Nth Node From End

## Question
Given the head of a linked list, remove the nth node from the end of the list and return its head.

### Example
```python
Input: 1 -> 2 -> 3 -> 4 -> 5, n = 2
Output: 1 -> 2 -> 3 -> 5
```

### Approach
Use two pointers:
1. Move `fast` ahead by `n` steps
2. Move both `slow` and `fast` until `fast` reaches the end
3. Remove the target node

A dummy node helps handle edge cases.

### Time Complexity
- **O(n)**

### Space Complexity
- **O(1)**

### Solution
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


def removeNthFromEnd(head, n):
    dummy = ListNode(0, head)

    slow = dummy
    fast = dummy

    for _ in range(n + 1):
        fast = fast.next

    while fast:
        slow = slow.next
        fast = fast.next

    slow.next = slow.next.next

    return dummy.next
```

---

# Add Two Numbers (Linked List Form)

## Question
You are given two non-empty linked lists representing two non-negative integers.
The digits are stored in reverse order, and each node contains a single digit.

Add the two numbers and return the sum as a linked list.

### Example
```python
Input:
l1 = 2 -> 4 -> 3
l2 = 5 -> 6 -> 4

Output:
7 -> 0 -> 8
```

Explanation:
342 + 465 = 807

### Approach
Traverse both lists simultaneously:
- Add corresponding digits
- Include carry
- Create new nodes for the result

Continue until:
- both lists are exhausted
- and carry becomes zero

### Time Complexity
- **O(max(n, m))**

### Space Complexity
- **O(max(n, m))**

### Solution
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


def addTwoNumbers(l1, l2):
    dummy = ListNode()
    current = dummy

    carry = 0

    while l1 or l2 or carry:
        val1 = l1.val if l1 else 0
        val2 = l2.val if l2 else 0

        total = val1 + val2 + carry

        carry = total // 10
        digit = total % 10

        current.next = ListNode(digit)
        current = current.next

        if l1:
            l1 = l1.next

        if l2:
            l2 = l2.next

    return dummy.next
```
