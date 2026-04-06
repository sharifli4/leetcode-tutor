---
id: 150
title: Evaluate Reverse Polish Notation
difficulty: medium
category: stacks
tags: [array, math, stack]
leetcode_url: https://leetcode.com/problems/evaluate-reverse-polish-notation/
---

## Problem

You are given an array of strings `tokens` that represents an arithmetic expression in **Reverse Polish Notation** (postfix notation).

Evaluate the expression and return an integer that represents the value of the expression.

**Note:**
- Valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- Division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in RPN.
- The answer and all intermediate calculations will fit in a 32-bit integer.

**Example 1:**
```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**
```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3:**
```
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
```

**Constraints:**
- `1 <= tokens.length <= 10^4`
- `tokens[i]` is either an operator `'+'`, `'-'`, `'*'`, or `'/'`, or an integer in the range `[-200, 200]`.

## Hint

In Reverse Polish Notation, every operator applies to the two most recently seen unapplied numbers. A stack is a perfect fit: push numbers, and when you see an operator, pop the top two, apply the operator, and push the result back.

## Approach

Iterate through the tokens with a stack. Push each numeric token as an integer. When an operator token is encountered, pop two values from the stack — the second pop is the left operand and the first pop is the right operand — apply the operator, and push the result. The final answer is the single remaining element on the stack.

## Walkthrough

1. Initialize an empty list `stack`.
2. For each `token` in `tokens`:
   a. If `token` is one of `{'+', '-', '*', '/'}`, pop `b = stack.pop()` (right operand) and `a = stack.pop()` (left operand).
   b. Compute the result based on the operator. For division, use `int(a / b)` to truncate toward zero (not `a // b`, which floors toward negative infinity).
   c. Push the result back onto the stack.
   d. Otherwise, push `int(token)` onto the stack.
3. Return `stack[0]` (the sole remaining element).

## Solution

```python
from typing import List

class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        operators = {'+', '-', '*', '/'}

        for token in tokens:
            if token in operators:
                b = stack.pop()
                a = stack.pop()
                if token == '+':
                    stack.append(a + b)
                elif token == '-':
                    stack.append(a - b)
                elif token == '*':
                    stack.append(a * b)
                else:  # '/'
                    # Truncate toward zero
                    stack.append(int(a / b))
            else:
                stack.append(int(token))

        return stack[0]
```

## Complexity

- **Time:** O(n) — each token is processed exactly once with O(1) stack operations.
- **Space:** O(n) — in the worst case (all numbers, no operators) the stack holds all n tokens.
