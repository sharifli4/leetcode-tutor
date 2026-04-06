---
id: 56
title: Merge Intervals
difficulty: medium
category: arrays
tags: [array, sorting]
leetcode_url: https://leetcode.com/problems/merge-intervals/
---

## Problem

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Example 1:**
```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Intervals [1,3] and [2,6] overlap, so they are merged into [1,6].
```

**Example 2:**
```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

**Example 3:**
```
Input: intervals = [[1,4],[0,4]]
Output: [[0,4]]
```

**Constraints:**
- `1 <= intervals.length <= 10^4`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 10^4`

## Hint

If you sort intervals by their start time, overlapping intervals are always adjacent. Then a single left-to-right pass can greedily merge them.

## Approach

Sort the intervals by start time. Iterate through them maintaining a `merged` result list. For each interval, if it overlaps with the last interval in `merged` (its start is at or before the last interval's end), extend the last interval's end if necessary. Otherwise, add the current interval as a new entry.

## Walkthrough

1. Sort `intervals` by the first element (start time).
2. Initialize `merged = [intervals[0]]`.
3. For each subsequent `interval` in `intervals[1:]`:
   a. Get `current_start, current_end = interval`.
   b. Get the last merged interval: `last_start, last_end = merged[-1]`.
   c. If `current_start <= last_end` (they overlap or touch):
      - Update the end of the last merged interval to `max(last_end, current_end)`.
   d. Otherwise, append `interval` to `merged` as a non-overlapping interval.
4. Return `merged`.

## Solution

```python
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        # Sort by start time
        intervals.sort(key=lambda x: x[0])

        merged = [intervals[0]]

        for current_start, current_end in intervals[1:]:
            last_end = merged[-1][1]

            if current_start <= last_end:
                # Overlapping: extend the last interval's end if needed
                merged[-1][1] = max(last_end, current_end)
            else:
                # Non-overlapping: start a new interval
                merged.append([current_start, current_end])

        return merged
```

## Complexity

- **Time:** O(n log n) — dominated by the initial sort; the merge pass is O(n).
- **Space:** O(n) — the output list holds at most n intervals (no merges possible), and the sort uses O(log n) stack space.
