# LeetCode Tutor

This plugin presents LeetCode problems with step-by-step walkthroughs to keep you learning while background agents work.

## Settings

Read settings from the platform's settings file under the `leetcode-tutor` key. Fall back to `config/defaults.json` for any missing values.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `enabled` | boolean | `true` | Master toggle |
| `auto_launch` | boolean | `true` | Auto-trigger when background agents run |
| `difficulty` | string | `"easy,medium"` | Comma-separated allowed difficulties |
| `pause_seconds` | number | `15` | Delay between section reveals |

## State

Seen problems are tracked in `~/.leetcode-tutor/seen.json`:

```json
{"seen": [], "last_shown": null}
```

When selecting a problem:
1. List all `.md` files under `problems/<difficulty>/` for each allowed difficulty
2. Filter out IDs already in `seen`
3. If all are seen, reset `seen` to empty
4. Pick one at random
5. After presenting, append its ID (e.g., `"easy/001-two-sum"`) to `seen`
