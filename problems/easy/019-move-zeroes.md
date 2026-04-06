---
id: 283
title: Move Zeroes
difficulty: easy
category: two-pointers
tags: [array, two pointers, in-place]
leetcode_url: https://leetcode.com/problems/move-zeroes/
---

## Problem

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note:** You must do this in-place without making a copy of the array.

**Example 1:**
```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Example 2:**
```
Input: nums = [0]
Output: [0]
```

**Example 3:**
```
Input: nums = [1,0,1,0,1]
Output: [1,1,1,0,0]
```

**Constraints:**
- `1 <= nums.length <= 10^4`
- `-2^31 <= nums[i] <= 2^31 - 1`

## Hint

Imagine a slow pointer that tracks the position where the next non-zero element should be placed, and a fast pointer that scans ahead looking for non-zero elements. What happens when the fast pointer finds one?

## Approach

Use two pointers: a write pointer (`pos`) and a read pointer that iterates through the array. Whenever the read pointer finds a non-zero value, write it to `pos` and advance `pos`. After all non-zero values have been written to the front, fill the remaining positions from `pos` to the end of the array with zeros. This makes exactly one pass to compact non-zeros and one partial pass to fill zeros.

## Walkthrough

1. Initialize `pos = 0` — this is the index where the next non-zero value should be placed.
2. Iterate through `nums` with index `i` from 0 to `len(nums) - 1`.
3. For each element: if `nums[i] != 0`, copy it to `nums[pos]` and increment `pos`.
4. After the loop, `nums[0:pos]` contains all non-zero elements in their original relative order.
5. Fill `nums[pos:]` with zeros: iterate from `pos` to the end and set each element to `0`.
6. The array is now modified in-place with zeros at the end.

## Solution

```python
def moveZeroes(nums: list) -> None:
    pos = 0

    # Write all non-zero elements to the front
    for i in range(len(nums)):
        if nums[i] != 0:
            nums[pos] = nums[i]
            pos += 1

    # Fill the rest with zeros
    for i in range(pos, len(nums)):
        nums[i] = 0
```

## Complexity

- **Time:** O(n) — two sequential single passes over the array.
- **Space:** O(1) — the array is modified in-place with no auxiliary data structures.
