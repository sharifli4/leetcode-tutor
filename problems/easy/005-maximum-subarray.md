---
id: 53
title: Maximum Subarray
difficulty: easy
category: arrays
tags: [array, dynamic-programming, divide-and-conquer]
leetcode_url: https://leetcode.com/problems/maximum-subarray/
---

## Problem

Given an integer array `nums`, find the **subarray** with the largest sum and return its sum.

A subarray is a contiguous non-empty sequence of elements within an array.

**Example 1:**
```
Input: nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Output: 6
Explanation: The subarray [4, -1, 2, 1] has the largest sum = 6.
```

**Example 2:**
```
Input: nums = [1]
Output: 1
```

**Example 3:**
```
Input: nums = [5, 4, -1, 7, 8]
Output: 23
Explanation: The subarray [5, 4, -1, 7, 8] has sum = 23.
```

**Constraints:**
- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

## Hint

As you extend a running subarray to the right, ask yourself: is it ever worth carrying forward a negative running sum? If the current running sum turns negative, what should you do with the *next* element you encounter?

## Approach

Use Kadane's algorithm: maintain a running sum of the current subarray. At each step, decide whether to extend the existing subarray or start a fresh one from the current element — whichever gives a larger value. Track the global maximum seen at any point.

## Walkthrough

1. Initialize `current_sum` to `nums[0]` and `max_sum` to `nums[0]` (handles all-negative arrays correctly).
2. Iterate over `nums` starting from index 1.
3. For each `num`, the best subarray ending here is either `num` alone, or the previous subarray extended by `num`.
4. Set `current_sum = max(num, current_sum + num)`.
5. If `current_sum > max_sum`, update `max_sum = current_sum`.
6. Continue until all elements are processed.
7. Return `max_sum`.

## Solution

```python
from typing import List

class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        current_sum = nums[0]
        max_sum = nums[0]

        for num in nums[1:]:
            current_sum = max(num, current_sum + num)
            max_sum = max(max_sum, current_sum)

        return max_sum
```

## Complexity

- **Time:** O(n) — a single pass through the array.
- **Space:** O(1) — only two integer variables are maintained.
