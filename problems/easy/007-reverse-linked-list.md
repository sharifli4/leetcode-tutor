---
id: 206
title: Reverse Linked List
difficulty: easy
category: linked-lists
tags: [linked-list, recursion]
leetcode_url: https://leetcode.com/problems/reverse-linked-list/
---

## Problem

Given the `head` of a singly linked list, reverse the list, and return the reversed list's head.

**Example 1:**
```
Input: head = [1, 2, 3, 4, 5]
Output: [5, 4, 3, 2, 1]
```

**Example 2:**
```
Input: head = [1, 2]
Output: [2, 1]
```

**Example 3:**
```
Input: head = []
Output: []
```

**Constraints:**
- The number of nodes in the list is in the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

## Hint

You need to flip the direction of every `next` pointer. Before you change a node's `next` pointer, make sure you haven't lost your reference to the rest of the list.

## Approach

Iterate through the list while maintaining two pointers: `prev` (initially `None`) and `current` (initially `head`). At each step, save `current.next`, flip `current.next` to point to `prev`, then advance both pointers forward. When `current` becomes `None`, `prev` is the new head.

## Walkthrough

1. Initialize `prev = None` and `current = head`.
2. While `current` is not `None`:
   a. Save `next_node = current.next` so you don't lose the rest of the list.
   b. Reverse the pointer: `current.next = prev`.
   c. Advance `prev = current`.
   d. Advance `current = next_node`.
3. After the loop, `current` is `None` and `prev` points to the old tail — the new head.
4. Return `prev`.

## Solution

```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        current = head

        while current:
            next_node = current.next  # save remainder
            current.next = prev       # reverse pointer
            prev = current            # advance prev
            current = next_node       # advance current

        return prev
```

## Complexity

- **Time:** O(n) — each node is visited exactly once.
- **Space:** O(1) — reversal is done in-place with a fixed number of pointer variables.
