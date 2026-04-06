---
id: 8
title: String to Integer (atoi)
difficulty: medium
category: strings
tags: [string]
leetcode_url: https://leetcode.com/problems/string-to-integer-atoi/
---

## Problem

Implement the `myAtoi(s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function).

The algorithm for `myAtoi(s)` is as follows:
1. **Whitespace:** Ignore any leading whitespace.
2. **Signedness:** Determine the sign by checking whether the next character is `'-'` or `'+'`, defaulting to positive.
3. **Conversion:** Read in digits until a non-digit character or the end of the string is reached. Ignore leading zeros.
4. **Rounding:** If the integer is out of the 32-bit signed integer range `[-2^31, 2^31 - 1]`, clamp it to that range.

**Example 1:**
```
Input: s = "42"
Output: 42
```

**Example 2:**
```
Input: s = "   -042"
Output: -42
Explanation: Leading whitespace is ignored, '-' sets sign, leading zeros are ignored.
```

**Example 3:**
```
Input: s = "1337c0d3"
Output: 1337
Explanation: Digits are read until the 'c' is encountered.
```

**Example 4:**
```
Input: s = "words and 987"
Output: 0
Explanation: The first non-whitespace character 'w' is not a digit or sign, so return 0.
```

**Constraints:**
- `0 <= s.length <= 200`
- `s` consists of English letters, digits, `' '`, `'+'`, `'-'`, and `'.'`.

## Hint

Work through the string one step at a time following the four rules in order: skip spaces, read an optional sign, read digits until a non-digit appears. Handle the 32-bit overflow case last — Python integers don't overflow, so you can clamp the accumulated result at the end.

## Approach

Use an index pointer to walk through the string sequentially: first skip leading spaces, then optionally read a sign character, then accumulate digit characters into an integer. Once a non-digit is encountered (or the string ends), clamp the result to the 32-bit signed integer range and apply the sign.

## Walkthrough

1. Define the 32-bit bounds: `INT_MIN = -2**31`, `INT_MAX = 2**31 - 1`.
2. Set `i = 0`. Advance `i` past any leading space characters.
3. Determine `sign = 1`. If `s[i] == '-'`, set `sign = -1` and advance `i`. If `s[i] == '+'`, just advance `i`.
4. Initialize `result = 0`.
5. While `i < len(s)` and `s[i].isdigit()`, compute `result = result * 10 + int(s[i])` and advance `i`.
6. Apply the sign: `result *= sign`.
7. Clamp: return `max(INT_MIN, min(INT_MAX, result))`.

## Solution

```python
class Solution:
    def myAtoi(self, s: str) -> int:
        INT_MIN, INT_MAX = -(2**31), 2**31 - 1
        i = 0
        n = len(s)

        # Step 1: Skip leading whitespace
        while i < n and s[i] == ' ':
            i += 1

        # Step 2: Read optional sign
        sign = 1
        if i < n and s[i] in ('+', '-'):
            if s[i] == '-':
                sign = -1
            i += 1

        # Step 3: Read digits
        result = 0
        while i < n and s[i].isdigit():
            result = result * 10 + int(s[i])
            i += 1

        # Step 4: Apply sign and clamp
        result *= sign
        return max(INT_MIN, min(INT_MAX, result))
```

## Complexity

- **Time:** O(n) — we scan through the string at most once.
- **Space:** O(1) — only a constant number of variables are used.
