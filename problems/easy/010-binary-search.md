---
id: 704
title: Binary Search
difficulty: easy
category: binary-search
tags: [array, binary-search]
leetcode_url: https://leetcode.com/problems/binary-search/
---

## Problem

Given an array of integers `nums` which is sorted in **ascending order**, and an integer `target`, write a function to search `target` in `nums`. If `target` exists then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**
```
Input: nums = [-1, 0, 3, 5, 9, 12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4.
```

**Example 2:**
```
Input: nums = [-1, 0, 3, 5, 9, 12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1.
```

**Example 3:**
```
Input: nums = [5], target = 5
Output: 0
```

**Constraints:**
- `1 <= nums.length <= 10^4`
- `-10^4 < nums[i], target < 10^4`
- All integers in `nums` are **unique**.
- `nums` is sorted in ascending order.

## Hint

Every time you look at the middle element you can eliminate half the array. If the middle is too small, the answer must be to its right; if it's too large, the answer must be to its left.

## Approach

Maintain a search window with `left` and `right` pointers. Repeatedly compute the midpoint and compare `nums[mid]` to `target`. If they match, return `mid`. If `target` is greater, move `left` to `mid + 1` to search the right half. If `target` is smaller, move `right` to `mid - 1` to search the left half. Stop when `left` exceeds `right`.

## Walkthrough

1. Set `left = 0` and `right = len(nums) - 1`.
2. While `left <= right`:
   a. Compute `mid = left + (right - left) // 2` (avoids integer overflow in other languages; safe in Python).
   b. If `nums[mid] == target`, return `mid`.
   c. If `nums[mid] < target`, set `left = mid + 1` (search right half).
   d. If `nums[mid] > target`, set `right = mid - 1` (search left half).
3. If the loop ends without returning, `target` is not in the array — return `-1`.

## Solution

```python
from typing import List

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = left + (right - left) // 2

            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1

        return -1
```

## Complexity

- **Time:** O(log n) — the search space is halved on every iteration.
- **Space:** O(1) — only a constant number of pointer variables are used.
