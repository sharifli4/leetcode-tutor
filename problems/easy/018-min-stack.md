---
id: 155
title: Min Stack
difficulty: easy
category: stacks
tags: [stack, design]
leetcode_url: https://leetcode.com/problems/min-stack/
---

## Problem

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` — Initializes the stack object.
- `void push(int val)` — Pushes the element `val` onto the stack.
- `void pop()` — Removes the element on the top of the stack.
- `int top()` — Gets the top element of the stack.
- `int getMin()` — Retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

**Example 1:**
```
Input:
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output:
[null,null,null,null,-3,null,0,-2]

Explanation:
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); -> Returns -3.
minStack.pop();
minStack.top();    -> Returns 0.
minStack.getMin(); -> Returns -2.
```

**Constraints:**
- `-2^31 <= val <= 2^31 - 1`
- Methods `pop`, `top` and `getMin` operations will always be called on non-empty stacks.
- At most `3 * 10^4` calls will be made to `push`, `pop`, `top`, and `getMin`.

## Hint

When you pop the current minimum off the stack, how do you know what the new minimum is? What if each element in the stack "remembered" what the minimum was at the time it was pushed?

## Approach

Maintain two stacks in parallel: the main stack for values, and an auxiliary `min_stack` that mirrors the main stack but each position stores the minimum value seen at that stack depth. When pushing, push the new minimum (the smaller of the new value and the current top of `min_stack`) onto `min_stack`. When popping, pop both stacks together. `getMin` is simply the top of `min_stack`.

## Walkthrough

1. Initialize two lists: `stack` (the main stack) and `min_stack` (tracks the running minimum).
2. **push(val):** Append `val` to `stack`. Compute the new minimum as `min(val, min_stack[-1])` if `min_stack` is non-empty, otherwise just `val`. Append this minimum to `min_stack`.
3. **pop():** Pop the top from both `stack` and `min_stack` simultaneously.
4. **top():** Return `stack[-1]`.
5. **getMin():** Return `min_stack[-1]` — the minimum valid for the current stack state.
6. Because `min_stack` and `stack` are always the same length, popping one element from the main stack automatically reveals the minimum that was valid before that element was pushed.

## Solution

```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        if self.min_stack:
            self.min_stack.append(min(val, self.min_stack[-1]))
        else:
            self.min_stack.append(val)

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```

## Complexity

- **Time:** O(1) for all operations — each function performs a constant number of list append/pop/index operations.
- **Space:** O(n) — the auxiliary `min_stack` stores one entry per element, doubling the space compared to a plain stack.
