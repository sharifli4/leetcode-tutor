---
id: 121
title: Best Time to Buy and Sell Stock
difficulty: easy
category: arrays
tags: [array, dynamic-programming, greedy]
leetcode_url: https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
---

## Problem

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`-th day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

**Example 1:**
```
Input: prices = [7, 1, 5, 3, 6, 4]
Output: 5
Explanation: Buy on day 2 (price = 1), sell on day 5 (price = 6). Profit = 6 - 1 = 5.
```

**Example 2:**
```
Input: prices = [7, 6, 4, 3, 1]
Output: 0
Explanation: Prices only decrease, so no profitable transaction is possible.
```

**Example 3:**
```
Input: prices = [2, 4, 1]
Output: 2
Explanation: Buy on day 1 (price = 2), sell on day 2 (price = 4). Profit = 4 - 2 = 2.
```

**Constraints:**
- `1 <= prices.length <= 10^5`
- `0 <= prices[i] <= 10^4`

## Hint

You must buy before you sell. As you scan left to right, try keeping track of the cheapest price you've seen so far — that's always the best candidate to have bought on.

## Approach

Scan through prices while tracking the minimum price seen so far. At each day, compute the profit if you were to sell at today's price (today's price minus the running minimum). Update the maximum profit if this is better than any previous candidate. Update the minimum price if today's price is lower.

## Walkthrough

1. Initialize `min_price` to infinity and `max_profit` to 0.
2. Iterate over each `price` in `prices`.
3. If `price < min_price`, update `min_price = price` (found a cheaper buy day).
4. Otherwise, compute `profit = price - min_price`.
5. If `profit > max_profit`, update `max_profit = profit`.
6. Continue until all prices are processed.
7. Return `max_profit` (0 if prices never increased after a low point).

## Solution

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = float('inf')
        max_profit = 0

        for price in prices:
            if price < min_price:
                min_price = price
            else:
                profit = price - min_price
                if profit > max_profit:
                    max_profit = profit

        return max_profit
```

## Complexity

- **Time:** O(n) — a single pass through the prices array.
- **Space:** O(1) — only two variables are maintained regardless of input size.
