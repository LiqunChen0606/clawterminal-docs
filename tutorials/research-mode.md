# Research Mode — Tutorial

> Most chat with an AI is "ask, get answer." Research Mode is "describe a goal, let the search find the answer." CatClaw turns a propose-evaluate-refine engine into a slash command. You write the task in English, Haiku translates it into runnable Python, and SimpleTES does the actual searching on your Mac. While that runs in the background, the new ClawNotifier daemon makes sure you find out the moment it finishes.

---

## Part 1: What Research Mode Actually Does

There are three new pieces working together.

- **`/notifier install`** sets up a tiny daemon on your Mac that watches for completed background jobs and fires native macOS notifications when they finish. No cloud service, no Apple Push Notification keys, no Node.js process. Just a launchd agent and a shell script polling `/tmp/.claw_out_*.txt` every 2 seconds.
- **`/simpletes install`** clones [SimpleTES](https://github.com/wq-will/SimpleTES) and runs `uv sync` on your Mac. SimpleTES is a propose-evaluate-refine search engine — it generates candidate Python programs, evaluates them, keeps the strongest, and refines from there.
- **`/research <task>`** is the user-facing command. You type a research goal in English. Haiku translates it into two Python files (`init_program.py` + `evaluator.py`), shows you both for review, lets you tweak the search budget, and dispatches the actual search as a background job.

Think of it as: you bring a goal, CatClaw brings the scaffolding and the search loop.

---

## Part 2: Step-by-Step Setup

### Step 1: Install the notifier daemon

You want notifications when background jobs finish. Type:

```
/notifier install
```

A confirmation card appears with the exact shell command preview. Tap **Install**. The daemon writes `~/.clawnotifier/notifier.sh`, a launchd agent plist, and bootstraps it with `launchctl bootstrap gui/$(id -u)`. You'll see "Installed" in the Jobs tab within a few seconds.

Test it: `/submit echo hi` and wait for the standard completion sentinel. A native macOS notification should appear in the upper-right of your Mac screen with the title "ClawTerminal" and body "Job complete."

If you ever want to remove it: `/notifier uninstall`. To verify it's running: `/notifier status`.

### Step 2: Install SimpleTES

```
/simpletes install
```

The card previews:

```bash
command -v uv >/dev/null 2>&1 || curl -LsSf https://astral.sh/uv/install.sh | sh
[ -d ~/.simpletes ] || git clone https://github.com/wq-will/SimpleTES.git ~/.simpletes
cd ~/.simpletes && uv sync
```

Tap **Install**. This takes a minute or two — `uv` will install Python 3.11 if you don't have it, then sync all dependencies. When it finishes, the notifier daemon you just set up will fire a notification on your Mac. (Nice loop closure.)

SimpleTES uses LiteLLM internally and picks up `ANTHROPIC_API_KEY` / `OPENAI_API_KEY` / `GEMINI_API_KEY` from your shell profile. Because background jobs run inside `${SHELL:-/bin/bash} -lic '...'`, your `.zshrc` / `.bashrc` is sourced and the keys are inherited automatically.

### Step 3: Create a Research-Mode chatroom

Tap the **+** button on the room tab bar to open the new-chatroom sheet (it's a sheet now, not an alert — that's how it can host a toggle). Fill in:

- **Room name** — e.g. "Sort experiments"
- **Project directory** — wherever you want SimpleTES task files to land logically (the Python files actually go to `~/.simpletes/tasks/<timestamp>/`, but it's nice to have a project home)
- **Super Research Mode** toggle — flip it on

Tap **Create**. Open the new room. You should see a small purple **🔬 Research** chip in the mode banner above the input field. The chip is a reminder — `/research` is always available, but the chip tells you this room is dedicated to it.

### Step 4: Run your first research task

Type in the chatroom:

```
/research find a faster sort for nearly-sorted 10,000 integer lists
```

CatClaw shows a brief "Generating research task..." status. Behind the scenes, Haiku is reading your task description and producing a JSON document with three fields: `init_program` (the starting Python program SimpleTES will refine), `evaluator` (a Python module that scores any candidate against your goal), and `task_summary` (a one-line restatement of what you asked for).

A sheet slides up showing:

- **Header** — the AI-generated `task_summary`
- **Initial Program** (collapsible) — monospaced Python preview, ~30 lines max. Tap to expand.
- **Evaluator** (collapsible) — same treatment.
- **SimpleTES knobs** — three steppers:
  - `--num-chains` (default 3) — how many parallel refinement chains to run
  - `--k-candidates` (default 3) — how many candidate programs to evaluate per round
  - `--total-budget` (default 30) — total LLM call budget across all chains
- **Run SimpleTES** + **Cancel** buttons

Read both Python files. If anything looks wrong — the evaluator is scoring the wrong thing, or the initial program isn't representative — cancel the sheet and rephrase your task. If they look right, adjust knobs (higher budget = better results but costs more), then tap **Run SimpleTES**.

### Step 5: Wait for the job

The sheet dismisses. A new entry appears in your Jobs tab labeled with the task summary. The job runs `uv run python main.py --init-program tasks/<id>/init_program.py --evaluator tasks/<id>/evaluator.py --num-chains 3 --k-candidates 3 --total-budget 30` on your Mac, with output streaming in real time.

Depending on your budget, this can take anywhere from a couple of minutes to an hour. SimpleTES is propose-evaluate-refine — each round generates new candidates, scores them with your evaluator, and keeps the winners. The total LLM budget caps how many rounds you'll get.

You can leave the app, lock your phone, do whatever. When SimpleTES finishes:

1. The standard background-job result message lands in your chatroom with the final program and its score.
2. Because you installed the notifier, your Mac shows a native notification banner — "ClawTerminal · Job complete."

Tap into the job to see full streaming output: the candidate generation traces, evaluator scores, and the final winning program.

---

## Part 3: Tips for Better Results

- **Be specific about the metric.** "Find a faster sort" is vague. "Find a sort that minimizes total comparisons on lists where 95% of elements are already in place" gives Haiku a much better evaluator.
- **Constrain the search space.** "Find a Python implementation under 50 lines that..." prevents pathological winners.
- **Start small.** Run with the default budget first. If results look promising, re-run with higher `--total-budget` to deepen the search.
- **Check the evaluator first.** If the evaluator is buggy, every candidate gets a meaningless score and the search wastes budget. Read it carefully before tapping Run.
- **One task per room.** The 🔬 chip is per-chatroom for a reason. Keep different research tasks in different rooms so the conversation history stays focused on each goal.

---

## Part 4: When to Use Research Mode vs. Just Asking

| Use `/research` when | Use a normal chat when |
|---------------------|------------------------|
| You can write an evaluator that scores candidates | You want a single answer, not a search |
| The space of possible solutions is large | The answer is in documentation |
| You'd benefit from running 30+ candidates | One round-trip to Claude is enough |
| You want the search to run while you're away | You want immediate back-and-forth |
| You're optimizing a metric (speed, size, accuracy) | You're learning or debugging |

Research Mode shines when "I'd try a bunch of approaches and pick the best one" is more valuable than "Claude tells me the right approach in one shot."

---

## Part 5: Troubleshooting

**The notifier doesn't fire.** Run `/notifier status`. If `launchctl list` doesn't show `app.clawterminal.notifier`, re-install with `/notifier uninstall` then `/notifier install`. macOS may also need notification permission for `Script Editor` — check System Settings → Notifications.

**SimpleTES errors with "ANTHROPIC_API_KEY not set."** Your shell profile doesn't have the key exported, or the background job isn't sourcing your profile. Verify with `/openclaw status`-style sanity check: open a terminal in CatClaw and run `echo $ANTHROPIC_API_KEY`. If it's empty there too, add `export ANTHROPIC_API_KEY=...` to your `.zshrc`.

**The generated `init_program` and `evaluator` look wrong.** Cancel the sheet, rephrase your task with more specifics, and try again. Haiku is fast but it can only work with what you give it.

**The search runs but returns nonsense winners.** The evaluator is probably wrong — it's scoring the wrong thing or has a bug. Read it carefully and re-run with a corrected task description.

---

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/notifier install` | Install the Mac launchd notifier daemon |
| `/notifier uninstall` | Remove the daemon |
| `/notifier status` | Check the daemon is running and tail its log |
| `/simpletes install` | Install SimpleTES + `uv` on your Mac |
| `/simpletes uninstall` | Remove `~/.simpletes` |
| `/simpletes status` | Verify SimpleTES is installed |
| `/research <task>` | Translate a task into init_program + evaluator and dispatch a SimpleTES search |
| Super Research Mode toggle | Per-chatroom toggle in the new-room sheet; surfaces the 🔬 chip |

**Required dependencies:** Python 3.11+ (auto-installed by `uv`), `git` (preinstalled on macOS), `osascript` (preinstalled on macOS).

**Where SimpleTES lives:** `~/.simpletes` on your Mac. Task files land in `~/.simpletes/tasks/<timestamp>/`.

**Notification mechanism:** Native macOS notifications via `osascript -e 'display notification ...'`. Mac-only — no iPhone push.
