---
id: 143
title: Reorder List
difficulty: medium
category: linked-lists
tags: [linked-list, two-pointers, stack, recursion]
leetcode_url: https://leetcode.com/problems/reorder-list/
---

## Problem

You are given the head of a singly linked list. The list can be represented as:

`L0 → L1 → … → Ln-1 → Ln`

Reorder it to:

`L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …`

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1:**
```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

**Example 2:**
```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

**Constraints:**
- The number of nodes in the list is in the range `[1, 5 * 10^4]`.
- `1 <= Node.val <= 1000`

## Hint

The pattern "first, last, second, second-to-last, ..." suggests you need access to both ends of the list simultaneously. What if you split the list in half and reversed one of the halves?

## Approach

Break the problem into three steps: (1) find the middle of the list using slow/fast pointers, (2) reverse the second half in place, then (3) merge the two halves by alternating nodes from each.

## Walkthrough

1. Use slow/fast pointers to find the middle of the list. When `fast` reaches the end, `slow` is at the midpoint.
2. Split the list: set `slow.next = None` to terminate the first half.
3. Reverse the second half: iterate through it, reversing the `next` pointers until you have a reversed second-half list.
4. Merge the two halves: alternate taking one node from the first half, then one from the second half.
5. The first half drives the merge loop — continue until either pointer is exhausted.

## Solution

```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        if not head or not head.next:
            return

        # Step 1: Find the middle using slow/fast pointers
        slow, fast = head, head
        while fast.next and fast.next.next:
            slow = slow.next
            fast = fast.next.next

        # Step 2: Reverse the second half
        prev = None
        curr = slow.next
        slow.next = None  # Cut the list in half
        while curr:
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        second = prev  # Head of reversed second half

        # Step 3: Merge the two halves
        first = head
        while second:
            tmp1 = first.next
            tmp2 = second.next
            first.next = second
            second.next = tmp1
            first = tmp1
            second = tmp2
```

## Complexity

- **Time:** O(n) — each of the three phases (find middle, reverse, merge) is a single linear pass.
- **Space:** O(1) — the reordering is done in place with only a handful of pointer variables.
