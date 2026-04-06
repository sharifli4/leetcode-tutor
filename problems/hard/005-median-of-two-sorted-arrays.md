---
id: 4
title: Median of Two Sorted Arrays
difficulty: hard
category: binary-search
tags: [array, binary-search, divide-and-conquer]
leetcode_url: https://leetcode.com/problems/median-of-two-sorted-arrays/
---

## Problem

Given two sorted arrays `nums1` and `nums2` of sizes `m` and `n` respectively, return the median of the two sorted arrays.

The overall run time complexity must be O(log(m + n)).

**Example 1:**
```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.0
Explanation: merged array = [1,2,3], median = 2.0
```

**Example 2:**
```
Input: nums2 = [1,2], nums2 = [3,4]
Output: 2.5
Explanation: merged array = [1,2,3,4], median = (2+3)/2 = 2.5
```

**Example 3:**
```
Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.0
```

**Constraints:**
- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-10^6 <= nums1[i], nums2[i] <= 10^6`

## Hint

The median partitions the combined array into two equal halves. If you fix the cut point in the smaller array, the cut point in the larger array is determined by the total size. Ask yourself: what property must hold between the four elements surrounding both cut points for the partition to be correct?

## Approach

Binary search on the cut position within the smaller array. A valid partition requires that every element on the left of either cut is less than or equal to every element on the right. Adjust the binary search boundaries based on a comparison of the two elements that straddle the cuts, then read off the median from the four boundary values.

## Walkthrough

1. Ensure `nums1` is the shorter array (swap if needed) to keep binary search efficient.
2. Let `m = len(nums1)`, `n = len(nums2)`. The combined left half has `half = (m + n) // 2` elements.
3. Binary search with `lo = 0`, `hi = m` (inclusive range of valid cut positions in `nums1`).
4. In each iteration, compute `i = (lo + hi) // 2` — the number of elements taken from `nums1` into the left half.
5. Compute `j = half - i` — the number of elements taken from `nums2`.
6. Identify the four boundary values:
   - `left1 = nums1[i-1]` if `i > 0` else `-inf`
   - `right1 = nums1[i]` if `i < m` else `+inf`
   - `left2 = nums2[j-1]` if `j > 0` else `-inf`
   - `right2 = nums2[j]` if `j < n` else `+inf`
7. **If `left1 <= right2` and `left2 <= right1`:** the partition is correct.
   - If `(m + n)` is odd, return `min(right1, right2)`.
   - If even, return `(max(left1, left2) + min(right1, right2)) / 2`.
8. **If `left1 > right2`:** `i` is too large — move `hi = i - 1`.
9. **Otherwise (`left2 > right1`):** `i` is too small — move `lo = i + 1`.

## Solution

```python
from typing import List

class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        # Always binary search on the shorter array
        if len(nums1) > len(nums2):
            nums1, nums2 = nums2, nums1

        m, n = len(nums1), len(nums2)
        half = (m + n) // 2
        lo, hi = 0, m

        while lo <= hi:
            i = (lo + hi) // 2  # cut point in nums1
            j = half - i        # cut point in nums2

            left1  = nums1[i - 1] if i > 0 else float('-inf')
            right1 = nums1[i]     if i < m else float('inf')
            left2  = nums2[j - 1] if j > 0 else float('-inf')
            right2 = nums2[j]     if j < n else float('inf')

            if left1 <= right2 and left2 <= right1:
                # Correct partition found
                if (m + n) % 2 == 1:
                    return float(min(right1, right2))
                else:
                    return (max(left1, left2) + min(right1, right2)) / 2.0
            elif left1 > right2:
                hi = i - 1  # too many elements from nums1
            else:
                lo = i + 1  # too few elements from nums1

        return 0.0  # guaranteed not reached given valid input
```

## Complexity

- **Time:** O(log(min(m, n))) — binary search runs on the shorter of the two arrays.
- **Space:** O(1) — only a constant number of index and boundary variables are used.
