# Multi-CLI Tool Support

> Use Aider, Codex, or any custom CLI tool alongside Claude in dedicated chatrooms.

---

## Overview

ClawTerminal is not limited to Claude Code. You can create chatrooms that run different CLI tools over SSH, each with its own tmux session, binary detection, and output parsing. This lets you use the best tool for each task without leaving the app.

---

## Supported Tools

| Tool | Description | Binary |
| ---- | ----------- | ------ |
| **Claude** (default) | Claude Code CLI with full tool use, MCP servers, and `--resume` | `claude` |
| **Aider** | AI pair programming with git integration | `aider` |
| **Codex** | OpenAI Codex CLI | `codex` |
| **Custom** | Any CLI tool you configure | User-defined |

---

## Setting Up a Tool-Specific Chatroom

1. Create a new chatroom (tap **+** in the chatroom list)
2. Tap the **Info (i)** button
3. Under **CLI Tool**, select the tool you want to use
4. Set the **Project Directory** to your project's path on your Mac
5. Start chatting --- the tool runs in its own tmux session

Each tool type uses its own tmux session name prefix, so multiple tool chatrooms can run simultaneously without interfering with each other.

---

## Binary Detection

ClawTerminal automatically detects where the tool binary is installed on your Mac. It searches common paths:

- `/usr/local/bin/`
- `/opt/homebrew/bin/`
- `~/.local/bin/`
- `~/.npm-global/bin/`

The detected path is cached per tool type for fast startup. If detection fails, you can set a custom binary path in the chatroom's Info panel.

---

## Custom CLI Tools

For tools not in the built-in list:

1. Create a new chatroom → Info → CLI Tool → **Custom**
2. Set the **Binary Path** (e.g. `/usr/local/bin/my-tool`)
3. Set the **Invocation Command** --- the shell command template used to run the tool. Use `{message}` as a placeholder for user input:

```text
my-tool --prompt "{message}" --output-format json
```

4. Set the **Project Directory** as usual

---

## Background Jobs with Non-Claude Tools

`/submit` works with all tool types. When you submit a background job from an Aider or Codex chatroom, the job runs using that chatroom's configured tool, not Claude.

The job result is parsed as plain text for non-Claude tools (Claude uses stream-json parsing for richer output).

---

## Tips

- **One tool per chatroom:** Each chatroom is bound to a single tool type. Create separate chatrooms for Claude, Aider, etc.
- **Shared SSH connection:** All chatrooms share the same SSH connection to your Mac. Switching between tool chatrooms is instant.
- **Session persistence:** Like Claude chatrooms, non-Claude tool sessions persist in tmux. If your phone disconnects, the tool keeps running and you can resume on reconnect.
- **Model switching:** `/model` only applies to Claude and API mode. Other tools use their own model configuration (set via their respective config files on your Mac).
