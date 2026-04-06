---
id: 124
title: Binary Tree Maximum Path Sum
difficulty: hard
category: trees
tags: [tree, binary-tree, depth-first-search, dynamic-programming, recursion]
leetcode_url: https://leetcode.com/problems/binary-tree-maximum-path-sum/
---

## Problem

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes has an edge connecting them. A node can appear in a sequence at most once. The path does not need to pass through the root.

The **path sum** of a path is the sum of all node values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

**Example 1:**
```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```

**Example 2:**
```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

**Example 3:**
```
Input: root = [-3]
Output: -3
```

**Constraints:**
- The number of nodes in the tree is in the range `[1, 3 * 10^4]`.
- `-1000 <= Node.val <= 1000`

## Hint

A path can "turn" at most once — it goes down one side, possibly through a node, and down the other. When you're doing DFS, you need to track two different quantities: the best path that passes *through* the current node (which may update the global answer), and the best path that *extends upward* from the current node to its parent (which can only go one direction).

## Approach

Perform a post-order DFS. At each node, recursively get the maximum single-arm contribution from the left and right children (clamped to 0 if negative — we'd rather not include a losing subtree). Use `node.val + left_gain + right_gain` to evaluate a path that passes through the current node and update a global maximum. Return only `node.val + max(left_gain, right_gain)` to the parent, since a path going up can only branch one way.

## Walkthrough

1. Initialize `self.max_sum = float('-inf')` (must handle all-negative trees).
2. Define a helper `dfs(node)` that returns the maximum gain achievable from this node going downward in a single direction.
3. Base case: if `node` is `None`, return 0.
4. Recurse: `left_gain = max(dfs(node.left), 0)` — discard negative contributions.
5. Recurse: `right_gain = max(dfs(node.right), 0)`.
6. Compute the **path through this node**: `path_sum = node.val + left_gain + right_gain`.
7. Update `self.max_sum = max(self.max_sum, path_sum)`.
8. Return `node.val + max(left_gain, right_gain)` — the best single-arm extension to the parent.
9. Call `dfs(root)` and return `self.max_sum`.

## Solution

```python
from typing import Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.max_sum = float('-inf')

        def dfs(node: Optional[TreeNode]) -> int:
            if not node:
                return 0

            # Only take a child's contribution if it's positive
            left_gain = max(dfs(node.left), 0)
            right_gain = max(dfs(node.right), 0)

            # Best path that passes through this node (can use both arms)
            path_through = node.val + left_gain + right_gain
            self.max_sum = max(self.max_sum, path_through)

            # Return the best single-arm gain to the parent
            return node.val + max(left_gain, right_gain)

        dfs(root)
        return self.max_sum
```

## Complexity

- **Time:** O(n) — every node is visited exactly once during the DFS.
- **Space:** O(h) — the call stack depth equals the height of the tree; O(n) in the worst case (skewed tree), O(log n) for a balanced tree.
