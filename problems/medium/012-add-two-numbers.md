---
id: 2
title: Add Two Numbers
difficulty: medium
category: linked-lists
tags: [linked-list, math, recursion]
leetcode_url: https://leetcode.com/problems/add-two-numbers/
---

## Problem

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807
```

**Example 2:**
```
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3:**
```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
Explanation: 9999999 + 9999 = 10009998
```

**Constraints:**
- The number of nodes in each list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

## Hint

Think of it like elementary school addition: process digit by digit from the head (which is the ones place since the list is reversed), keeping track of any carry that spills into the next position.

## Approach

Iterate through both lists simultaneously, summing corresponding digits along with any carry from the previous step. At each position, the new digit is `(sum) % 10` and the new carry is `(sum) // 10`. Continue until both lists are exhausted and carry is zero.

## Walkthrough

1. Create a dummy head node to simplify result list construction; keep a `current` pointer at it.
2. Initialize `carry = 0`.
3. While either `l1` or `l2` is not `None`, or `carry != 0`:
   a. Extract the value from `l1` (or 0 if exhausted) and advance `l1`.
   b. Extract the value from `l2` (or 0 if exhausted) and advance `l2`.
   c. Compute `total = val1 + val2 + carry`.
   d. Set `carry = total // 10` and create a new node with `total % 10`.
   e. Attach the new node to `current.next` and advance `current`.
4. Return `dummy.next` as the head of the result list.

## Solution

```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0)
        current = dummy
        carry = 0

        while l1 is not None or l2 is not None or carry != 0:
            val1 = l1.val if l1 else 0
            val2 = l2.val if l2 else 0

            total = val1 + val2 + carry
            carry = total // 10
            current.next = ListNode(total % 10)
            current = current.next

            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next

        return dummy.next
```

## Complexity

- **Time:** O(max(m, n)) — we iterate through both lists once, where m and n are their lengths.
- **Space:** O(max(m, n)) — the result list has at most max(m, n) + 1 nodes.
