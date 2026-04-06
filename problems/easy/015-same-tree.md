---
id: 100
title: Same Tree
difficulty: easy
category: trees
tags: [tree, depth-first search, breadth-first search, binary tree, recursion]
leetcode_url: https://leetcode.com/problems/same-tree/
---

## Problem

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example 1:**
```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

**Example 2:**
```
Input: p = [1,2], q = [1,null,2]
Output: false
Explanation: The trees have different structures.
```

**Example 3:**
```
Input: p = [1,2,1], q = [1,1,2]
Output: false
Explanation: The trees have the same structure but different node values.
```

**Constraints:**
- The number of nodes in both trees is in the range `[0, 100]`.
- `-10^4 <= Node.val <= 10^4`

## Hint

Two trees are the same if their roots have equal values AND their left subtrees are the same AND their right subtrees are the same. Think recursively.

## Approach

Use recursive DFS to compare both trees simultaneously. At each recursive call, check three things: both nodes are `None` (equal, base case), exactly one is `None` (unequal structure), or their values differ. If none of those short-circuit, recurse on left and right children. The trees are identical only if every corresponding pair of nodes passes all checks.

## Walkthrough

1. Base case 1: if both `p` and `q` are `None`, the subtrees are identical — return `True`.
2. Base case 2: if exactly one of them is `None`, the structures differ — return `False`.
3. Value check: if `p.val != q.val`, the nodes differ — return `False`.
4. Recursively check the left subtrees: `isSameTree(p.left, q.left)`.
5. Recursively check the right subtrees: `isSameTree(p.right, q.right)`.
6. Return `True` only if both recursive calls return `True`.

## Solution

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isSameTree(p: TreeNode, q: TreeNode) -> bool:
    if p is None and q is None:
        return True
    if p is None or q is None:
        return False
    if p.val != q.val:
        return False
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
```

## Complexity

- **Time:** O(n) — where n is the number of nodes in the smaller tree; we visit each pair of corresponding nodes once.
- **Space:** O(h) — the recursion stack depth equals the height of the trees; O(log n) average, O(n) worst case.
