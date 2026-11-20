# Interactive Tutorials — Tutorial

> Learn CatClaw by actually using it. The `/tutorial` command launches guided, step-by-step tracks that validate your real commands as you go — no passive reading, just hands-on practice.

---

## What It Does

CatClaw now ships with a built-in learning system. Instead of reading a wall of docs and trying to remember which command does what, you run a tutorial and CatClaw walks you through real actions, verifying each step on the live app before moving on.

Eight tracks, 57 steps total, all inside the app:

| Track | Level | What you learn |
|-------|-------|----------------|
| **beginner** | Beginner | First message, `/help`, `/cost`, `/search`, inline Run buttons, pinning, memory basics |
| **terminal** | Beginner | Terminal tab, extended keyboard, mini terminal, AI error diagnosis, terminal tabs |
| **workflows** | Intermediate | `/submit`, `/plan`, `/pin`, `/remember`, `/effort`, `/personality`, `/btw`, `/retry` |
| **anywhere** | Intermediate | Mac Sessions (`/sessions`), `/model`, `/effort xhigh`/`max`, `/doctor`, `/netdiag`, Show Reply Timings, Keep Claude running between messages, queued messages, `/bg` with Live Activities, `/notifier hooks` for Mac alerts and Lock-Screen approvals |
| **agents** | Intermediate | `/batch --app`, `/team --app`, `/race`, the Jobs tab, trajectories, `/orchestrate` |
| **devops** | Advanced | `/git`, `/health`, `/monitor`, `/security`, `/gentest`, `/changelog`, `/notify`, `/dashboard` |
| **power** | Advanced | `/dream`, `/soul`, `/bg` workflows, long-press send, branching, `/trace` |
| **research** | Advanced | `/notifier install`, `/simpletes install`, `/research`, Super Research Mode |

Every tutorial step includes:

- A plain-English explanation of what you are about to do
- The exact command or gesture to try
- Automatic verification — CatClaw watches for the expected action and moves on only when it detects you did it
- A "Skip step" option if you already know the material

---

## Getting Started

### Option A — Slash Command

```
/tutorial
```

Lists all eight tracks with their step counts.

To jump directly to a track:

```
/tutorial beginner
/tutorial terminal
/tutorial workflows
/tutorial anywhere
/tutorial agents
/tutorial devops
/tutorial power
/tutorial research
```

### Option B — Settings

Open **Settings → Learn Hub**, or tap the Learn icon in the chatroom toolbar. Same track list, with progress bars showing how far you got in each track across past sessions.

### Resetting

Want to re-do a track from scratch?

```
/tutorial reset
```

Clears all step progress for every track. The next time you open a tutorial, you start fresh on step one.

---

## What a Tutorial Step Looks Like

Say you run `/tutorial anywhere`. The first card says:

> **Type `/sessions` to see every Claude Code session running on your Mac — including ones you started in Terminal at your desk.**
>
> _Hint: also in the chatroom's … menu and the drawer._

You type `/sessions` and the Mac Sessions sheet opens. The card shows a quick success message and moves on:

> **Type `/model` to see the models for this room's AI tool. Switch with an ID or an alias — e.g. `/model opus`, `/model fable`, or `/model default`.**

And so on. Each step is a tiny, verifiable action — not a wall of text.

---

## Tips

- **Start with `beginner`** even if you have used CatClaw before. It takes about 4 minutes and covers the 3 or 4 features you might have missed.
- **Tracks are independent.** You do not have to finish `beginner` to try `agents`. Pick whatever matches your current interest.
- **Progress persists across app restarts.** If you finish 3 steps of `devops` and close the app, you resume at step 4 next time.
- **Run `/tutorial reset` before a demo** — it resets progress so the tutorial cards show up fresh for an audience.
- **Tutorials are fully local.** No network calls, no API costs, no analytics sent anywhere.

---

## Why This Exists

CatClaw has over 60 slash commands and a lot of hidden gestures. Docs are good, but muscle memory is better. The tutorial system is for the 80% of features most users never discover — not because they are hard, but because nobody told them to try.

Five minutes in the `power` track and you will be using `/soul`, `/effort high`, and `/dream` like you have had them all along.
