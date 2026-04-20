# Interactive Tutorials

> `/tutorial` launches guided, step-by-step tracks that validate your real commands as you go. Six tracks, 43 steps total, fully hands-on.

## List all tracks

```
/tutorial
```

Shows:

```
Tutorial Tracks

 ▸ beginner   — First-time essentials (7 steps, ~4 min)
 ▸ terminal   — Power-user terminal features (6 steps, ~5 min)
 ▸ workflows  — Day-to-day developer workflows (8 steps, ~7 min)
 ▸ agents     — Multi-agent orchestration (7 steps, ~8 min)
 ▸ devops     — Developer intelligence toolkit (8 steps, ~6 min)
 ▸ power      — Advanced commands and hidden features (7 steps, ~7 min)
```

## Start a specific track

```
/tutorial beginner
/tutorial workflows
/tutorial power
```

The first step card appears. Follow the instructions, perform the action, and CatClaw auto-advances when it detects you completed it.

## Reset your progress

```
/tutorial reset
```

Output:

```
Tutorial progress cleared for all 6 tracks.
You'll start from step 1 on your next tutorial.
```

## Launch from Settings

Open **Settings → Learn ClawTerminal**. Same tracks, plus progress bars showing which you have partially or fully completed.

---

## When to use each track

| You are... | Start with... |
|------------|--------------|
| New to CatClaw | `beginner` |
| Comfortable with basics, want more from the terminal | `terminal` |
| Happy with SSH, curious about AI chatroom features | `workflows` |
| Ready to orchestrate multiple agents | `agents` |
| Building a DevOps workflow on your phone | `devops` |
| A CatClaw veteran who wants to see what you missed | `power` |

## Tips

- Tracks are independent — finish any one without doing the others.
- Progress persists across app restarts. Close the app mid-tutorial and resume later.
- Runs fully local. No API calls, no analytics, no cost.
- Re-run `/tutorial reset` before demos to get fresh tutorial cards for your audience.
