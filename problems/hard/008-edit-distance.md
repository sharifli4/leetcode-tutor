---
id: 72
title: Edit Distance
difficulty: hard
category: dynamic-programming
tags: [string, dynamic-programming]
leetcode_url: https://leetcode.com/problems/edit-distance/
---

## Problem

Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` into `word2`.

You have the following three operations permitted on a word:
- **Insert** a character
- **Delete** a character
- **Replace** a character

**Example 1:**
```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation:
  horse -> rorse (replace 'h' with 'r')
  rorse -> rose  (delete 'r')
  rose  -> ros   (delete 'e')
```

**Example 2:**
```
Input: word1 = "intention", word2 = "execution"
Output: 5
```

**Example 3:**
```
Input: word1 = "", word2 = "abc"
Output: 3
```

**Constraints:**
- `0 <= word1.length <= 500`
- `0 <= word2.length <= 500`
- `word1` and `word2` consist of lowercase English letters.

## Hint

Think about what it means to solve the problem for two prefixes — `word1[:i]` and `word2[:j]`. If the last characters match, no new operation is needed. If they don't, you have exactly three choices corresponding to the three allowed operations. Building up from smaller subproblems is the key.

## Approach

Use a 2D DP table where `dp[i][j]` represents the minimum edit distance between the first `i` characters of `word1` and the first `j` characters of `word2`. Base cases handle converting to/from empty strings. The recurrence either carries over `dp[i-1][j-1]` when characters match, or takes `1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])` for delete, insert, and replace respectively.

## Walkthrough

1. Let `m = len(word1)`, `n = len(word2)`. Create a `(m+1) x (n+1)` DP table.
2. **Base cases:**
   - `dp[i][0] = i` for all `i` — converting `word1[:i]` to an empty string requires `i` deletions.
   - `dp[0][j] = j` for all `j` — converting an empty string to `word2[:j]` requires `j` insertions.
3. Fill the table row by row for `i` from 1 to `m`, `j` from 1 to `n`:
4. If `word1[i-1] == word2[j-1]` (characters match):
   - `dp[i][j] = dp[i-1][j-1]` — no operation needed.
5. Otherwise (characters differ), take the minimum of three options plus 1:
   - `dp[i-1][j]` + 1 → **delete** `word1[i-1]` (align `word1[:i-1]` with `word2[:j]`)
   - `dp[i][j-1]` + 1 → **insert** `word2[j-1]` into `word1` (align `word1[:i]` with `word2[:j-1]`)
   - `dp[i-1][j-1]` + 1 → **replace** `word1[i-1]` with `word2[j-1]`
6. The answer is `dp[m][n]`.

## Solution

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)

        # dp[i][j] = min edits to convert word1[:i] -> word2[:j]
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        # Base cases: converting to/from empty string
        for i in range(m + 1):
            dp[i][0] = i  # delete all i characters
        for j in range(n + 1):
            dp[0][j] = j  # insert all j characters

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]  # characters match, no cost
                else:
                    dp[i][j] = 1 + min(
                        dp[i - 1][j],      # delete from word1
                        dp[i][j - 1],      # insert into word1
                        dp[i - 1][j - 1],  # replace in word1
                    )

        return dp[m][n]
```

## Complexity

- **Time:** O(m * n) — the entire (m+1) x (n+1) DP table is filled exactly once.
- **Space:** O(m * n) — the full table is stored; this can be reduced to O(min(m, n)) by keeping only two rows at a time.
