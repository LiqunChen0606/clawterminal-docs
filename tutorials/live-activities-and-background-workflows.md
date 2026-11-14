# Live Activities & Background Workflows — Tutorial

> Give your Mac a whole goal ("audit this repo for TODOs and summarize them"), put your phone away, and check progress from the Lock Screen or the Dynamic Island. `/bg` runs the goal as a Claude Code workflow on your Mac, and every background job now has a Live Activity, so you can see how it's going without opening the app.

---

## Part 1: `/bg` — Background Workflows

```
/bg audit this repo for TODOs and summarize them
```

`/bg <goal>` submits your goal as a background job and asks Claude Code to handle it with its **Workflow tool**: it plans the work, runs parallel subagents, checks the results, and writes a final summary.

- **Runs on your Mac, in its own tmux window.** It keeps going if your phone sleeps, loses signal, or disconnects.
- **Keep chatting.** The chatroom stays free while the workflow runs.
- **Notified when done.** You get the same completion notification as any background job (see [Smart Notifications](smart-notifications.md)).

### Going bigger with `--ultra`

```
/bg --ultra migrate the networking layer to async/await and update every call site
```

`--ultra` asks for the most thorough orchestration Claude Code offers. Use it for big, multi-file goals where thoroughness matters more than speed or cost.

### Starting one without typing `/bg`

Type your goal, **long-press the send button**, and choose **Run as Background Workflow**.

### Following along

Workflows get their own purple **Workflows** section in the Jobs tab, with a **Workflow** chip. Each one shows your goal (not the internal prompt), and its trajectory lists readable Workflow, Agent, and Task steps, so you can see what each subagent did. Cost and status work like any other job.

### `/bg` vs `/submit` vs `/team`

| Command | Best for |
|---------|----------|
| `/submit <task>` | One focused task, done by one agent |
| `/bg <goal>` | A bigger goal you're happy to let Claude Code plan and split up on its own |
| `/team --app <goal>` | When you want to watch ClawTerminal's visual Research → Implement → Review waves |

---

## Part 2: Live Activities & the Dynamic Island

Every background job, whether from `/submit`, `/bg`, `/team`, `/batch`, `/orchestrate`, `/race`, or a `?` search, shows a **Live Activity** on your Lock Screen and in the **Dynamic Island**:

- **Status at a glance:** what the job is doing, elapsed time, and cost so far
- **One per run:** a multi-agent run (`/team`, `/batch`, `/orchestrate`, `/race`) shows as a single activity, not one per agent
- **Finished jobs stay a few minutes:** done and failed jobs stay visible for about four minutes so you see the result, then clear themselves
- **Tap to open:** tapping the activity opens ClawTerminal on that job

Up to three activities show at once. When a fourth job starts, the oldest activity is removed.

### Privacy: details are hidden by default

Anyone who can see your phone can see the Lock Screen. So by default, a Live Activity shows only a **generic status** ("Working", "Thinking"), the elapsed time, and the cost. It does not show what you asked for.

To also show the job's request and the tool it's running, turn on **Settings → AI Intelligence → Job Details on Lock Screen**.

### "Updating paused — open ClawTerminal"

Live Activities are updated by the app while it follows the job. If iOS suspends ClawTerminal in the background, or the connection to your Mac drops, updates stop. Instead of showing a stale "Running" forever, the activity says **"Updating paused — open ClawTerminal"**. Open the app and it catches up. The job itself never stops: it keeps running on your Mac the whole time.

For a guaranteed alert when a job finishes, even with the app closed, set up Mac-side push with `/notifier` (Apple Push, Telegram, or the no-account ntfy option). See [Smart Notifications](smart-notifications.md).

---

## Tips

- **Hide details in public, show them at home.** The Lock Screen toggle takes effect from the next update.
- **Pair `/bg` with `/effort`.** Background jobs inherit the room's `/effort` level, so `/effort high` before a `/bg` run gets deeper reasoning from every subagent.
- **Watch a workflow live.** Turn on **Settings → AI Intelligence → Live Subagent Progress** (needs Claude Code v2.1.211 or newer) to see subagent work stream into the trajectory.
