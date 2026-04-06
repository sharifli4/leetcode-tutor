---
id: 98
title: Validate Binary Search Tree
difficulty: medium
category: trees
tags: [tree, depth-first-search, binary-search-tree, binary-tree]
leetcode_url: https://leetcode.com/problems/validate-binary-search-tree/
---

## Problem

Given the `root` of a binary tree, determine if it is a valid **binary search tree** (BST).

A valid BST is defined as follows:
- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**
```
Input: root = [2,1,3]
Output: true
```

**Example 2:**
```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

**Example 3:**
```
Input: root = [5,4,6,null,null,3,7]
Output: false
Explanation: Node 3 is in the right subtree of 5, violating the BST property.
```

**Constraints:**
- The number of nodes in the tree is in the range `[1, 10^4]`.
- `-2^31 <= Node.val <= 2^31 - 1`

## Hint

Checking only the direct parent-child relationship is not enough — every node in a left subtree must be less than all ancestors above it. Can you pass down the valid range a node is allowed to have as you recurse?

## Approach

Recursively validate each node by passing down a `(min_val, max_val)` range that the current node's value must fall within. When going left, tighten the upper bound to the current node's value; when going right, tighten the lower bound.

## Walkthrough

1. Define a helper `validate(node, min_val, max_val)`.
2. Base case: if `node` is `None`, return `True` (empty tree is valid).
3. Check if `node.val` is within `(min_val, max_val)` — exclusive bounds. If not, return `False`.
4. Recursively validate the left subtree with `max_val` tightened to `node.val`.
5. Recursively validate the right subtree with `min_val` tightened to `node.val`.
6. Return `True` only if both recursive calls return `True`.
7. Call `validate(root, float('-inf'), float('inf'))` and return the result.

## Solution

```python
from typing import Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def validate(node: Optional[TreeNode], min_val: float, max_val: float) -> bool:
            if node is None:
                return True
            if not (min_val < node.val < max_val):
                return False
            return (validate(node.left, min_val, node.val) and
                    validate(node.right, node.val, max_val))

        return validate(root, float('-inf'), float('inf'))
```

## Complexity

- **Time:** O(n) — every node is visited exactly once.
- **Space:** O(h) — the recursion stack depth equals the tree height h; O(log n) for a balanced tree, O(n) in the worst case (skewed tree).
