---
id: 23
title: Merge K Sorted Lists
difficulty: hard
category: linked-lists
tags: [linked-list, divide-and-conquer, heap, merge-sort]
leetcode_url: https://leetcode.com/problems/merge-k-sorted-lists/
---

## Problem

You are given an array of `k` linked lists, where each linked list is sorted in ascending order. Merge all the linked lists into one sorted linked list and return it.

**Example 1:**
```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked lists are:
  1 -> 4 -> 5
  1 -> 3 -> 4
  2 -> 6
Merging them gives: 1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6
```

**Example 2:**
```
Input: lists = []
Output: []
```

**Example 3:**
```
Input: lists = [[]]
Output: []
```

**Constraints:**
- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- Each `lists[i]` is sorted in ascending order.
- The sum of all `lists[i].length` will not exceed `10^4`.

## Hint

With k lists you need to efficiently find the smallest current head at each step. Think about a data structure that gives you the minimum element in O(log k) time — that way you don't re-scan all k heads on every pick.

## Approach

Use a min-heap to always extract the node with the smallest value across all list heads. Push the initial heads of all non-null lists into the heap, then repeatedly pop the minimum, attach it to the result, and push that node's successor. This processes every node exactly once while keeping the heap size bounded by k.

## Walkthrough

1. Handle the edge case where `lists` is empty or contains only null lists — return `None`.
2. Create a dummy head node to simplify result list construction; keep a `curr` pointer to it.
3. Initialize a min-heap. Push tuples `(node.val, index, node)` for each non-null list head, using `index` as a tiebreaker to avoid comparing `ListNode` objects directly.
4. Enter the main loop: while the heap is non-empty, pop the smallest tuple `(val, i, node)`.
5. Attach the popped node to `curr.next` and advance `curr` to this node.
6. If the popped node has a `next`, push `(node.next.val, i, node.next)` onto the heap so its successor enters the competition.
7. Repeat until the heap is exhausted — all nodes have been placed in sorted order.
8. Return `dummy.next` as the head of the merged list.

## Solution

```python
import heapq
from typing import List, Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        dummy = ListNode(0)
        curr = dummy
        heap = []

        # Push the head of each non-empty list onto the heap.
        # Use the list index as a tiebreaker so we never compare ListNode objects.
        for i, node in enumerate(lists):
            if node:
                heapq.heappush(heap, (node.val, i, node))

        while heap:
            val, i, node = heapq.heappop(heap)
            curr.next = node
            curr = curr.next
            if node.next:
                heapq.heappush(heap, (node.next.val, i, node.next))

        return dummy.next
```

## Complexity

- **Time:** O(N log k) — N is the total number of nodes across all lists; each heap push/pop is O(log k).
- **Space:** O(k) — the heap holds at most one node per list at any time.
