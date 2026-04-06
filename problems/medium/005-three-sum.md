---
id: 15
title: 3Sum
difficulty: medium
category: two-pointers
tags: [array, two-pointers, sorting]
leetcode_url: https://leetcode.com/problems/3sum/
---

## Problem

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

The solution set must **not contain duplicate triplets**.

**Example 1:**
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

**Example 2:**
```
Input: nums = [0,1,1]
Output: []
```

**Example 3:**
```
Input: nums = [0,0,0]
Output: [[0,0,0]]
```

**Constraints:**
- `3 <= nums.length <= 3000`
- `-10^5 <= nums[i] <= 10^5`

## Hint

If you sort the array first, you can fix one element at a time and reduce the remaining problem to Two Sum II — finding a pair in a sorted subarray. The sorted order also makes it straightforward to skip duplicates.

## Approach

Sort the array, then iterate over each element as the fixed first number. For each fixed element, use two pointers starting at the next position and the end of the array to find pairs that complete the triplet. Skip duplicate values at the outer loop and after finding a valid triplet to avoid duplicate results.

## Walkthrough

1. Sort `nums` in non-decreasing order.
2. Iterate with index `i` from `0` to `len(nums) - 3`.
3. If `nums[i] > 0`, break early — no triplet can sum to zero when all remaining values are positive.
4. If `i > 0` and `nums[i] == nums[i - 1]`, skip to avoid processing the same first element twice.
5. Set `left = i + 1` and `right = len(nums) - 1`.
6. While `left < right`, compute `total = nums[i] + nums[left] + nums[right]`.
7. If `total == 0`, record the triplet, then advance `left` past duplicates and retreat `right` past duplicates before continuing.
8. If `total < 0`, increment `left` to increase the sum.
9. If `total > 0`, decrement `right` to decrease the sum.
10. Return all collected triplets.

## Solution

```python
from typing import List

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        result = []

        for i in range(len(nums) - 2):
            if nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left, right = i + 1, len(nums) - 1
            while left < right:
                total = nums[i] + nums[left] + nums[right]
                if total == 0:
                    result.append([nums[i], nums[left], nums[right]])
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    left += 1
                    right -= 1
                elif total < 0:
                    left += 1
                else:
                    right -= 1

        return result
```

## Complexity

- **Time:** O(n^2) — sorting is O(n log n), and the outer loop with the inner two-pointer scan is O(n^2) overall.
- **Space:** O(1) extra — sorting is in-place; the output list is not counted as extra space.
