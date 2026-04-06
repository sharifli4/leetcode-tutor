---
id: 25
title: Reverse Nodes in K-Group
difficulty: hard
category: linked-lists
tags: [linked-list, recursion, iterative]
leetcode_url: https://leetcode.com/problems/reverse-nodes-in-k-group/
---

## Problem

Given the head of a linked list, reverse the nodes of the list `k` at a time, and return the modified list.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then the left-out nodes at the end should remain as is.

You may not alter the values in the list's nodes — only the nodes themselves may be changed.

**Example 1:**
```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**
```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

**Example 3:**
```
Input: head = [1,2,3,4,5], k = 1
Output: [1,2,3,4,5]
```

**Constraints:**
- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

## Hint

Before reversing a group, first verify that at least k nodes remain — otherwise leave them alone. For the reversal itself, think about how to reconnect the tail of the reversed group to the result of processing the rest of the list.

## Approach

Use a dummy head to simplify pointer management. For each group, first check whether k nodes exist ahead; if not, stop. Then reverse exactly k nodes in place by rewiring `next` pointers, keeping track of the group's tail (which becomes the new connection point to the next group). Repeat until fewer than k nodes remain.

## Walkthrough

1. Create a `dummy` node pointing to `head`. Use `group_prev` (initially `dummy`) to track the node just before each group.
2. Enter the main loop:
3. **Check k nodes exist:** walk a `kth` pointer k steps forward from `group_prev`. If `kth` is `None` before k steps, break — remaining nodes stay as-is.
4. Identify `group_next = kth.next` — the node immediately after the current group.
5. **Reverse k nodes:** use standard iterative reversal with `prev = group_next` and `curr = group_prev.next`, iterating k times.
   - In each step: save `tmp = curr.next`, point `curr.next = prev`, advance `prev = curr`, `curr = tmp`.
6. After reversal, `prev` is the new head of the reversed group and `group_prev.next` is now the tail of the reversed group.
7. Connect the tail: `group_prev.next.next = group_next` (already done by step 5's reversal).
   - Actually: let `tail = group_prev.next` (original first node, now last), then set `group_prev.next = prev`.
8. Advance `group_prev = tail` for the next iteration.
9. Return `dummy.next`.

## Solution

```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        group_prev = dummy

        while True:
            # Check if k nodes remain starting after group_prev
            kth = self.get_kth(group_prev, k)
            if not kth:
                break

            group_next = kth.next  # node right after the current group

            # Reverse k nodes: prev starts as group_next so the tail connects correctly
            prev = group_next
            curr = group_prev.next
            for _ in range(k):
                tmp = curr.next
                curr.next = prev
                prev = curr
                curr = tmp

            # group_prev.next is the old head of the group (now the tail after reversal)
            tail = group_prev.next
            group_prev.next = prev  # prev is the new head of the reversed group
            group_prev = tail       # advance group_prev to the tail of the reversed group

        return dummy.next

    def get_kth(self, curr: Optional[ListNode], k: int) -> Optional[ListNode]:
        """Walk k steps from curr; return the node at position k, or None if not enough nodes."""
        while curr and k > 0:
            curr = curr.next
            k -= 1
        return curr
```

## Complexity

- **Time:** O(n) — every node is visited a constant number of times (once for the k-check walk, once for reversal).
- **Space:** O(1) — only a fixed set of pointers is maintained; no recursion stack or auxiliary structures.
