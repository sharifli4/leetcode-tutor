---
id: 1
title: Two Sum
difficulty: easy
category: arrays-hashing
tags: [array, hash-table]
leetcode_url: https://leetcode.com/problems/two-sum/
---

## Problem

Given an array of integers `nums` and an integer `target`, return the **indices** of the two numbers that add up to `target`.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order.

**Example 1:**
```
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
Explanation: nums[0] + nums[1] = 2 + 7 = 9
```

**Example 2:**
```
Input: nums = [3, 2, 4], target = 6
Output: [1, 2]
```

**Example 3:**
```
Input: nums = [3, 3], target = 6
Output: [0, 1]
```

**Constraints:**
- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`
- Only one valid answer exists.

## Hint

For each number you visit, think about what value you *need* to complete the pair. Is there a data structure that lets you check in O(1) whether you've already seen that needed value?

## Approach

Use a hash map to store each number and its index as you iterate through the array. For every element, compute its complement (`target - nums[i]`) and check if that complement already exists in the map. If it does, you've found your pair; if not, add the current element to the map and continue.

## Walkthrough

1. Create an empty dictionary `seen` that maps a number's value to its index.
2. Loop through `nums` with both index `i` and value `num`.
3. Compute `complement = target - num`.
4. Check if `complement` is already a key in `seen`.
5. If yes, return `[seen[complement], i]` — those are the two indices.
6. If no, store `seen[num] = i` and move to the next element.
7. Because the problem guarantees exactly one solution, we will always find the answer before the loop ends.

## Solution

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # value -> index
        for i, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [seen[complement], i]
            seen[num] = i
        return []  # guaranteed not reached
```

## Complexity

- **Time:** O(n) — we iterate through the array once; each hash map lookup and insertion is O(1) on average.
- **Space:** O(n) — in the worst case the map stores all n elements before finding the answer.
