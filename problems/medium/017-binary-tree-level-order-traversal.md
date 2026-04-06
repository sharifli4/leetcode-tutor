---
id: 102
title: Binary Tree Level Order Traversal
difficulty: medium
category: trees
tags: [tree, breadth-first-search, binary-tree]
leetcode_url: https://leetcode.com/problems/binary-tree-level-order-traversal/
---

## Problem

Given the `root` of a binary tree, return the **level order traversal** of its nodes' values (i.e., from left to right, level by level).

**Example 1:**
```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

**Example 2:**
```
Input: root = [1]
Output: [[1]]
```

**Example 3:**
```
Input: root = []
Output: []
```

**Constraints:**
- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`

## Hint

Level order traversal processes nodes layer by layer — a natural fit for a queue. How can you tell when one level ends and the next begins while processing from a single queue?

## Approach

Use BFS with a queue. At the start of each iteration, the queue contains exactly the nodes of the current level. Process all of them (recording their values), and enqueue their children to form the next level. Repeat until the queue is empty.

## Walkthrough

1. If `root` is `None`, return an empty list immediately.
2. Initialize a `deque` with `root` and an empty `result` list.
3. While the queue is not empty:
   a. Determine the current level size: `level_size = len(queue)`.
   b. Create an empty `level` list.
   c. Loop `level_size` times: dequeue a node, append its value to `level`, and enqueue its non-null children (left then right).
   d. Append `level` to `result`.
4. Return `result`.

## Solution

```python
from typing import Optional, List
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        result = []
        queue = deque([root])

        while queue:
            level_size = len(queue)
            level = []

            for _ in range(level_size):
                node = queue.popleft()
                level.append(node.val)

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            result.append(level)

        return result
```

## Complexity

- **Time:** O(n) — every node is enqueued and dequeued exactly once.
- **Space:** O(n) — at most one full level is held in the queue at a time; the widest level can have up to n/2 nodes.
