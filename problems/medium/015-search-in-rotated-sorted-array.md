---
id: 33
title: Search in Rotated Sorted Array
difficulty: medium
category: binary-search
tags: [array, binary-search]
leetcode_url: https://leetcode.com/problems/search-in-rotated-sorted-array/
---

## Problem

There is an integer array `nums` sorted in ascending order (with **distinct** values). Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`.

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**
```
Input: nums = [1], target = 0
Output: -1
```

**Constraints:**
- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- All values of `nums` are **unique**.
- `nums` is an ascending array that is possibly rotated.
- `-10^4 <= target <= 10^4`

## Hint

In a rotated sorted array, when you pick a midpoint, at least one of the two halves is guaranteed to be fully sorted. Can you figure out which half is sorted, and then decide if the target falls within that sorted half?

## Approach

Use binary search, but at each step determine which half of the current window is sorted. If the left half is sorted and the target lies within its range, search left; otherwise search right. Apply the same logic if the right half is sorted.

## Walkthrough

1. Set `left = 0` and `right = len(nums) - 1`.
2. Compute `mid = (left + right) // 2`.
3. If `nums[mid] == target`, return `mid`.
4. Determine which half is sorted by comparing `nums[left]` with `nums[mid]`:
   - If `nums[left] <= nums[mid]`, the **left half** is sorted.
     - If `nums[left] <= target < nums[mid]`, the target is in the left half: set `right = mid - 1`.
     - Otherwise, search right: set `left = mid + 1`.
   - Otherwise, the **right half** is sorted.
     - If `nums[mid] < target <= nums[right]`, the target is in the right half: set `left = mid + 1`.
     - Otherwise, search left: set `right = mid - 1`.
5. Repeat from step 2 while `left <= right`.
6. Return `-1` if the target is not found.

## Solution

```python
from typing import List

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid

            # Left half is sorted
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            # Right half is sorted
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```

## Complexity

- **Time:** O(log n) — each iteration halves the search space, same as standard binary search.
- **Space:** O(1) — only a constant number of index variables are used.
