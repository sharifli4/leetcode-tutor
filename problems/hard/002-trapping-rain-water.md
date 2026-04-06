---
id: 42
title: Trapping Rain Water
difficulty: hard
category: two-pointers
tags: [array, two-pointers, dynamic-programming, stack, monotonic-stack]
leetcode_url: https://leetcode.com/problems/trapping-rain-water/
---

## Problem

Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

**Example 1:**
```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The bars trap 6 units of water (visualize the elevation profile with water filling the valleys).
```

**Example 2:**
```
Input: height = [4,2,0,3,2,5]
Output: 9
```

**Example 3:**
```
Input: height = [3,0,2,0,4]
Output: 7
```

**Constraints:**
- `n == height.length`
- `1 <= n <= 2 * 10^4`
- `0 <= height[i] <= 10^5`

## Hint

The water level above any position is determined by the shorter of the tallest bars to its left and right. Think about whether you really need to know both sides simultaneously — or whether you can make progress as soon as you're certain about one side.

## Approach

Use two pointers starting at the left and right ends of the array, each tracking the maximum height seen so far on their respective side. At each step, process whichever side has the smaller max: the water at that position is `(max_so_far - current_height)`, because the taller opposite side is guaranteed to contain it. Advance the pointer inward and update the running max.

## Walkthrough

1. Initialize `left = 0`, `right = len(height) - 1`, `left_max = 0`, `right_max = 0`, and `water = 0`.
2. Loop while `left < right`.
3. Compare `height[left]` and `height[right]`.
4. **If `height[left] <= height[right]`:** the bottleneck is on the left side.
   - If `height[left] >= left_max`, update `left_max = height[left]` (no water trapped here, it raises the left wall).
   - Otherwise, add `left_max - height[left]` to `water` (left wall determines water level).
   - Advance `left += 1`.
5. **Otherwise:** the bottleneck is on the right side.
   - If `height[right] >= right_max`, update `right_max = height[right]`.
   - Otherwise, add `right_max - height[right]` to `water`.
   - Advance `right -= 1`.
6. Return `water` once `left` and `right` meet.

## Solution

```python
from typing import List

class Solution:
    def trap(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1
        left_max, right_max = 0, 0
        water = 0

        while left < right:
            if height[left] <= height[right]:
                if height[left] >= left_max:
                    left_max = height[left]
                else:
                    water += left_max - height[left]
                left += 1
            else:
                if height[right] >= right_max:
                    right_max = height[right]
                else:
                    water += right_max - height[right]
                right -= 1

        return water
```

## Complexity

- **Time:** O(n) — each element is visited at most once as the two pointers converge.
- **Space:** O(1) — only a constant number of variables are used; no auxiliary arrays needed.
