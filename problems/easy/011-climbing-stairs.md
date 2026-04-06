---
id: 70
title: Climbing Stairs
difficulty: easy
category: dynamic-programming
tags: [dynamic programming, memoization, fibonacci]
leetcode_url: https://leetcode.com/problems/climbing-stairs/
---

## Problem

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**
```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**
```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

**Example 3:**
```
Input: n = 5
Output: 8
```

**Constraints:**
- `1 <= n <= 45`

## Hint

Think about what choices you have at each step. To reach step `n`, you must have come from step `n-1` or step `n-2`. How does this relate to a well-known sequence?

## Approach

This is a classic Fibonacci-style dynamic programming problem. The number of ways to reach step `n` equals the number of ways to reach step `n-1` (then take 1 step) plus the number of ways to reach step `n-2` (then take 2 steps). We can solve this bottom-up with O(1) space by tracking only the last two values.

## Walkthrough

1. Handle the base cases: there is 1 way to reach step 1 (one single step), and 2 ways to reach step 2 (1+1 or 2).
2. For each step from 3 to `n`, the number of ways equals the sum of the previous two counts.
3. Initialize two variables `prev2 = 1` (ways to reach step 1) and `prev1 = 2` (ways to reach step 2).
4. Iterate from step 3 up to and including `n`.
5. On each iteration, compute `current = prev1 + prev2`.
6. Shift the window forward: `prev2 = prev1`, `prev1 = current`.
7. After the loop finishes, `prev1` holds the answer for step `n`.
8. Return `prev1`.

## Solution

```python
def climbStairs(n: int) -> int:
    if n <= 2:
        return n

    prev2, prev1 = 1, 2
    for _ in range(3, n + 1):
        current = prev1 + prev2
        prev2 = prev1
        prev1 = current

    return prev1
```

## Complexity

- **Time:** O(n) — we iterate once from 3 to n, performing constant work each step.
- **Space:** O(1) — only two variables are kept regardless of n.
