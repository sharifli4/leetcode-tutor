---
id: 226
title: Invert Binary Tree
difficulty: easy
category: trees
tags: [tree, depth-first search, breadth-first search, binary tree, recursion]
leetcode_url: https://leetcode.com/problems/invert-binary-tree/
---

## Problem

Given the `root` of a binary tree, invert the tree, and return its root.

Inverting a binary tree means swapping the left and right children of every node — producing a mirror image of the original tree.

**Example 1:**
```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
Explanation:
    4                 4
   / \    becomes   / \
  2   7            7   2
 / \ / \          / \ / \
1  3 6  9        9  6 3  1
```

**Example 2:**
```
Input: root = [2,1,3]
Output: [2,3,1]
```

**Example 3:**
```
Input: root = []
Output: []
```

**Constraints:**
- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

## Hint

To mirror a tree at the root, you need to swap its left and right children. But each of those children is itself a subtree that also needs to be mirrored. Sound familiar?

## Approach

Use recursive DFS (pre-order works naturally here). At each node, swap its left and right children, then recursively invert the left and right subtrees. The base case is when the node is `None`. Because we swap before recursing, the approach also works cleanly iteratively with a queue (BFS), but the recursive version is the most concise.

## Walkthrough

1. Base case: if `root` is `None`, return `None` — nothing to invert.
2. Swap the left and right children of the current node: `root.left, root.right = root.right, root.left`.
3. Recursively call `invertTree` on the new `root.left` (which was the original right child).
4. Recursively call `invertTree` on the new `root.right` (which was the original left child).
5. Return `root` — the node itself hasn't moved, only its subtrees have been mirrored.

## Solution

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def invertTree(root: TreeNode) -> TreeNode:
    if root is None:
        return None

    root.left, root.right = root.right, root.left
    invertTree(root.left)
    invertTree(root.right)

    return root
```

## Complexity

- **Time:** O(n) — every node is visited exactly once.
- **Space:** O(h) — the recursion stack depth equals the tree height; O(log n) average, O(n) worst case for a skewed tree.
