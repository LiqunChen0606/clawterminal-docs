# Every Claude Code Command Works — Examples

> ClawTerminal now forwards any slash command your Mac's `claude` session supports — including commands added in future CLI releases — instead of returning "Unknown command". Commands that need an interactive terminal offer a one-tap jump to the Terminal tab. Copy-paste scenarios below.

## Quick Start

1. Open a chatroom connected to your Mac in **CLI mode** (active tmux session)
2. Type any slash command — even one ClawTerminal has no custom card for
3. If `claude` supports it, the output renders in the rich card view
4. If it's interactive-only, tap **Open in Terminal tab** when prompted

---

## How forwarding decides what to do

```
You type a slash command
        |
        +-- ClawTerminal has a custom UI for it?  --> open the purpose-built card
        |        (e.g. /research, /team, /pr)
        |
        +-- Your claude session supports it?       --> forward to CLI, render as a card
        |        (any other command, incl. new ones)
        |
        +-- Interactive-only command?              --> "Open in Terminal tab" shortcut
                 (/config, /mcp, /agents, /login, /model)
```

---

## 1. Forward a command ClawTerminal has no card for

```
/summary
```

ClawTerminal forwards it to your Mac's `claude` session and renders the result as a normal response card. No "Unknown command" error, no app update needed.

**Good for:** Any Claude Code command that doesn't have a dedicated ClawTerminal screen — today's niche commands and tomorrow's new ones alike.

---

## 2. A brand-new command from a future CLI release

```
/whatever-ships-next
```

If your `claude` binary on the Mac understands it, ClawTerminal passes it through and shows the output. You don't wait for a ClawTerminal update — the day the CLI gets a new command, it works here.

---

## 3. Interactive-only commands jump to the Terminal tab

These commands need a real TTY, so they offer a one-tap shortcut instead of a broken card:

```
/config
/mcp
/agents
/login
/model
```

Type any of them and you'll see:

```
/mcp is interactive and needs a terminal.
[ Open in Terminal tab ]
```

Tap **Open in Terminal tab** to drive the interactive menu where it belongs.

---

## 4. Custom-card commands are unchanged

These still open their purpose-built ClawTerminal UI — forwarding doesn't touch them:

```
/research find a faster bloom filter   → research card
/team Build a REST API with tests       → visual command center (with --app)
/pr review                              → PR review card
```

The passthrough only kicks in for commands ClawTerminal doesn't already render itself.

---

## Notes

- **CLI mode only.** Forwarding sends the command to the `claude` session on your Mac, so you need an active CLI-mode chatroom with a live tmux session. Direct API mode has no CLI to forward to.
- **Worst case is a Terminal-tab nudge.** You'll never hit a hard "Unknown command" wall for a real CLI command again — at most you'll be sent to the Terminal tab for the interactive ones.
- **When unsure, just type it.** Let ClawTerminal decide whether to render a card, forward to the CLI, or hand you the Terminal shortcut.
