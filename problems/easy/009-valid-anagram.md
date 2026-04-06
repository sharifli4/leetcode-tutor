---
id: 242
title: Valid Anagram
difficulty: easy
category: strings
tags: [hash-table, string, sorting]
leetcode_url: https://leetcode.com/problems/valid-anagram/
---

## Problem

Given two strings `s` and `t`, return `true` if `t` is an **anagram** of `s`, and `false` otherwise.

An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, using all the original letters exactly once.

**Example 1:**
```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**
```
Input: s = "rat", t = "car"
Output: false
```

**Example 3:**
```
Input: s = "listen", t = "silent"
Output: true
```

**Constraints:**
- `1 <= s.length, t.length <= 5 * 10^4`
- `s` and `t` consist of lowercase English letters.

## Hint

Two strings are anagrams if and only if they contain exactly the same characters in the same quantities. How could you count the frequency of each character in both strings and compare the results efficiently?

## Approach

Count the frequency of each character in both strings using a hash map (or Python's `Counter`). If the two frequency maps are equal, the strings are anagrams. As a quick early exit, if the strings have different lengths they cannot be anagrams.

## Walkthrough

1. If `len(s) != len(t)`, return `False` immediately.
2. Create a dictionary `count` to track the character balance.
3. Iterate over both strings simultaneously (zip them together, or use two loops):
   a. Increment `count[char]` for each character in `s`.
   b. Decrement `count[char]` for each character in `t`.
4. After processing, check every value in `count`.
5. If all values are `0`, every character appears the same number of times in both strings — return `True`.
6. If any value is non-zero, return `False`.

## Solution

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        count = {}

        for char in s:
            count[char] = count.get(char, 0) + 1

        for char in t:
            count[char] = count.get(char, 0) - 1

        return all(v == 0 for v in count.values())
```

## Complexity

- **Time:** O(n) — two linear passes (one per string) plus a final O(k) check over at most 26 distinct letters, where n is the string length.
- **Space:** O(k) — the dictionary holds at most k distinct characters (k ≤ 26 for lowercase English letters), effectively O(1) for a fixed alphabet.
