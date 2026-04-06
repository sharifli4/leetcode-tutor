---
id: 104
title: Maximum Depth of Binary Tree
difficulty: easy
category: trees
tags: [tree, depth-first search, breadth-first search, recursion, binary tree]
leetcode_url: https://leetcode.com/problems/maximum-depth-of-binary-tree/
---

## Problem

Given the `root` of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**
```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2:**
```
Input: root = [1,null,2]
Output: 2
```

**Example 3:**
```
Input: root = []
Output: 0
```

**Constraints:**
- The number of nodes in the tree is in the range `[0, 10^4]`.
- `-100 <= Node.val <= 100`

## Hint

The depth of a tree is related to the depths of its subtrees. What is the depth of a tree whose root has a left subtree of depth 3 and a right subtree of depth 5?

## Approach

Use recursive DFS. The maximum depth of a tree rooted at a node is 1 (for the node itself) plus the maximum depth of its deeper subtree. The base case is a `None` node, which has depth 0. This naturally traverses every node and bubbles the correct depth back up.

## Walkthrough

1. Define the recursive function `maxDepth(root)`.
2. Base case: if `root` is `None`, return `0`.
3. Recursively compute the depth of the left subtree: `left_depth = maxDepth(root.left)`.
4. Recursively compute the depth of the right subtree: `right_depth = maxDepth(root.right)`.
5. The depth of the current subtree is `1 + max(left_depth, right_depth)`.
6. Return that value.
7. The initial call `maxDepth(root)` gives the answer for the whole tree.

## Solution

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxDepth(root: TreeNode) -> int:
    if root is None:
        return 0
    return 1 + max(maxDepth(root.left), maxDepth(root.right))
```

## Complexity

- **Time:** O(n) — every node is visited exactly once.
- **Space:** O(h) — the recursion stack is as deep as the tree height `h`; O(log n) average, O(n) worst case for a skewed tree.
