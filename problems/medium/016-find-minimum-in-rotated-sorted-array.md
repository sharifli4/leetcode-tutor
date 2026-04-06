---
id: 153
title: Find Minimum in Rotated Sorted Array
difficulty: medium
category: binary-search
tags: [array, binary-search]
leetcode_url: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
---

## Problem

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. Given the sorted rotated array `nums` of **unique** elements, return the minimum element of this array.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**
```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2:**
```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,3,4,5,6,7] and it was rotated 4 times.
```

**Example 3:**
```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times (no change).
```

**Constraints:**
- `n == nums.length`
- `1 <= n <= 5000`
- `-5000 <= nums[i] <= 5000`
- All the integers of `nums` are **unique**.
- `nums` is sorted and rotated between `1` and `n` times.

## Hint

The minimum element is the only one that is smaller than its predecessor. Think about how comparing `nums[mid]` to `nums[right]` tells you which side of the rotation point you are currently on.

## Approach

Use binary search comparing the midpoint value to the rightmost value. If `nums[mid] > nums[right]`, the minimum must be in the right half (we are currently in the larger, left portion of the rotation). Otherwise, the minimum is in the left half including `mid`.

## Walkthrough

1. Set `left = 0` and `right = len(nums) - 1`.
2. While `left < right`:
   a. Compute `mid = (left + right) // 2`.
   b. If `nums[mid] > nums[right]`, the rotation pivot (minimum) lies to the right of `mid`: set `left = mid + 1`.
   c. Otherwise, the minimum is at `mid` or to its left: set `right = mid`.
3. When `left == right`, both point to the minimum element.
4. Return `nums[left]`.

## Solution

```python
from typing import List

class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = (left + right) // 2

            if nums[mid] > nums[right]:
                # Minimum is in the right half
                left = mid + 1
            else:
                # Minimum is at mid or in the left half
                right = mid

        return nums[left]
```

## Complexity

- **Time:** O(log n) — the search space halves on each iteration.
- **Space:** O(1) — only index variables are used, no extra data structures.
