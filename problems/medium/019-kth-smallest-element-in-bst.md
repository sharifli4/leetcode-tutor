---
id: 230
title: Kth Smallest Element in a BST
difficulty: medium
category: trees
tags: [tree, depth-first-search, binary-search-tree, binary-tree]
leetcode_url: https://leetcode.com/problems/kth-smallest-element-in-a-bst/
---

## Problem

Given the `root` of a binary search tree, and an integer `k`, return the `k`th smallest value (**1-indexed**) of all the values of the nodes in the tree.

**Example 1:**
```
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

**Example 2:**
```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

**Constraints:**
- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 10^4`
- `0 <= Node.val <= 10^4`

**Follow up:** If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

## Hint

Recall a key property of BSTs: in-order traversal visits nodes in ascending sorted order. The kth node visited during in-order traversal is the kth smallest.

## Approach

Perform an in-order traversal (left → root → right) while counting visited nodes. When the counter reaches `k`, the current node holds the answer. This can be implemented iteratively with a stack for efficiency, or recursively with early termination.

## Walkthrough

1. Use an iterative in-order traversal with an explicit stack to avoid recursion overhead.
2. Initialize `stack = []`, `current = root`, and `count = 0`.
3. Loop while `current` is not `None` or the stack is not empty:
   a. Push all the way left: while `current`, push `current` and go left.
   b. Pop the top node — this is the next smallest.
   c. Increment `count`. If `count == k`, return `node.val`.
   d. Move to the right subtree: `current = node.right`.
4. The problem guarantees `k` is valid, so we will always find the answer.

## Solution

```python
from typing import Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stack = []
        current = root
        count = 0

        while current or stack:
            # Go as far left as possible
            while current:
                stack.append(current)
                current = current.left

            # Process the node
            current = stack.pop()
            count += 1
            if count == k:
                return current.val

            # Move to the right subtree
            current = current.right

        return -1  # Should never reach here given valid input
```

## Complexity

- **Time:** O(H + k) — where H is the tree height. We traverse down to the leftmost node (O(H)) then visit k nodes.
- **Space:** O(H) — the stack holds at most H nodes at any time (the left spine of the current subtree).
