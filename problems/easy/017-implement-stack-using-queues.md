---
id: 225
title: Implement Stack using Queues
difficulty: easy
category: stacks
tags: [stack, queue, design]
leetcode_url: https://leetcode.com/problems/implement-stack-using-queues/
---

## Problem

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:
- `void push(int x)` — Pushes element `x` to the top of the stack.
- `int pop()` — Removes the element on the top of the stack and returns it.
- `int top()` — Returns the element on the top of the stack.
- `boolean empty()` — Returns `true` if the stack is empty, `false` otherwise.

**Notes:**
- You must use only standard queue operations — push to back, peek/pop from front, size, and is empty.
- Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you only use standard queue operations.

**Example 1:**
```
Input:
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]

Output:
[null, null, null, 2, 2, false]

Explanation:
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top();   -> returns 2
myStack.pop();   -> returns 2
myStack.empty(); -> returns false
```

**Constraints:**
- `1 <= x <= 9`
- At most 100 calls will be made to `push`, `pop`, `top`, and `empty`.
- All calls to `pop` and `top` are valid.

**Follow-up:** Can you implement the stack using only one queue?

## Hint

A queue gives you the oldest element first, but a stack needs the newest element first. What if, after pushing a new element to the back, you rotated all the older elements to the back so the new element ends up at the front?

## Approach

Use the push-costly approach with a single queue (a `deque`). When pushing a new element, add it to the back of the queue, then rotate all the previously existing elements (one by one, dequeue from front and enqueue to back) so the newest element sits at the front. After this rotation, `pop` and `top` are simply dequeue from the front — O(1) operations.

## Walkthrough

1. Initialize the class with a single `deque` called `queue`.
2. **push(x):** Append `x` to the right end of the deque. Record the current size before the push as `rotate_count = len(queue) - 1`. Then dequeue from the left and enqueue to the right exactly `rotate_count` times, cycling all older elements behind the new one.
3. After the rotation, `x` is at the front of the deque (leftmost position).
4. **pop():** Popleft from the deque and return the value. Since `push` maintained the invariant that the top is always at the front, this is O(1).
5. **top():** Return `queue[0]` — the front element — without removing it.
6. **empty():** Return `len(queue) == 0`.

## Solution

```python
from collections import deque

class MyStack:

    def __init__(self):
        self.queue = deque()

    def push(self, x: int) -> None:
        self.queue.append(x)
        # Rotate all elements except the one just added to the front
        for _ in range(len(self.queue) - 1):
            self.queue.append(self.queue.popleft())

    def pop(self) -> int:
        return self.queue.popleft()

    def top(self) -> int:
        return self.queue[0]

    def empty(self) -> bool:
        return len(self.queue) == 0
```

## Complexity

- **Time:** O(n) per `push` (must rotate n-1 elements); O(1) for `pop`, `top`, and `empty`.
- **Space:** O(n) — the queue stores all n elements currently on the stack.
