---
id: 238
title: Product of Array Except Self
difficulty: medium
category: arrays-hashing
tags: [array, prefix-sum]
leetcode_url: https://leetcode.com/problems/product-of-array-except-self/
---

## Problem

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and **without using the division operation**.

**Example 1:**
```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
Explanation:
  answer[0] = 2*3*4 = 24
  answer[1] = 1*3*4 = 12
  answer[2] = 1*2*4 = 8
  answer[3] = 1*2*3 = 6
```

**Example 2:**
```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

**Example 3:**
```
Input: nums = [2,5,3]
Output: [15,6,10]
```

**Constraints:**
- `2 <= nums.length <= 10^5`
- `-30 <= nums[i] <= 30`
- The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

## Hint

The product of everything except `nums[i]` is the product of everything to the *left* of index `i` multiplied by the product of everything to the *right* of index `i`. Can you compute both halves efficiently?

## Approach

Build the result array using two passes. In the first pass (left to right), fill each position with the product of all elements to its left. In the second pass (right to left), multiply each position by the running product of all elements to its right. This avoids division and uses O(1) extra space beyond the output array.

## Walkthrough

1. Initialize `result` as an array of ones with the same length as `nums`.
2. First pass — left products: maintain a running `prefix = 1`. For each index `i` from left to right, set `result[i] = prefix`, then multiply `prefix` by `nums[i]`.
3. After the first pass, `result[i]` holds the product of all elements strictly to the left of `i`.
4. Second pass — right products: maintain a running `suffix = 1`. For each index `i` from right to left, multiply `result[i] *= suffix`, then multiply `suffix` by `nums[i]`.
5. After the second pass, `result[i]` holds left_product * right_product, which is the product of all elements except `nums[i]`.
6. Return `result`.

## Solution

```python
from typing import List

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        result = [1] * n

        # Fill result[i] with product of all elements to the left of i
        prefix = 1
        for i in range(n):
            result[i] = prefix
            prefix *= nums[i]

        # Multiply result[i] by product of all elements to the right of i
        suffix = 1
        for i in range(n - 1, -1, -1):
            result[i] *= suffix
            suffix *= nums[i]

        return result
```

## Complexity

- **Time:** O(n) — two linear passes over the array.
- **Space:** O(1) extra — the output array itself is not counted as extra space per the problem's convention.
