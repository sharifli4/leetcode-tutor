---
id: 300
title: Longest Increasing Subsequence
difficulty: medium
category: dynamic-programming
tags: [array, binary-search, dynamic-programming]
leetcode_url: https://leetcode.com/problems/longest-increasing-subsequence/
---

## Problem

Given an integer array `nums`, return the length of the longest **strictly increasing subsequence**.

A **subsequence** is a sequence derived from the array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**
```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The LIS is [2,3,7,101] or [2,5,7,101], length 4.
```

**Example 2:**
```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

**Example 3:**
```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

**Constraints:**
- `1 <= nums.length <= 2500`
- `-10^4 <= nums[i] <= 10^4`

**Follow up:** Can you come up with an algorithm that runs in `O(n log n)` time complexity?

## Hint

For the O(n²) approach: `dp[i]` should represent the length of the LIS ending at index `i`. For the O(n log n) approach: maintain a list that represents the smallest possible tail of all increasing subsequences of each length found so far.

## Approach

The O(n log n) approach maintains a `tails` list where `tails[i]` holds the smallest tail element of all increasing subsequences of length `i + 1`. For each new number, use binary search to find where it fits in `tails` — either extending the list or replacing an existing value to keep the tails as small as possible.

## Walkthrough

1. Initialize an empty list `tails`.
2. For each number `num` in `nums`:
   a. Use `bisect_left` to find the leftmost index `pos` in `tails` where `tails[pos] >= num`.
   b. If `pos == len(tails)`, `num` extends the longest subsequence found so far: append it.
   c. Otherwise, replace `tails[pos]` with `num` — a smaller tail value for a subsequence of the same length gives more room to extend later.
3. The answer is `len(tails)`.

## Solution

```python
from typing import List
import bisect

class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        tails = []

        for num in nums:
            pos = bisect.bisect_left(tails, num)
            if pos == len(tails):
                tails.append(num)
            else:
                tails[pos] = num

        return len(tails)
```

## Complexity

- **Time:** O(n log n) — for each of the n elements we perform a binary search over `tails` which has at most n entries.
- **Space:** O(n) — the `tails` list stores at most n elements in the worst case.
