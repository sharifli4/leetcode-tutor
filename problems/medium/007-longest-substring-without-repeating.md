---
id: 3
title: Longest Substring Without Repeating Characters
difficulty: medium
category: strings
tags: [hash-table, string, sliding-window]
leetcode_url: https://leetcode.com/problems/longest-substring-without-repeating-characters/
---

## Problem

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Note that "pwke" is a subsequence, not a substring.
```

**Constraints:**
- `0 <= s.length <= 5 * 10^4`
- `s` consists of English letters, digits, symbols, and spaces.

## Hint

Think of a window defined by two pointers, left and right, that slides across the string. When you add a character to the right that's already in the window, you need to shrink the window from the left until the duplicate is gone. A hash map storing each character's most recent index can help you jump the left pointer directly instead of crawling one step at a time.

## Approach

Use a sliding window with a hash map that records the last seen index of each character. Expand the window by advancing the right pointer. When a character is encountered that already exists inside the current window, jump the left pointer to one position past the previous occurrence of that character. Track the maximum window size throughout.

## Walkthrough

1. Initialize `char_index` as an empty dictionary mapping characters to their most recent index.
2. Set `left = 0` and `max_len = 0`.
3. Iterate with `right` from `0` to `len(s) - 1`, processing character `c = s[right]`.
4. If `c` is in `char_index` and `char_index[c] >= left`, the character is inside the current window — move `left = char_index[c] + 1` to exclude the duplicate.
5. Update `char_index[c] = right` to record the new position.
6. Update `max_len = max(max_len, right - left + 1)`.
7. Return `max_len`.

## Solution

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_index = {}
        left = 0
        max_len = 0

        for right, c in enumerate(s):
            if c in char_index and char_index[c] >= left:
                left = char_index[c] + 1
            char_index[c] = right
            max_len = max(max_len, right - left + 1)

        return max_len
```

## Complexity

- **Time:** O(n) — the right pointer visits each character once; the left pointer only moves forward, so total work is O(n).
- **Space:** O(min(n, a)) — the hash map holds at most as many entries as there are unique characters in the string, where `a` is the size of the character set (at most 128 for ASCII).
