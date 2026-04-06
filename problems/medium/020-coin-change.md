---
id: 322
title: Coin Change
difficulty: medium
category: dynamic-programming
tags: [array, dynamic-programming, breadth-first-search]
leetcode_url: https://leetcode.com/problems/coin-change/
---

## Problem

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**
```
Input: coins = [1,5,11], amount = 15
Output: 3
Explanation: 11 + 3*1 = 14... actually 5 + 5 + 5 = 15 — 3 coins.
```

**Example 2:**
```
Input: coins = [1,5,11], amount = 11
Output: 1
Explanation: A single 11-coin covers the amount exactly.
```

**Example 3:**
```
Input: coins = [2], amount = 3
Output: -1
```

**Constraints:**
- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 2^31 - 1`
- `0 <= amount <= 10^4`

## Hint

Think about it sub-problem by sub-problem: what is the minimum number of coins to make each smaller amount from 0 up to `amount`? Each sub-problem's answer can build on previously computed answers.

## Approach

Build a bottom-up DP table `dp` where `dp[i]` is the minimum number of coins needed to make amount `i`. Initialize `dp[0] = 0` (no coins for zero amount) and all other entries to infinity. For each amount from 1 to `amount`, try subtracting each coin and take the minimum.

## Walkthrough

1. Create `dp` array of size `amount + 1` filled with `float('inf')`.
2. Set `dp[0] = 0` — base case: zero coins needed for zero amount.
3. For each `i` from `1` to `amount` (inclusive):
   a. For each `coin` in `coins`:
      - If `coin <= i` and `dp[i - coin] != float('inf')`, update `dp[i] = min(dp[i], dp[i - coin] + 1)`.
4. After the loops, if `dp[amount]` is still `float('inf')`, return `-1`.
5. Otherwise, return `dp[amount]`.

## Solution

```python
from typing import List

class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount + 1)
        dp[0] = 0

        for i in range(1, amount + 1):
            for coin in coins:
                if coin <= i and dp[i - coin] != float('inf'):
                    dp[i] = min(dp[i], dp[i - coin] + 1)

        return dp[amount] if dp[amount] != float('inf') else -1
```

## Complexity

- **Time:** O(amount * len(coins)) — we fill each of the `amount` cells by checking every coin denomination.
- **Space:** O(amount) — the DP table has `amount + 1` entries.
