---
id: 217
title: Contains Duplicate
difficulty: easy
category: arrays-hashing
tags: [array, hash-table, sorting]
leetcode_url: https://leetcode.com/problems/contains-duplicate/
---

## Problem

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**
```
Input: nums = [1, 2, 3, 1]
Output: true
```

**Example 2:**
```
Input: nums = [1, 2, 3, 4]
Output: false
```

**Example 3:**
```
Input: nums = [1, 1, 1, 3, 3, 4, 3, 2, 4, 2]
Output: true
```

**Constraints:**
- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

## Hint

A set can only hold unique values. Think about what it means when the size of a set built from the array differs from the length of the array itself.

## Approach

Insert each number into a set as you iterate. If you ever try to insert a number that is already present, you've found a duplicate and can immediately return `True`. If the loop completes without finding a duplicate, return `False`. Equivalently, you can compare the length of the set to the length of the array after the fact.

## Walkthrough

1. Create an empty set `seen`.
2. Iterate over each `num` in `nums`.
3. If `num` is already in `seen`, return `True` immediately.
4. Otherwise, add `num` to `seen` and continue.
5. After the loop ends with no duplicate found, return `False`.

## Solution

```python
from typing import List

class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for num in nums:
            if num in seen:
                return True
            seen.add(num)
        return False
```

## Complexity

- **Time:** O(n) — each element is visited once; set lookup and insertion are O(1) on average.
- **Space:** O(n) — in the worst case (all distinct elements) the set holds all n values.
