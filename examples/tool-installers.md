# Tool Installers — Examples

> Real scenarios for installing, checking, and uninstalling third-party AI CLIs on your Mac directly from CatClaw — no SSH terminal hop, no copy-pasted curl-bash.

## Quick Start

1. Open any chatroom that's connected to your Mac via SSH
2. Type `/openclaw install` (or `/hermes install`)
3. Read the confirmation card — it shows the exact shell command CatClaw will run
4. Tap the action button (blue for install) — the work dispatches as a background job
5. Watch the Jobs tab for live output, or wait for the push notification

---

## Commands

```
/openclaw                — show description + repo link + subcommand list
/openclaw install        — install OpenClaw CLI on your Mac
/openclaw uninstall      — uninstall OpenClaw + stop its launchd daemon
/openclaw status         — print binary path + version

/hermes                  — show description + repo link + subcommand list
/hermes install          — install Hermes Agent CLI on your Mac
/hermes uninstall        — remove Hermes Agent venv + symlink
/hermes status           — print binary path + version
```

---

## Scenario 1: Bootstrapping a Brand-New Mac

You just unboxed a new dev machine and SSH'd into it from CatClaw. You want both AI CLIs on it before you do anything else.

```
/openclaw install
```

A confirmation card slides up showing:

```
npm install -g openclaw@latest && openclaw onboard --install-daemon
```

Tap **Install**. The card dismisses, a job appears in the Jobs tab with a green spinner, and output streams in real time as npm fetches the package. A push notification fires when the daemon is registered.

Without waiting:

```
/hermes install
```

Same flow, different command:

```
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

Both jobs run independently in their own tmux windows on your Mac. Lock your phone, walk away. Two notifications later, both CLIs are live.

---

## Scenario 2: Pre-Demo Sanity Check

You're about to demo a `/race` between Claude, Codex, OpenClaw, and Hermes Agent at a meetup tomorrow. You want to confirm both CLIs are installed and on the right path.

```
/openclaw status
```

The card preview:

```
which openclaw && openclaw --version
```

Tap **Check** (gray button — read-only). Within a couple of seconds the Jobs tab shows:

```
/usr/local/bin/openclaw
openclaw v0.7.2
```

Then:

```
/hermes status
```

```
/Users/you/.local/bin/hermes
hermes-agent 1.2.0
```

Both green. You're demo-ready.

If either had been missing — `which: openclaw: command not found`, no version line — you'd have time to install it tonight instead of debugging on stage tomorrow.

---

## Scenario 3: Switching Tools, Uninstalling Cleanly

You've been trying out a few AI CLIs and decided to consolidate down to just one. You want Hermes Agent off your machine entirely — no leftover virtualenv files, no orphaned symlink in PATH.

```
/hermes uninstall
```

The card slides up with a **red** action button (the visual cue for destructive actions) and the exact removal command:

```
rm -f ~/.local/bin/hermes && rm -rf ~/.hermes-agent/
```

Read it. If your `~/.hermes-agent/` directory holds anything you want to keep, hit **Cancel** — nothing runs.

Looks right? Tap **Uninstall**. The job dispatches, the symlink and venv go away, and a push fires when it's done.

Then verify:

```
/hermes status
```

```
which: hermes: command not found
```

Clean. The binary is gone, PATH no longer resolves it, and the on-disk venv is removed. If you ever decide to come back, `/hermes install` rebuilds the whole thing in one tap.

---

## Scenario 4: Reading the Description Before Committing

You've heard about OpenClaw but you're not sure exactly what it is, and you don't want to install something blind. Just type:

```
/openclaw
```

A system message gets appended to your conversation:

```
OpenClaw — third-party AI CLI agent for Claude on macOS
Repo: https://github.com/openclaw/openclaw

Subcommands:
  /openclaw install     Install via npm + onboarding daemon
  /openclaw uninstall   Stop daemon + npm uninstall
  /openclaw status      Print path + version
```

The repo URL is tappable — opens the upstream GitHub page in Safari. Once you've read the README and decided you want it, run `/openclaw install` and the install card opens.

`/hermes` works the same way.

---

## Scenario 5: Stacking Installs With a Notification Roll-Up

You want to install both CLIs and then a third tool you'll dispatch manually, and you want one notification when all three are done — not three separate ones interrupting your flow.

```
/openclaw install
/hermes install
/notify when all background jobs are done
```

The first two open install cards back-to-back; tap **Install** on each. They dispatch as independent jobs and run in parallel. The `/notify` rule starts polling job state every 30 seconds. When the last one flips to `done`, you get a single rollup push notification — the per-job notifications still arrive but the rollup tells you the whole batch is finished.

Useful when you're bootstrapping multiple machines or installing several tools after a clean OS reinstall.

---

## Why a Confirmation Card

Every install/uninstall/status command in CatClaw goes through a card before it runs, even though we already know what command the registry will produce. Two reasons:

- **You see the command before it touches your Mac.** No surprise curl-bash, no hidden npm flags. If you'd rather copy and run the command yourself in the Terminal tab, the card lets you do that instead.
- **Color-coded by intent.** Blue for install (constructive), red for uninstall (destructive), gray for status (read-only). The half-second of "is this the action I meant?" is enough to catch most wrong taps.

If you ever want to skip the card entirely for a familiar command, copy the command preview and dispatch it directly from `/submit`. The card is the safe path; `/submit` is the expert path.

---

## Where Output Goes

- **Jobs tab** — every dispatched install/uninstall/status appears as a job row with live streaming output, the trajectory timeline, and the final exit code
- **Push notification** — fires when the job completes
- **Mac tmux session** — under the hood the work runs in a dedicated tmux window that you can also inspect from the Mac side using the session name shown on the job detail page

---

## Repos

- [openclaw/openclaw](https://github.com/openclaw/openclaw)
- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)
