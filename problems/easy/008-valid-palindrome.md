---
id: 125
title: Valid Palindrome
difficulty: easy
category: two-pointers
tags: [two-pointers, string]
leetcode_url: https://leetcode.com/problems/valid-palindrome/
---

## Problem

A phrase is a **palindrome** if, after converting all uppercase letters to lowercase and removing all non-alphanumeric characters, it reads the same forward and backward.

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

**Example 1:**
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: After cleaning: "amanaplanacanalpanama" — reads the same forwards and backwards.
```

**Example 2:**
```
Input: s = "race a car"
Output: false
Explanation: After cleaning: "raceacar" — not a palindrome.
```

**Example 3:**
```
Input: s = " "
Output: true
Explanation: After removing non-alphanumeric characters, the string is empty, which reads the same forwards and backwards.
```

**Constraints:**
- `1 <= s.length <= 2 * 10^5`
- `s` consists only of printable ASCII characters.

## Hint

Place one pointer at the start of the string and another at the end. Skip any characters that aren't letters or digits, then compare what's left. What condition tells you the check is complete?

## Approach

Use two pointers starting at opposite ends of the string. Skip non-alphanumeric characters by advancing the relevant pointer inward. Once both pointers land on alphanumeric characters, compare them (case-insensitively). If they differ the string is not a palindrome; if they match, move both pointers inward and repeat until the pointers cross.

## Walkthrough

1. Initialize `left = 0` and `right = len(s) - 1`.
2. While `left < right`:
   a. Advance `left` rightward while `left < right` and `s[left]` is not alphanumeric.
   b. Advance `right` leftward while `left < right` and `s[right]` is not alphanumeric.
   c. If `s[left].lower() != s[right].lower()`, return `False`.
   d. Otherwise, increment `left` and decrement `right`.
3. If the loop completes without mismatch, return `True`.

## Solution

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1

        while left < right:
            # Skip non-alphanumeric from the left
            while left < right and not s[left].isalnum():
                left += 1
            # Skip non-alphanumeric from the right
            while left < right and not s[right].isalnum():
                right -= 1

            if s[left].lower() != s[right].lower():
                return False

            left += 1
            right -= 1

        return True
```

## Complexity

- **Time:** O(n) — each character is examined at most once by one of the two pointers.
- **Space:** O(1) — only two integer pointers are used; no copy of the string is made.
