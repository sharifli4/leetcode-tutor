---
id: 32
title: Longest Valid Parentheses
difficulty: hard
category: dynamic-programming
tags: [string, dynamic-programming, stack]
leetcode_url: https://leetcode.com/problems/longest-valid-parentheses/
---

## Problem

Given a string containing just the characters `'('` and `')'`, return the length of the longest valid (well-formed) parentheses substring.

**Example 1:**
```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
```

**Example 2:**
```
Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".
```

**Example 3:**
```
Input: s = ""
Output: 0
```

**Example 4:**
```
Input: s = "()(())"
Output: 6
```

**Constraints:**
- `0 <= s.length <= 3 * 10^4`
- `s[i]` is `'('` or `')'`.

## Hint

Think about using a stack to track indices rather than characters. When do you know a valid pair has just closed? And what does the current stack top tell you about where the most recent unmatched character sits — and therefore how long the current valid run is?

## Approach

Maintain a stack initialized with a sentinel value `-1` (representing the index just before any valid substring starts). Push `(` indices onto the stack. When a `)` arrives, pop the top: if the stack becomes empty, push the current index as the new sentinel (this `)` is unmatched); otherwise the length of the current valid substring is `i - stack[-1]`. Track the global maximum throughout.

## Walkthrough

1. Initialize `stack = [-1]` and `max_len = 0`.
2. Iterate through the string with index `i` and character `c`.
3. **If `c == '('`:** push `i` onto the stack.
4. **If `c == ')'`:**
   - Pop the top of the stack (it's either a matching `(` index or the previous sentinel).
   - If the stack is now **empty**, this `)` is unmatched — push `i` as the new sentinel (base reference for future valid substrings).
   - If the stack is **not empty**, the valid substring extends from just after `stack[-1]` to `i`. Compute `length = i - stack[-1]` and update `max_len`.
5. Return `max_len` after processing all characters.

**Why this works:** The sentinel on the stack always holds the index of the last unmatched `)`. The distance from the current index to that sentinel is exactly the length of the contiguous valid substring ending at `i`.

## Solution

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        stack = [-1]  # sentinel: index of last unmatched ')'
        max_len = 0

        for i, c in enumerate(s):
            if c == '(':
                stack.append(i)
            else:  # c == ')'
                stack.pop()
                if not stack:
                    # This ')' is unmatched; it becomes the new base
                    stack.append(i)
                else:
                    # Valid substring from stack[-1]+1 to i (inclusive)
                    max_len = max(max_len, i - stack[-1])

        return max_len
```

## Complexity

- **Time:** O(n) — the string is scanned once; each index is pushed and popped at most once.
- **Space:** O(n) — in the worst case (all `(`) every index is on the stack simultaneously.
