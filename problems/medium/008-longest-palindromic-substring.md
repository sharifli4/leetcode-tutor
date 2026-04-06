---
id: 5
title: Longest Palindromic Substring
difficulty: medium
category: strings
tags: [string, dynamic-programming, two-pointers]
leetcode_url: https://leetcode.com/problems/longest-palindromic-substring/
---

## Problem

Given a string `s`, return the **longest palindromic substring** in `s`.

**Example 1:**
```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**
```
Input: s = "cbbd"
Output: "bb"
```

**Example 3:**
```
Input: s = "racecar"
Output: "racecar"
```

**Constraints:**
- `1 <= s.length <= 1000`
- `s` consists of only digits and English letters.

## Hint

Every palindrome has a center — either a single character (for odd-length palindromes) or a pair of identical characters (for even-length palindromes). Try expanding outward from each possible center and see how far you can go.

## Approach

Expand around center: for each index in the string, attempt to grow a palindrome outward for both odd-length (center is a single character) and even-length (center is between two characters) cases. Keep track of the start and end of the longest palindrome found so far.

## Walkthrough

1. If `s` is empty, return `""`.
2. Initialize `start = 0` and `end = 0` to track the best palindrome's indices (inclusive).
3. Define a helper `expand(left, right)` that expands as long as `left >= 0`, `right < len(s)`, and `s[left] == s[right]`. Return the `(left, right)` of the widest palindrome found.
4. For each index `i` in `s`:
   a. Expand for odd-length palindrome: `l1, r1 = expand(i, i)`.
   b. Expand for even-length palindrome: `l2, r2 = expand(i, i + 1)`.
   c. For each result, if `r - l > end - start`, update `start = l` and `end = r`.
5. Return `s[start:end + 1]`.

## Solution

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if not s:
            return ""

        def expand(left: int, right: int):
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            # After the loop, s[left+1:right] is the palindrome
            return left + 1, right - 1

        start, end = 0, 0

        for i in range(len(s)):
            l1, r1 = expand(i, i)       # odd-length
            l2, r2 = expand(i, i + 1)   # even-length

            if r1 - l1 > end - start:
                start, end = l1, r1
            if r2 - l2 > end - start:
                start, end = l2, r2

        return s[start:end + 1]
```

## Complexity

- **Time:** O(n^2) — for each of the n centers, the expansion can take up to O(n) steps in the worst case.
- **Space:** O(1) — only a constant number of index variables are used.
