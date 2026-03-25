# Batch Multi-Agent Orchestration (`/batch`)

> Spawn a coordinated swarm of AI agents — a Commander decomposes your goal, parallel Workers execute it, and a Synthesizer merges the results.

---

## Overview

`/batch` is the next evolution of ClawTerminal's agent orchestration. Where `/orchestrate` spawns a fixed set of three role-specific agents (Researcher, Implementer, Reviewer), `/batch` gives you full control: choose how many workers to spawn, optionally assign different CLI tools to different subtasks, attach skills, and enable automatic checkpointing.

The workflow has three phases:

```text
┌─────────────────────────────────────────────────┐
│  /batch --agents 4 redesign the auth module     │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
          ┌─────────────────┐
          │   Commander     │  Reads your goal, decomposes
          │   (orchestrator)│  it into N parallel subtasks
          └────────┬────────┘
                   │  assigns subtasks
        ┌──────────┼──────────┬──────────┐
        ▼          ▼          ▼          ▼
  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
  │ Worker 1 │ │ Worker 2 │ │ Worker 3 │ │ Worker 4 │
  │ (Claude) │ │ (Claude) │ │ (Claude) │ │ (Claude) │
  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘
       │             │            │              │
       │   All workers run as parallel jobs      │
       └──────────────┬─────┬────────────────────┘
                      │     │
                      ▼     ▼
              ┌──────────────────┐
              │   Synthesizer    │  Reads all worker results,
              │                  │  posts a unified summary
              └──────────────────┘
```

---

## Basic Usage

```text
/batch redesign the authentication module
```

This spawns the default 3 workers. The Commander breaks your goal into 3 subtasks, each runs as a background job, and the Synthesizer combines their output.

---

## Flags

| Flag | Description | Default |
|------|-------------|---------|
| `--agents N` | Number of parallel worker agents | `3` |
| `--multi` | Cross-tool assignment — Commander assigns different CLI tools to different subtasks based on their strengths | off |
| `--ckpt` | Enable automatic checkpoint detection on all worker jobs | off |
| `--skills alias1,alias2` | Attach named skills to all worker agents | none |

### `--agents N`

Control how many worker agents run in parallel. Valid range: 2–10.

```text
/batch --agents 5 perform a full security audit of the codebase
```

More agents means finer task decomposition and faster wall-clock time (assuming your Mac can handle the load), but also more token usage. For most tasks, 3–4 agents hit the sweet spot.

### `--multi` — Cross-Tool Assignment

When `--multi` is set, the Commander SSHs to your Mac and detects which CLI tools are installed (Claude, Codex, Gemini, Aider). It then assigns each subtask to the tool best suited for it:

| Tool | Strengths |
|------|-----------|
| **Claude** | Code reasoning, architecture, documentation |
| **Codex** | Focused code generation, completions |
| **Gemini** | Long-context analysis, large file reads |
| **Aider** | Git-integrated refactoring, multi-file edits |

```text
/batch --agents 4 --multi migrate the database schema from Postgres to SQLite
```

The Commander might assign:
- Worker 1 (Gemini) — Read and analyze the full schema file
- Worker 2 (Aider) — Rewrite migration scripts with git integration
- Worker 3 (Claude) — Update ORM models and type annotations
- Worker 4 (Claude) — Write tests for the migration

If a tool is not installed on your Mac, the Commander falls back to Claude for that subtask.

### `--ckpt` — Auto Checkpoints

All worker jobs get `--ckpt` enabled, so their progress is tracked automatically in the Jobs panel. See [Understanding Agent Checkpoints](agent-checkpoints.md) for how checkpoints work.

```text
/batch --agents 3 --ckpt refactor the payment service
```

### `--skills` — Attach Skills

Attach one or more skills (by their alias or display name) to all worker agents. The skills are injected into each worker's system prompt, providing shared context.

```text
/batch --agents 3 --skills docker,testing audit the deployment configuration
```

---

## Combining Flags

All flags can be combined:

```text
/batch --agents 5 --multi --ckpt --skills backend,testing implement OAuth2 login flow
```

This spawns 5 workers, assigns tools based on detected installs, enables auto-checkpoints on all workers, and attaches the `backend` and `testing` skills to each.

---

## Viewing Batch Jobs

Open the **Jobs** panel (clipboard icon in the chat toolbar) to see the batch group.

### Job Group Structure

A `/batch` run creates the following jobs, displayed as a collapsible group:

| Job | Role | Color |
|-----|------|-------|
| Commander | Decomposes the goal into subtasks | Gray |
| Worker 1 … N | Executes assigned subtask | Blue |
| Synthesizer | Merges all worker results | Purple |

The group header shows:
- The original goal text
- Overall status (running / all complete / any failed)
- Agent count badge

### Individual Job Details

Tap any job in the group to open its **Job Detail** view:
- **Request**: the subtask the Commander assigned to this worker
- **Result**: the worker's output
- **Tool**: which CLI tool ran this worker (visible when `--multi` is used)
- **Checkpoints**: timeline of progress markers (when `--ckpt` is enabled)

---

## `/batch` vs `/orchestrate` vs `/submit`

| | `/submit` | `/orchestrate` | `/batch` |
|---|-----------|----------------|----------|
| **Agents** | 1 | 3 (fixed roles) | 2–10 (dynamic) |
| **Task decomposition** | You define the task | Fixed: Researcher / Implementer / Reviewer | Commander decomposes dynamically |
| **Multi-tool** | No | No | Yes (`--multi`) |
| **Checkpoints** | `--ckpt` flag | Not supported | `--ckpt` flag |
| **Skills injection** | `--skills` flag | No | `--skills` flag |
| **Best for** | Single focused task | Multi-perspective review | Complex goals needing flexible decomposition |

---

## Tips

- **Start with the default 3 workers.** Add more only if the Commander's decomposition feels too coarse-grained for your task.
- **Use `--multi` for polyglot tasks.** If your goal naturally spans code generation, documentation, and testing, letting different tools handle each subtask often produces better results than forcing everything through one tool.
- **Combine with Skills.** If your project has a custom skill (e.g. a Skill describing your API design conventions), attach it with `--skills` so every worker inherits the same context.
- **Check the Commander's subtasks.** After the Commander finishes (usually within 30 seconds), the worker job cards in the Jobs panel show the exact subtask text. If the decomposition looks wrong, you can cancel the workers and re-run with a more specific goal.
- **`--ckpt` is always worth adding for long tasks.** It costs nothing extra and gives you a live progress timeline without asking Claude to manually output markers.
