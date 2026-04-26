# Tool Installers — Tutorial

> Bringing a new AI CLI onto your Mac usually means hopping into a terminal, copying a one-liner from a README, hoping the curl-bash works, and then doing it all again next month when you uninstall. The Tool Installers commands fold that whole loop into a single tap from inside any chatroom — install, uninstall, and check status of third-party AI CLIs without ever leaving CatClaw.

---

## Part 1: What These Commands Do

Two slash commands ship with this release:

- **`/openclaw`** — installs, uninstalls, or checks the OpenClaw CLI (`openclaw/openclaw`).
- **`/hermes`** — installs, uninstalls, or checks the Hermes Agent CLI (`NousResearch/hermes-agent`).

Each command has three subcommands — `install`, `uninstall`, and `status` — that each open a **confirmation card** showing the exact shell command CatClaw is about to run on your Mac. You read it, you tap the action button, and the command dispatches as a background job over your existing SSH connection. Output streams to the Jobs tab in real time and a push notification fires when the install finishes.

If you type just `/openclaw` or `/hermes` with no subcommand, CatClaw appends a small system message with a one-line description of the tool, a tappable link to its GitHub repo, and the list of subcommands. Useful when you forget which actions are available.

---

## Part 2: When to Use Each Command

### Bootstrapping a new Mac

You just got a new dev machine. Your CatClaw room is already SSH'd into it. You want OpenClaw and Hermes Agent on day one.

```
/openclaw install
```

A card slides up:

```
Install OpenClaw

Command preview:
  npm install -g openclaw@latest && openclaw onboard --install-daemon

Repo: https://github.com/openclaw/openclaw

[ Cancel ]   [ Install ]
```

Tap **Install**. The card dismisses and a new entry appears in your Jobs tab — green spinner, label "Install OpenClaw". Output streams as npm fetches the package and runs the onboarding daemon. A push notification fires when it's done.

Then:

```
/hermes install
```

Same flow. Same confirmation, different command. You can run both back-to-back; they queue as independent background jobs and run in their own tmux windows.

### Pre-demo sanity check

Before a TestFlight session or a customer demo, you want to confirm both CLIs are installed on the right path and reporting the right version. No need to `ssh` in and run `which` by hand:

```
/openclaw status
/hermes status
```

Each one opens a gray-buttoned status card:

```
Check OpenClaw

Command preview:
  which openclaw && openclaw --version

[ Cancel ]   [ Check ]
```

Tap **Check**. Within a couple of seconds the Jobs tab shows the binary path and version string. If either tool is missing, the output makes that immediately obvious — `which: openclaw: command not found`, no exit code 0.

### Uninstalling cleanly when switching tools

You've decided to consolidate down to one CLI. The uninstall flow is identical to install except the action button is **red** — that's the visual cue that it removes things on your machine.

```
/hermes uninstall
```

The card:

```
Uninstall Hermes Agent

Command preview:
  rm -f ~/.local/bin/hermes && rm -rf ~/.hermes-agent/

Repo: https://github.com/NousResearch/hermes-agent

[ Cancel ]   [ Uninstall ]
```

Read the command. If it looks right, tap **Uninstall**. The job dispatches; the symlink and the virtualenv directory are removed; you get a notification when it's done. If something looks off — for example you keep a venv at that path for a different project — hit **Cancel**, and nothing runs.

`/openclaw uninstall` works the same way: it stops the launchd daemon (so the agent isn't running while you're trying to remove its binary) and then runs `npm uninstall -g openclaw`.

---

## Part 3: How the Confirmation Card Works

Every install/uninstall/status invocation goes through the same two-step flow:

1. **Card shows.** A sheet slides up with the tool name as the title, the repo URL as a tappable link, the exact shell command CatClaw is about to run, and a colored action button:
   - **Blue** — install (constructive)
   - **Red** — uninstall (destructive)
   - **Gray** — status (read-only)
2. **You decide.** Tap the action button to dispatch, or tap **Cancel** to dismiss without running anything. The Cancel path leaves zero side effects — no tmux session, no Jobs tab entry, no notification.

Why a card and not just-run-it? Two reasons:

- **You see the command before it touches your Mac.** No surprise curl-bash. If the install command is something you'd rather review or modify, you can copy it from the card and run it yourself in the Terminal tab instead.
- **Visual confirmation matches the intent.** Red for "this will delete things" gives you a half-second pause before a destructive action — long enough to catch a wrong tap.

---

## Part 4: Where the Output Goes

Tapping the action button doesn't open a terminal panel — the install happens in the background using the same infrastructure as `/submit`. That means:

- **Jobs tab** — every dispatched install/uninstall/status appears as a job row. Tap it to see live streaming output, the trajectory timeline, and the final exit code.
- **Push notification** — when the job completes, you get a notification with the result. Tap it to jump straight to the job detail view.
- **Mac tmux session** — under the hood the job runs in a dedicated tmux window on your Mac. If you ever want to inspect the install live from the Mac side, the tmux session name is the same one shown in the job detail.

This means you can fire `/openclaw install`, lock your phone, and finish your coffee. The notification will tell you when it's safe to come back.

---

## Part 5: Combining With Other Commands

### After `/openclaw install`, run `/openclaw status`

Right after an install completes, fire the status check to verify the binary is on PATH and reporting the version you expected. Two taps, two cards, full peace of mind.

### Before a `/race`, make sure all CLIs are installed

If you're planning to run `/race` across Claude, Codex, Gemini, plus OpenClaw and Hermes Agent, run a quick `/openclaw status` and `/hermes status` first. The race fails ugly if a tool isn't there; checking up-front saves you a debugging detour.

### Pair with `/notify` for unattended installs

```
/openclaw install
/notify when openclaw install job finishes
```

The first dispatches the install. The second sets up a natural-language notification rule — when the install job's status changes to complete, you get a push (in addition to the built-in one). Useful if you've stacked multiple installs and want a single rollup notification.

---

## Part 6: Adding More Tools

The two tools shipped today (OpenClaw and Hermes Agent) live in a hardcoded registry inside the app. The registry is intentionally small so that every install command in the dropdown has been hand-verified against the upstream README.

If you want a tool added to the registry, the fastest path is to file an issue on the CatClaw repo with the official install/uninstall/status one-liners from the upstream project — we'll add it in the next release.

---

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/openclaw` | Show OpenClaw description + repo link + subcommand list |
| `/openclaw install` | Install OpenClaw CLI globally via npm + run onboarding daemon |
| `/openclaw uninstall` | Stop launchd daemon + `npm uninstall -g openclaw` |
| `/openclaw status` | Print OpenClaw binary path + version |
| `/hermes` | Show Hermes Agent description + repo link + subcommand list |
| `/hermes install` | Install Hermes Agent via the upstream `install.sh` script |
| `/hermes uninstall` | Remove `~/.local/bin/hermes` symlink + `~/.hermes-agent/` venv |
| `/hermes status` | Print Hermes binary path + version |

**Confirmation card colors:** blue = install, red = uninstall, gray = status.

**Where output goes:** Jobs tab (live stream) + push notification on completion.

**Repos:**
- [openclaw/openclaw](https://github.com/openclaw/openclaw)
- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)
