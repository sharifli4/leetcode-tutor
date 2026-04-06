---
id: 347
title: Top K Frequent Elements
difficulty: medium
category: arrays-hashing
tags: [array, hash-table, sorting, heap, bucket-sort]
leetcode_url: https://leetcode.com/problems/top-k-frequent-elements/
---

## Problem

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in **any order**.

**Example 1:**
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**
```
Input: nums = [1], k = 1
Output: [1]
```

**Example 3:**
```
Input: nums = [1,2,2,3,3,3], k = 2
Output: [2,3]
```

**Constraints:**
- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `k` is in the range `[1, the number of unique elements in the array]`.
- The answer is **guaranteed** to be unique.

## Hint

You need to rank elements by how often they appear. Once you have each element's count, think about how you could avoid a full sort — perhaps by organizing elements into buckets indexed by their frequency.

## Approach

Count the frequency of each element with a hash map, then use bucket sort: create an array of lists indexed by frequency (index = count), place each element into its frequency bucket, then read buckets from highest frequency down until k elements have been collected.

## Walkthrough

1. Build a frequency map: `count[num]` = number of times `num` appears.
2. Create a list `buckets` of length `len(nums) + 1` where index `i` holds all numbers that appear exactly `i` times.
3. For each `(num, freq)` pair in the frequency map, append `num` to `buckets[freq]`.
4. Initialize an empty result list.
5. Iterate over `buckets` from index `len(nums)` down to `1`.
6. For each non-empty bucket, add its elements to the result.
7. Stop once `len(result) == k` and return the result.

## Solution

```python
from typing import List
from collections import Counter

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)
        buckets = [[] for _ in range(len(nums) + 1)]
        for num, freq in count.items():
            buckets[freq].append(num)

        result = []
        for freq in range(len(buckets) - 1, 0, -1):
            for num in buckets[freq]:
                result.append(num)
                if len(result) == k:
                    return result
        return result
```

## Complexity

- **Time:** O(n) — counting is O(n), building buckets is O(n), and scanning buckets is O(n) in the worst case.
- **Space:** O(n) — the frequency map and buckets array together hold at most n elements.
