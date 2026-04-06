---
id: 110
title: Balanced Binary Tree
difficulty: easy
category: trees
tags: [tree, depth-first search, recursion, binary tree]
leetcode_url: https://leetcode.com/problems/balanced-binary-tree/
---

## Problem

Given a binary tree, determine if it is height-balanced.

A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

**Example 1:**
```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

**Example 2:**
```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```

**Example 3:**
```
Input: root = []
Output: true
```

**Constraints:**
- The number of nodes in the tree is in the range `[0, 5000]`.
- `-10^4 <= Node.val <= 10^4`

## Hint

Can you write a recursive function that returns both the height of a subtree and whether that subtree is balanced? Returning two pieces of information at once avoids redundant work.

## Approach

Use a single post-order DFS that returns the height of each subtree (or -1 as a sentinel if a subtree is already unbalanced). At each node, check whether the absolute difference in heights of the left and right subtrees exceeds 1. If it does, propagate the -1 sentinel upward so we stop checking. This solves the problem in a single pass.

## Walkthrough

1. Define a helper `check(node)` that returns the height of the subtree rooted at `node`, or `-1` if the subtree is unbalanced.
2. Base case: if `node` is `None`, return `0` (height of an empty subtree).
3. Recursively call `check(node.left)` to get the left height. If it returns `-1`, immediately return `-1`.
4. Recursively call `check(node.right)` to get the right height. If it returns `-1`, immediately return `-1`.
5. Check the balance condition at the current node: `abs(left_height - right_height) > 1`. If true, return `-1`.
6. Otherwise, return `1 + max(left_height, right_height)` as the height of the current subtree.
7. In the main function, call `check(root)` and return `True` if the result is not `-1`, `False` otherwise.

## Solution

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isBalanced(root: TreeNode) -> bool:
    def check(node):
        if node is None:
            return 0

        left_height = check(node.left)
        if left_height == -1:
            return -1

        right_height = check(node.right)
        if right_height == -1:
            return -1

        if abs(left_height - right_height) > 1:
            return -1

        return 1 + max(left_height, right_height)

    return check(root) != -1
```

## Complexity

- **Time:** O(n) — each node is visited exactly once in the post-order traversal.
- **Space:** O(h) — the call stack grows to the height `h` of the tree; O(log n) for a balanced tree, O(n) in the worst case (skewed tree).
