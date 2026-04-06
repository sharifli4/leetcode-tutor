---
id: 21
title: Merge Two Sorted Lists
difficulty: easy
category: linked-lists
tags: [linked-list, recursion]
leetcode_url: https://leetcode.com/problems/merge-two-sorted-lists/
---

## Problem

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** linked list and return the head of the merged list. The merged list should be made by splicing together the nodes of the first two lists.

**Example 1:**
```
Input: list1 = [1, 2, 4], list2 = [1, 3, 4]
Output: [1, 1, 2, 3, 4, 4]
```

**Example 2:**
```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**
```
Input: list1 = [], list2 = [0]
Output: [0]
```

**Constraints:**
- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in non-decreasing order.

## Hint

Creating a throwaway "dummy" node to start your result list lets you avoid special-casing the head of the merged list. Then just compare the front nodes of the two lists and advance whichever is smaller.

## Approach

Use a dummy head node to simplify edge cases. Maintain a `current` pointer that always points to the last node added to the merged list. At each step, compare the values at the fronts of `list1` and `list2`, attach the smaller node to `current.next`, and advance both `current` and the chosen list's pointer. When one list is exhausted, attach the remainder of the other.

## Walkthrough

1. Create a `dummy` node (value doesn't matter); set `current = dummy`.
2. While both `list1` and `list2` are non-null:
   a. Compare `list1.val` and `list2.val`.
   b. Attach the node with the smaller value to `current.next`.
   c. Advance the pointer of the list whose node was just used.
   d. Advance `current` to `current.next`.
3. Once one list is exhausted, attach the remaining non-null list directly to `current.next` (it's already sorted).
4. Return `dummy.next` — the actual head of the merged list.

## Solution

```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeTwoLists(
        self, list1: Optional[ListNode], list2: Optional[ListNode]
    ) -> Optional[ListNode]:
        dummy = ListNode(0)
        current = dummy

        while list1 and list2:
            if list1.val <= list2.val:
                current.next = list1
                list1 = list1.next
            else:
                current.next = list2
                list2 = list2.next
            current = current.next

        # Attach the remaining nodes (at most one list is non-null)
        current.next = list1 if list1 else list2

        return dummy.next
```

## Complexity

- **Time:** O(m + n) — every node from both lists is visited exactly once, where m and n are the lengths of `list1` and `list2`.
- **Space:** O(1) — the merge is done in-place by re-linking existing nodes; only a constant number of pointers are used.
