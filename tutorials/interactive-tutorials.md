# Interactive Tutorials — Tutorial

> Learn CatClaw by actually using it. The `/tutorial` command launches guided, step-by-step tracks that validate your real commands as you go — no passive reading, just hands-on practice.

---

## What It Does

CatClaw now ships with a built-in learning system. Instead of reading a wall of docs and trying to remember which command does what, you run a tutorial and CatClaw walks you through real actions, verifying each step on the live app before moving on.

Six tracks, 43 steps total, all inside the app:

| Track | What you learn |
|-------|----------------|
| **beginner** | First-time essentials: connecting via SSH, opening a chatroom, sending your first message |
| **terminal** | Power-user terminal features: tabs, extended keyboard, port forwarding, Mosh |
| **workflows** | Day-to-day workflows: `/submit`, `/plan`, `/pin`, `/search`, `@web` |
| **agents** | Multi-agent orchestration: `/batch`, `/team`, `/race`, `--vcs`, `--routing` |
| **devops** | Developer intelligence: `/git`, `/health`, `/monitor`, `/security`, `/changelog` |
| **power** | Advanced: `/soul`, `/personality`, `/effort`, `/dream`, `/whatif`, `/wrapped` |

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

Lists all six tracks with one-line descriptions. Tap any row to start.

To jump directly to a track:

```
/tutorial beginner
/tutorial terminal
/tutorial workflows
/tutorial agents
/tutorial devops
/tutorial power
```

### Option B — Settings

Open **Settings → Learn ClawTerminal**. Same track list, with progress bars showing how far you got in each track across past sessions.

### Resetting

Want to re-do a track from scratch?

```
/tutorial reset
```

Clears all step progress for every track. The next time you open a tutorial, you start fresh on step one.

---

## What a Tutorial Step Looks Like

Say you run `/tutorial beginner`. The first card might say:

> **Step 1 of 7 — Connect to your Mac**
>
> CatClaw needs an SSH connection to your Mac to run commands. Tap the Connections tab at the bottom, then tap **+** to add your first profile.
>
> _Waiting for you to connect..._

You tap the Connections tab, add a profile, connect. The instant CatClaw detects an active SSH session, the card advances:

> **Step 2 of 7 — Open a chatroom**
>
> Now open the Claude tab. This is where the AI magic happens. Tap **New Chat** to create your first chatroom.

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
