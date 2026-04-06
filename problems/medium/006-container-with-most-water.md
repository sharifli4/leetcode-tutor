---
id: 11
title: Container With Most Water
difficulty: medium
category: two-pointers
tags: [array, two-pointers, greedy]
leetcode_url: https://leetcode.com/problems/container-with-most-water/
---

## Problem

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i`th line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

**Note:** You may not slant the container.

**Example 1:**
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The lines at indices 1 and 8 (heights 8 and 7) form the container.
             Width = 8 - 1 = 7, min height = 7, area = 49.
```

**Example 2:**
```
Input: height = [1,1]
Output: 1
```

**Example 3:**
```
Input: height = [4,3,2,1,4]
Output: 16
```

**Constraints:**
- `n == height.length`
- `2 <= n <= 10^5`
- `0 <= height[i] <= 10^4`

## Hint

Start with the widest possible container (leftmost and rightmost lines). The area is limited by the shorter of the two walls. If you move the taller wall inward, the width decreases and the height can only stay the same or decrease — can you do better? What about moving the shorter wall?

## Approach

Use two pointers starting at opposite ends of the array. At each step, compute the current area (width times the shorter height) and update the maximum. Move the pointer pointing to the shorter line inward — this is the only move that could possibly find a larger area.

## Walkthrough

1. Set `left = 0` and `right = len(height) - 1`.
2. Initialize `max_area = 0`.
3. While `left < right`:
   a. Compute `width = right - left`.
   b. Compute `current_area = width * min(height[left], height[right])`.
   c. Update `max_area = max(max_area, current_area)`.
   d. If `height[left] <= height[right]`, increment `left`; otherwise decrement `right`.
4. Return `max_area`.

## Solution

```python
from typing import List

class Solution:
    def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1
        max_area = 0

        while left < right:
            width = right - left
            current_area = width * min(height[left], height[right])
            max_area = max(max_area, current_area)

            if height[left] <= height[right]:
                left += 1
            else:
                right -= 1

        return max_area
```

## Complexity

- **Time:** O(n) — each pointer moves at most n steps in total, so the while loop runs at most n times.
- **Space:** O(1) — only a constant number of variables are used.
