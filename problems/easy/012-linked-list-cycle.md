---
id: 141
title: Linked List Cycle
difficulty: easy
category: linked-lists
tags: [linked list, two pointers, Floyd's cycle detection]
leetcode_url: https://leetcode.com/problems/linked-list-cycle/
---

## Problem

Given the `head` of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if some node in the list can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (0-indexed). Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

**Example 1:**
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**
```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3:**
```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

**Constraints:**
- The number of nodes in the list is in the range `[0, 10^4]`.
- `-10^5 <= Node.val <= 10^5`
- `pos` is `-1` or a valid index in the linked list.

**Follow up:** Can you solve it using O(1) memory?

## Hint

What if you had two runners moving through the list at different speeds? If there's a cycle, will they ever meet?

## Approach

Floyd's Tortoise and Hare algorithm uses two pointers moving at different speeds. The slow pointer advances one node at a time while the fast pointer advances two nodes at a time. If there is a cycle, the fast pointer will eventually lap the slow pointer and they will meet. If there is no cycle, the fast pointer will reach the end of the list.

## Walkthrough

1. Handle the edge case: if the list is empty (`head` is `None`), return `False` immediately.
2. Initialize two pointers: `slow = head` and `fast = head`.
3. Enter a loop that continues as long as `fast` and `fast.next` are not `None` (i.e., we haven't reached the end).
4. Move `slow` one step forward: `slow = slow.next`.
5. Move `fast` two steps forward: `fast = fast.next.next`.
6. After each move, check if `slow == fast`. If they are the same node, a cycle exists — return `True`.
7. If the loop exits naturally (fast reached the end), there is no cycle — return `False`.

## Solution

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def hasCycle(head: ListNode) -> bool:
    slow = head
    fast = head

    while fast is not None and fast.next is not None:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            return True

    return False
```

## Complexity

- **Time:** O(n) — in the worst case (no cycle), fast traverses the whole list; with a cycle, the two pointers meet within at most n iterations.
- **Space:** O(1) — only two pointer variables are used, regardless of list length.
