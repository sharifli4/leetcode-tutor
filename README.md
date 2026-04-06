# LeetCode Tutor

A plugin for Claude Code and Cursor that presents LeetCode problems with step-by-step walkthroughs while your AI agents work in the background.

## Features

- 50 curated LeetCode problems (Easy, Medium, Hard)
- Paced reveal: problem → hint → approach → full solution
- Tracks seen problems so you don't get repeats
- Auto-launches when background agents run (configurable)
- Manual mode: `/leetcode` anytime

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

- `difficulty`: any combination of `easy`, `medium`, `hard`
- `pause_seconds`: delay between revealing each section
- `auto_launch`: automatically show problems when background agents run

## Usage

- `/leetcode` — show a random problem
- `/leetcode hard` — override difficulty for one problem
- Automatic mode: problems appear when background agents launch
