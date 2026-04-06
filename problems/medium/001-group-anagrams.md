---
id: 49
title: Group Anagrams
difficulty: medium
category: arrays-hashing
tags: [array, hash-table, string, sorting]
leetcode_url: https://leetcode.com/problems/group-anagrams/
---

## Problem

Given an array of strings `strs`, group the anagrams together. You can return the answer in **any order**.

An **anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, using all the original letters exactly once.

**Example 1:**
```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example 2:**
```
Input: strs = [""]
Output: [[""]]
```

**Example 3:**
```
Input: strs = ["a"]
Output: [["a"]]
```

**Constraints:**
- `1 <= strs.length <= 10^4`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters.

## Hint

Two strings are anagrams if and only if they share the same character frequencies. Think about what you could compute from each string that would be identical for all anagrams of each other — and then use that as a dictionary key.

## Approach

Sort each string alphabetically to produce a canonical key — all anagrams will produce the same sorted key. Use a hash map from sorted key to list of original strings, appending each string to the appropriate bucket. Return the values of the map.

## Walkthrough

1. Create a `defaultdict(list)` called `groups` to hold the buckets.
2. Iterate over each string `s` in `strs`.
3. Compute the key by sorting the characters of `s` and joining them back into a string: `key = "".join(sorted(s))`.
4. Append the original string `s` to `groups[key]`.
5. After processing all strings, return `list(groups.values())`.

## Solution

```python
from typing import List
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)
        for s in strs:
            key = "".join(sorted(s))
            groups[key].append(s)
        return list(groups.values())
```

## Complexity

- **Time:** O(n * k log k) — for each of the n strings we sort its characters in O(k log k) where k is the maximum string length.
- **Space:** O(n * k) — the hash map stores all n strings with their keys.
