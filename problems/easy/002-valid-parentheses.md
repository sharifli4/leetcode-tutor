---
id: 20
title: Valid Parentheses
difficulty: easy
category: stacks
tags: [string, stack]
leetcode_url: https://leetcode.com/problems/valid-parentheses/
---

## Problem

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

A string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**
```
Input: s = "()"
Output: true
```

**Example 2:**
```
Input: s = "()[]{}"
Output: true
```

**Example 3:**
```
Input: s = "(]"
Output: false
```

**Constraints:**
- `1 <= s.length <= 10^4`
- `s` consists of parentheses only `'()[]{}'`.

## Hint

When you encounter a closing bracket, you need to know what the most recent unmatched opening bracket was. Think about which data structure naturally gives you the "most recently added" item.

## Approach

Use a stack to track unmatched opening brackets. When an opening bracket is seen, push it. When a closing bracket is seen, check that the top of the stack holds the matching opener — if so, pop it; otherwise the string is invalid. After processing all characters, the string is valid only if the stack is empty.

## Walkthrough

1. Create an empty list `stack` to act as a stack.
2. Build a mapping from each closing bracket to its matching opener: `{')': '(', ']': '[', '}': '{'}`.
3. Iterate over each character `c` in `s`.
4. If `c` is an opening bracket (`(`, `[`, `{`), push it onto the stack.
5. If `c` is a closing bracket, check whether the stack is non-empty **and** the top equals the expected opener.
6. If both conditions hold, pop the top of the stack.
7. Otherwise, return `False` immediately.
8. After the loop, return `True` if the stack is empty (all openers were matched), `False` otherwise.

## Solution

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        closing_to_opening = {')': '(', ']': '[', '}': '{'}

        for c in s:
            if c in closing_to_opening:
                # It's a closing bracket
                if stack and stack[-1] == closing_to_opening[c]:
                    stack.pop()
                else:
                    return False
            else:
                # It's an opening bracket
                stack.append(c)

        return len(stack) == 0
```

## Complexity

- **Time:** O(n) — each character is pushed or popped at most once.
- **Space:** O(n) — in the worst case (all opening brackets) the stack holds n elements.
