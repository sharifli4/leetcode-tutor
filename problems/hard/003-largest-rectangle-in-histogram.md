---
id: 84
title: Largest Rectangle in Histogram
difficulty: hard
category: stacks
tags: [array, stack, monotonic-stack]
leetcode_url: https://leetcode.com/problems/largest-rectangle-in-histogram/
---

## Problem

Given an array of integers `heights` representing the histogram's bar heights where the width of each bar is 1, return the area of the largest rectangle in the histogram.

**Example 1:**
```
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The largest rectangle has area 10 (bars at indices 2 and 3, each of height 5 and 6,
             but bounded by height 5 for width 2 → 5*2=10).
```

**Example 2:**
```
Input: heights = [2,4]
Output: 4
```

**Example 3:**
```
Input: heights = [6,7,5,2,4,5,9,3]
Output: 16
```

**Constraints:**
- `1 <= heights.length <= 10^5`
- `0 <= heights[i] <= 10^4`

## Hint

A rectangle anchored at bar `i` can extend left and right only as far as every bar in that span is at least as tall as `heights[i]`. Think about a stack that keeps track of "bars that haven't been blocked yet" — what does it mean when a shorter bar arrives?

## Approach

Maintain a monotonic increasing stack of `(height, start_index)` pairs. When a bar shorter than the stack's top is encountered, every taller bar on the stack can no longer extend to the right, so pop and compute their areas. After the main loop, flush any remaining entries in the stack, extending each to the right end of the array. Track the global maximum throughout.

## Walkthrough

1. Initialize an empty stack and `max_area = 0`.
2. Iterate over each index `i` and `h = heights[i]`.
3. Set `start = i` — this will track how far left the current bar can extend.
4. While the stack is non-empty and the top's height is greater than `h`:
   - Pop `(top_height, top_start)` from the stack.
   - Compute `area = top_height * (i - top_start)` — the rectangle extends from `top_start` to just before `i`.
   - Update `max_area` if this area is larger.
   - Update `start = top_start` — the current bar `h` can extend back to wherever the popped bar started.
5. Push `(h, start)` onto the stack.
6. After the loop, flush remaining stack entries: for each `(top_height, top_start)`, compute `area = top_height * (len(heights) - top_start)`.
7. Return `max_area`.

## Solution

```python
from typing import List

class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        stack = []  # (height, start_index)
        max_area = 0

        for i, h in enumerate(heights):
            start = i
            # Pop all bars taller than the current bar
            while stack and stack[-1][0] > h:
                top_height, top_start = stack.pop()
                area = top_height * (i - top_start)
                max_area = max(max_area, area)
                start = top_start  # current bar extends back to this position

            stack.append((h, start))

        # Flush remaining bars — they extend to the right end
        n = len(heights)
        for top_height, top_start in stack:
            area = top_height * (n - top_start)
            max_area = max(max_area, area)

        return max_area
```

## Complexity

- **Time:** O(n) — each bar is pushed and popped from the stack at most once.
- **Space:** O(n) — in the worst case (strictly increasing heights) the stack holds all n bars.
