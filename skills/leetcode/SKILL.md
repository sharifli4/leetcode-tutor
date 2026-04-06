---
name: leetcode
description: "Show a LeetCode problem with step-by-step walkthrough. Use /leetcode to start, or it auto-launches when background agents run."
---

# LeetCode Tutor

Present a LeetCode problem with a paced, step-by-step walkthrough.

## Trigger

- User types `/leetcode` (manual)
- Hook invokes this skill when a background agent launches (auto)

## Arguments

- `/leetcode` — use configured difficulty filter
- `/leetcode easy` — override to easy only
- `/leetcode medium` — override to medium only
- `/leetcode hard` — override to hard only

## Behavior

1. **Read settings.** Check platform settings for `leetcode-tutor` config. Fall back to `config/defaults.json`.

2. **Read seen state.** Read `~/.leetcode-tutor/seen.json`. If the file doesn't exist, create it with `{"seen": [], "last_shown": null}`.

3. **Select a problem.**
   - Parse the `difficulty` setting (or argument override) into a list of allowed difficulties
   - List all problem files under `problems/<difficulty>/` for each allowed difficulty
   - Filter out any whose ID (e.g., `easy/001-two-sum`) is in the `seen` list
   - If no unseen problems remain, reset `seen` to `[]` and re-select
   - Pick one at random

4. **Read the problem file.** Parse the markdown sections: Problem, Hint, Approach, Walkthrough, Solution, Complexity.

5. **Paced reveal.** Present sections one at a time with pauses:

   **First message — Problem:**
   > ## 🧩 LeetCode #[id]: [title]
   > **Difficulty:** [difficulty] | **Category:** [category]
   >
   > [Problem section content]
   >
   > *Take a moment to think about it... hint coming in [pause_seconds] seconds*

   **Wait `pause_seconds` seconds using sleep.**

   **Second message — Hint:**
   > ## 💡 Hint
   > [Hint section content]
   >
   > *Approach coming in [pause_seconds] seconds...*

   **Wait `pause_seconds` seconds.**

   **Third message — Approach:**
   > ## 🎯 Approach
   > [Approach section content]
   >
   > *Full walkthrough coming in [pause_seconds] seconds...*

   **Wait `pause_seconds` seconds.**

   **Fourth message — Full Solution:**
   > ## 📝 Walkthrough
   > [Walkthrough section content]
   >
   > ## 💻 Solution
   > [Solution section content]
   >
   > ## 📊 Complexity
   > [Complexity section content]

6. **Update seen state.** Append the problem ID to `seen` and update `last_shown` to current ISO timestamp. Write to `~/.leetcode-tutor/seen.json`.

## Skip handling

If the user responds during the reveal (e.g., "skip", "show me the answer", "I know this one"), immediately show the Fourth message (full solution) and stop the paced reveal.

## Error handling

- If no problem files exist for the selected difficulty, tell the user and suggest changing their difficulty setting.
- If `~/.leetcode-tutor/` directory doesn't exist, create it.
