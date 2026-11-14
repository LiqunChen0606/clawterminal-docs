# Multi-CLI Tool Support

> Use Claude Code, Codex, Gemini CLI, or Aider in dedicated chatrooms, each with its own model choice.

---

## Overview

ClawTerminal isn't limited to Claude Code. Each chatroom can run a different AI command-line tool on your Mac over SSH, so you can use the best tool for each task without leaving the app.

---

## Supported Tools

| Tool | Description | Binary |
| ---- | ----------- | ------ |
| **Claude** (default) | Claude Code with full tool use, MCP servers, and session resume | `claude` |
| **Codex** | OpenAI's Codex CLI | `codex` |
| **Gemini** | Google's Gemini CLI | `gemini` |
| **Aider** | AI pair programming with git integration | `aider` |

Install whichever tools you want on your Mac. ClawTerminal finds them automatically.

---

## Switching a Chatroom's Tool

1. In the chatroom tab bar, **long-press the room's tab**
2. Open **AI Tool** and pick Claude, Codex, Gemini, or Aider
3. The room reconnects with the new tool. Start chatting

Each chatroom is bound to one tool, so create separate rooms for different tools. They all share the same SSH connection to your Mac, so switching between rooms is instant.

---

## Choosing Models

Every tool has its own saved model, used everywhere that tool runs: chat, background jobs, `/race`, and multi-tool `/batch`. Set it in **Settings → CLI Tool Models**, or with `/model` in a chatroom using that tool.

### Claude

| Choice | Notes |
|--------|-------|
| **Claude Sonnet 5.5** | Default: fast and capable for everyday work |
| **Claude Opus 5.5** | Strongest reasoning, for hard problems |
| **Claude Fable 5.2** | Fast frontier model with a 1M-token context |
| **Claude Haiku 5.5** | Cheapest and quickest, for simple tasks |
| **Claude Code default** | Passes no model flag, so Claude Code uses whatever your Mac's own configuration says |

`/model` also accepts Claude Code's aliases (`opus`, `sonnet`, `haiku`, `fable`, `opusplan`, `opus[1m]`) and any `claude-*` model ID. Labels and cost estimates use the model that actually ran, even when you pick an alias or the default.

### Codex, Gemini, and Aider

The first option for each of these tools is **CLI default**: no model flag, so the tool uses its own configured model. This is the safest choice, because it keeps working when a provider retires a model. Each tool also lists its current recommended models (for example Gemini's `auto`, `pro`, `flash`, and `flash-lite` aliases), and **Custom…** lets you type any model ID the tool accepts.

If you had a model saved that has since been retired, it switches to CLI default automatically.

### Thinking depth

`/effort low | medium | high | xhigh | max` sets how hard the model thinks. On Claude it becomes Claude Code's native `--effort` setting. Other tools get an equivalent instruction in the prompt. `/effort default` clears it. See [Session Intelligence](session-intelligence.md).

---

## How Each Tool Runs

ClawTerminal runs each tool in non-interactive mode, with automatic approval so a turn never stalls on a prompt you can't see:

- **Claude:** governed by **Settings → AI Intelligence → Claude Code Autonomy** (Full, Auto with a safety classifier, or Accept edits)
- **Codex:** runs in a workspace-write sandbox inside your project directory
- **Gemini:** runs with auto-approval
- **Aider:** runs with auto-confirm

---

## Binary Detection

ClawTerminal looks for each tool on your PATH and in common install locations:

- `~/.local/bin/`
- `/opt/homebrew/bin/`
- `/usr/local/bin/`
- `~/.npm-global/bin/` (npm-installed tools)

---

## Background Jobs With Other Tools

`/submit` works with every tool. A job submitted from a Codex, Gemini, or Aider chatroom runs with that room's tool and its saved model, captured when you submit. Results from non-Claude tools are shown as plain text (Claude jobs get the richer trajectory view).

---

## Tips

- **One tool per chatroom.** Name rooms after their tool ("Codex: API refactor") so they're easy to tell apart.
- **Compare tools with `/race`.** It sends the same prompt to several tools at once, each using its own saved model.
- **Session persistence.** Every tool runs in tmux on your Mac. If your phone disconnects, the tool keeps running on your Mac.
