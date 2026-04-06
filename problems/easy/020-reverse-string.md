---
id: 344
title: Reverse String
difficulty: easy
category: two-pointers
tags: [two pointers, string, in-place, recursion]
leetcode_url: https://leetcode.com/problems/reverse-string/
---

## Problem

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array in-place with O(1) extra memory.

**Example 1:**
```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

**Example 2:**
```
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

**Constraints:**
- `1 <= s.length <= 10^5`
- `s[i]` is a printable ASCII character.

## Hint

Place one pointer at the start and another at the end of the array. What happens if you swap those two characters and move the pointers toward each other?

## Approach

Use two pointers starting at opposite ends of the array. Swap the characters at the left and right pointers, then advance the left pointer forward and the right pointer backward. Continue until the two pointers meet (or cross) in the middle. This performs exactly `n // 2` swaps and requires no extra space.

## Walkthrough

1. Initialize `left = 0` and `right = len(s) - 1`.
2. Enter a loop that runs while `left < right`.
3. Swap `s[left]` and `s[right]` using Python's simultaneous assignment: `s[left], s[right] = s[right], s[left]`.
4. Increment `left` by 1 and decrement `right` by 1.
5. Repeat until the pointers meet in the middle.
6. The array `s` is now reversed in-place. No return value is needed (modification is in-place).

## Solution

```python
def reverseString(s: list) -> None:
    left, right = 0, len(s) - 1

    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
```

## Complexity

- **Time:** O(n) — we perform exactly n/2 swaps, visiting each character once.
- **Space:** O(1) — the swap is done in-place; only two pointer variables are used.
