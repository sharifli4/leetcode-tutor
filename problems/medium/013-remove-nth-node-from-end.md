---
id: 19
title: Remove Nth Node From End of List
difficulty: medium
category: linked-lists
tags: [linked-list, two-pointers]
leetcode_url: https://leetcode.com/problems/remove-nth-node-from-end-of-list/
---

## Problem

Given the head of a linked list, remove the `n`th node from the end of the list and return its head.

**Example 1:**
```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
Explanation: The 2nd node from the end is node with value 4.
```

**Example 2:**
```
Input: head = [1], n = 1
Output: []
```

**Example 3:**
```
Input: head = [1,2], n = 1
Output: [1]
```

**Constraints:**
- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**Follow up:** Could you do this in one pass?

## Hint

If you had two pointers separated by a gap of exactly `n` steps, what would the position of the trailing pointer be when the leading pointer reaches the end?

## Approach

Use two pointers, `fast` and `slow`, both starting at a dummy node before the head. Advance `fast` by `n + 1` steps first to create a gap of `n` between the two pointers. Then advance both together until `fast` reaches the end. At that point, `slow` sits just before the node to remove, so you can splice it out.

## Walkthrough

1. Create a `dummy` node that points to `head`; set both `fast` and `slow` to `dummy`.
2. Move `fast` forward `n + 1` times (the extra step ensures `slow` lands on the node *before* the target).
3. Advance both `fast` and `slow` one step at a time until `fast` is `None`.
4. Now `slow.next` is the node to remove.
5. Set `slow.next = slow.next.next` to skip over it.
6. Return `dummy.next` as the new head.

## Solution

```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        fast = dummy
        slow = dummy

        # Advance fast by n+1 steps
        for _ in range(n + 1):
            fast = fast.next

        # Move both until fast reaches the end
        while fast is not None:
            fast = fast.next
            slow = slow.next

        # Remove the nth node from end
        slow.next = slow.next.next

        return dummy.next
```

## Complexity

- **Time:** O(L) — we traverse the list at most once, where L is the list length.
- **Space:** O(1) — only two extra pointers are used regardless of list size.
