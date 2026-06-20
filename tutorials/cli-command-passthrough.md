# Every Claude Code Command Works — Tutorial

> ClawTerminal used to maintain its own list of slash commands. If you typed one the app didn't recognize, you got an "Unknown command" error — even when your `claude` session on the Mac supported it perfectly well. Now ClawTerminal forwards *any* slash command your Claude session understands straight through to the CLI and renders the output in the rich card view. New CLI commands work the day they ship. You no longer wait for an app update.

---

## Part 1: The Old Problem

Claude Code ships new slash commands regularly. ClawTerminal could only handle the ones it had been taught about. Type something newer — or something niche — and you hit a wall:

```
/some-new-command
→ Unknown command: /some-new-command
```

The command existed on your Mac. The app just refused to pass it along. The only workaround was to drop into the Terminal tab and run `claude` by hand.

---

## Part 2: What Changed

ClawTerminal is now a **superset** of the Claude Code command surface. When you type a slash command it doesn't have a custom UI for, it doesn't reject it — it forwards the command to the `claude` session running on your Mac and renders whatever comes back in the same rich card view you get for any response.

That means:

- **Commands ClawTerminal has a custom UI for** (`/research`, `/team`, `/pr`, …) keep their purpose-built cards.
- **Everything else** — including commands added in future Claude Code releases — forwards to the CLI and renders inline.
- **Interactive-only commands** that need a TTY get a one-tap shortcut to the Terminal tab instead of failing silently.

---

## Part 3: Forwarding a Command in Practice

Say a future Claude Code release adds a command called `/summary` and ClawTerminal has no custom card for it yet. Just type it:

```
/summary
```

ClawTerminal sees it isn't one of its own commands, forwards it to your Mac's `claude` session over the existing SSH/tmux pipeline, and renders the result as a normal response card. No error, no app update, no Terminal hop.

This works for any command your `claude` binary supports. If `claude` knows it, ClawTerminal will pass it along.

---

## Part 4: Interactive-Only Commands

A handful of Claude Code commands open an interactive UI that needs a real terminal (a TTY) to work:

- `/config`
- `/mcp`
- `/agents`
- `/login`
- `/model`

These can't render meaningfully inside a chat card — they expect arrow-key menus, live prompts, and keyboard navigation. Instead of forwarding them into a dead end, ClawTerminal recognizes them and shows a one-tap affordance:

```
/mcp
→ /mcp is interactive and needs a terminal.
  [ Open in Terminal tab ]
```

Tap **Open in Terminal tab** and ClawTerminal switches you to the Terminal tab where `claude` is (or can be) running interactively, so you can drive the menu directly. The command lands where it belongs instead of producing a broken card.

---

## Part 5: How to Tell What You'll Get

| You type | What happens |
|----------|--------------|
| A command with a custom ClawTerminal UI (`/research`, `/team`, `/pr`) | The purpose-built card opens |
| Any other command your `claude` session supports | Forwarded to the CLI, rendered as a response card |
| An interactive-only command (`/config`, `/mcp`, `/agents`, `/login`, `/model`) | A one-tap "open in Terminal tab" shortcut |

When in doubt, just type the command. The worst case is the Terminal-tab nudge — never a hard "Unknown command" wall.

---

## Tips

- **CLI mode required for forwarding.** Passthrough forwards to the `claude` session on your Mac, so it needs an active CLI-mode chatroom with a live tmux session. In Direct API mode there's no CLI to forward to.
- **New CLI release? Just try the command.** You don't need to wait for ClawTerminal to add support — if your Mac's `claude` understands it, it works today.
- **Interactive commands are a feature, not a failure.** The Terminal-tab shortcut is the right home for `/login`, `/model`, and the rest — they were always going to need a keyboard.
- **`/help` still lists the built-ins.** ClawTerminal's own grouped command reference shows the commands with custom cards; forwarded commands are whatever your CLI version supports on top of that.
