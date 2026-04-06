---
id: 128
title: Longest Consecutive Sequence
difficulty: medium
category: arrays-hashing
tags: [array, hash-table, union-find]
leetcode_url: https://leetcode.com/problems/longest-consecutive-sequence/
---

## Problem

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

**Example 1:**
```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive sequence is [1, 2, 3, 4].
```

**Example 2:**
```
Input: nums = [0,3,7,2,5,8,1,6,0,4]
Output: 9
Explanation: The longest consecutive sequence is [0, 1, 2, 3, 4, 5, 6, 7, 8].
```

**Example 3:**
```
Input: nums = [1,0,-1]
Output: 3
```

**Constraints:**
- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

## Hint

Sorting would solve this in O(n log n), but you need O(n). Think about which numbers are the *start* of a consecutive sequence — a number is a sequence start if the number one less than it is not present. Can you use a set to check that in O(1)?

## Approach

Put all numbers into a hash set for O(1) lookup. Then iterate over the set and, for each number that has no predecessor (`num - 1` is not in the set), expand the sequence forward as far as possible, counting its length. Track the maximum length seen across all such starting points.

## Walkthrough

1. Convert `nums` to a set `num_set` to eliminate duplicates and enable O(1) membership tests.
2. Initialize `best = 0`.
3. For each `num` in `num_set`, check whether `num - 1` is in `num_set`.
4. If `num - 1` is present, skip — `num` is not a sequence start.
5. If `num - 1` is absent, `num` is the start of a sequence. Set `current = num` and `length = 1`.
6. While `current + 1` is in `num_set`, increment `current` and `length`.
7. Update `best = max(best, length)`.
8. Return `best`.

## Solution

```python
from typing import List

class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        num_set = set(nums)
        best = 0

        for num in num_set:
            # Only start counting from the beginning of a sequence
            if num - 1 not in num_set:
                current = num
                length = 1
                while current + 1 in num_set:
                    current += 1
                    length += 1
                best = max(best, length)

        return best
```

## Complexity

- **Time:** O(n) — building the set is O(n); although we have a nested while loop, each number is visited at most twice (once as a potential start, once while expanding), so the total work is O(n).
- **Space:** O(n) — the hash set stores all unique elements.
