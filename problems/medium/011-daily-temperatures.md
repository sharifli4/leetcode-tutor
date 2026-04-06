---
id: 739
title: Daily Temperatures
difficulty: medium
category: stacks
tags: [array, stack, monotonic-stack]
leetcode_url: https://leetcode.com/problems/daily-temperatures/
---

## Problem

Given an array of integers `temperatures` representing the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `i`th day to get a warmer temperature. If there is no future day with a warmer temperature, `answer[i] = 0`.

**Example 1:**
```
Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]
```

**Example 2:**
```
Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]
```

**Example 3:**
```
Input: temperatures = [30,60,90]
Output: [1,1,0]
```

**Constraints:**
- `1 <= temperatures.length <= 10^5`
- `30 <= temperatures[i] <= 100`

## Hint

You need to find, for each day, the next day with a higher temperature. Think about maintaining a stack of days that are still "waiting" for a warmer day. What property should those days' temperatures have relative to each other?

## Approach

Use a monotonic decreasing stack that stores indices of days whose warmer day has not yet been found. For each new day, compare its temperature to the temperature at the index on top of the stack. While the current temperature is greater, pop the top index and record the wait time. Then push the current index onto the stack.

## Walkthrough

1. Initialize `answer = [0] * len(temperatures)`.
2. Initialize an empty `stack` that will hold indices.
3. Iterate with index `i` over `temperatures`.
4. While `stack` is not empty and `temperatures[i] > temperatures[stack[-1]]`:
   a. Pop `prev_index = stack.pop()`.
   b. Set `answer[prev_index] = i - prev_index`.
5. Push `i` onto `stack`.
6. Any indices remaining in `stack` at the end have no warmer future day; their `answer` value stays `0`.
7. Return `answer`.

## Solution

```python
from typing import List

class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)
        answer = [0] * n
        stack = []  # stores indices, temperatures are non-increasing

        for i, temp in enumerate(temperatures):
            while stack and temp > temperatures[stack[-1]]:
                prev_index = stack.pop()
                answer[prev_index] = i - prev_index
            stack.append(i)

        return answer
```

## Complexity

- **Time:** O(n) — each index is pushed onto the stack once and popped at most once, so total operations are O(n).
- **Space:** O(n) — the stack holds at most n indices in the worst case (strictly decreasing temperatures).
