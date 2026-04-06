# LeetCode Tutor

A plugin for Claude Code and Cursor that presents LeetCode problems with step-by-step walkthroughs while your AI agents work in the background.

## How It Works

```
┌─────────────────────────────────────────────────────────────────────┐
│  You: "Refactor the auth module"                                    │
│                                                                     │
│  ┌─────────────────────────┐    ┌────────────────────────────────┐  │
│  │   Background Agent      │    │   LeetCode Tutor (subagent)    │  │
│  │                         │    │                                │  │
│  │  Refactoring auth...    │    │  Here's a problem while you   │  │
│  │  Reading files...       │    │  wait!                         │  │
│  │  Updating tests...      │    │                                │  │
│  │  Running linter...      │    │  Problem → Hint → Approach     │  │
│  │  Committing...          │    │  → Solution                    │  │
│  │                         │    │                                │  │
│  │  ✓ Done!                │    │  ✓ You learned something!      │  │
│  └─────────────────────────┘    └────────────────────────────────┘  │
│                                                                     │
│  Both run in parallel — no idle time!                               │
└─────────────────────────────────────────────────────────────────────┘
```

## Paced Reveal Flow

Problems are revealed step-by-step so you can think before seeing the answer:

```
  ┌──────────────┐
  │   Problem    │  "Given an array of integers..."
  └──────┬───────┘
         │  ⏳ 15 seconds
  ┌──────▼───────┐
  │     Hint     │  "Think about what complement you need..."
  └──────┬───────┘
         │  ⏳ 15 seconds
  ┌──────▼───────┐
  │   Approach   │  "Use a hash map to store each number's index..."
  └──────┬───────┘
         │  ⏳ 15 seconds
  ┌──────▼───────┐
  │  Walkthrough │  Step-by-step breakdown
  │  + Solution  │  Complete Python code
  │  + O(n/n)    │  Time & space complexity
  └──────────────┘
```

> You can type "skip" at any point to jump straight to the full solution.

## Example Session

```
You: /leetcode

┌─────────────────────────────────────────────────────┐
│ LeetCode #1: Two Sum                                │
│ Difficulty: Easy  |  Category: Arrays & Hashing     │
│                                                     │
│ Given an array of integers nums and an integer      │
│ target, return indices of the two numbers such      │
│ that they add up to target.                         │
│                                                     │
│ Example:                                            │
│   Input: nums = [2,7,11,15], target = 9             │
│   Output: [0,1]                                     │
│                                                     │
│ Take a moment to think about it...                  │
│ hint coming in 15 seconds                           │
└─────────────────────────────────────────────────────┘

         ... 15 seconds later ...

┌─────────────────────────────────────────────────────┐
│ Hint                                                │
│                                                     │
│ Think about what complement you need for each       │
│ number. Is there a data structure that lets you     │
│ check if you've seen a value before in O(1)?        │
└─────────────────────────────────────────────────────┘

         ... 15 seconds later ...

┌─────────────────────────────────────────────────────┐
│ Approach                                            │
│                                                     │
│ Use a hash map to store each number's index as      │
│ you iterate. For each number, check if              │
│ (target - num) exists in the map.                   │
└─────────────────────────────────────────────────────┘

         ... 15 seconds later ...

┌─────────────────────────────────────────────────────┐
│ Walkthrough                                         │
│                                                     │
│ 1. Create an empty hash map                         │
│ 2. For each number at index i:                      │
│    - complement = target - nums[i]                  │
│    - If complement in map → return [map[c], i]      │
│    - Otherwise store nums[i] → i                    │
│                                                     │
│ Solution                                            │
│                                                     │
│   def two_sum(nums, target):                        │
│       seen = {}                                     │
│       for i, num in enumerate(nums):                │
│           complement = target - num                 │
│           if complement in seen:                    │
│               return [seen[complement], i]          │
│           seen[num] = i                             │
│                                                     │
│ Complexity                                          │
│   Time:  O(n)  — single pass                        │
│   Space: O(n)  — hash map stores up to n elements   │
└─────────────────────────────────────────────────────┘
```

## Auto-Launch Mode

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│  You: "Build the payment integration"                        │
│                                                              │
│  Agent: Dispatching background agent...                      │
│                                                              │
│  ┌─ Hook fires (PreToolUse → Agent) ──────────────────────┐  │
│  │                                                        │  │
│  │  auto_launch: true?  ──yes──▶  Invoke /leetcode skill  │  │
│  │                                as a subagent            │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────┐  ┌──────────────────────────────────┐  │
│  │  Agent working   │  │  Meanwhile...                    │  │
│  │  on payments...  │  │                                  │  │
│  │                  │  │  LeetCode #53: Maximum Subarray  │  │
│  │                  │  │  Difficulty: Easy                │  │
│  │                  │  │                                  │  │
│  │                  │  │  Find the contiguous subarray    │  │
│  │                  │  │  with the largest sum...         │  │
│  └──────────────────┘  └──────────────────────────────────┘  │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

## Problem Categories

```
 50 Problems
 ├── Easy (20)
 │   ├── Arrays & Hashing ····· Two Sum, Contains Duplicate, ...
 │   ├── Two Pointers ········· Valid Palindrome, Move Zeroes, ...
 │   ├── Stacks ··············· Valid Parentheses, Min Stack, ...
 │   ├── Strings ·············· Valid Anagram, Reverse String, ...
 │   ├── Linked Lists ········· Merge Two Lists, Reverse List, ...
 │   ├── Binary Search ········ Binary Search
 │   ├── Trees ················ Max Depth, Same Tree, Invert Tree, ...
 │   └── Dynamic Programming ·· Climbing Stairs
 │
 ├── Medium (22)
 │   ├── Arrays & Hashing ····· Group Anagrams, Top K Frequent, ...
 │   ├── Two Pointers ········· Three Sum, Container With Most Water, ...
 │   ├── Stacks ··············· Daily Temperatures, Evaluate RPN, ...
 │   ├── Strings ·············· Longest Substring, Palindromic Sub, ...
 │   ├── Linked Lists ········· Add Two Numbers, Reorder List, ...
 │   ├── Binary Search ········ Search Rotated Array, Find Min, ...
 │   ├── Trees ················ Level Order, Validate BST, Kth Smallest
 │   └── Dynamic Programming ·· Coin Change, LIS
 │
 └── Hard (8)
     ├── Linked Lists ········· Merge K Lists, Reverse K-Group
     ├── Two Pointers ········· Trapping Rain Water
     ├── Stacks ··············· Largest Rectangle in Histogram
     ├── Binary Search ········ Median of Two Sorted Arrays
     ├── Trees ················ Binary Tree Max Path Sum
     └── Dynamic Programming ·· Longest Valid Parens, Edit Distance
```

## Features

- **50 curated problems** — classic interview staples with complete walkthroughs
- **Paced reveal** — problem → hint → approach → full solution with configurable timing
- **No repeats** — tracks seen problems across sessions in `~/.leetcode-tutor/seen.json`
- **Auto-launch** — kicks in when background agents run (opt-in via settings)
- **Manual mode** — `/leetcode` anytime, `/leetcode hard` to override difficulty
- **Cross-platform** — works on both Claude Code and Cursor

## Installation

### Claude Code

```bash
/plugin install leetcode-tutor
```

## Configuration

Add to your settings under `leetcode-tutor`:

```json
{
  "leetcode-tutor": {
    "enabled": true,
    "auto_launch": true,
    "difficulty": "easy,medium",
    "pause_seconds": 15
  }
}
```

| Setting | Default | Description |
|---------|---------|-------------|
| `enabled` | `true` | Master toggle |
| `auto_launch` | `true` | Auto-trigger when background agents run |
| `difficulty` | `"easy,medium"` | Any combo of `easy`, `medium`, `hard` |
| `pause_seconds` | `15` | Delay between revealing each section |

## Usage

```
/leetcode              show a random problem (uses configured difficulty)
/leetcode easy         override to easy only
/leetcode medium       override to medium only
/leetcode hard         override to hard only
```
