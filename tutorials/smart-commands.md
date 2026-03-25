# Smart Commands

> User-defined slash commands with named parameters, background execution, tool overrides, skill injection, and automatic batch orchestration.

---

## Overview

ClawTerminal's custom command system lets you define your own `/command` shortcuts. Sprint 42 upgrades these into **Smart Commands** — commands that go beyond simple text expansion and can:

- Accept **named parameters** with optional defaults
- **Auto-submit** as a background job without manual `/submit`
- **Override the CLI tool** for a specific command regardless of the chatroom's default
- **Auto-attach skills** when the command runs
- **Auto-run as `/batch`** with a configured agent count

---

## Anatomy of a Smart Command

Each Smart Command has the following fields:

| Field | Type | Description |
|-------|------|-------------|
| **Name** | String | The slash command name, e.g. `deploy` → `/deploy` |
| **Template** | String | The prompt template, with `{param}` placeholders |
| **Parameters** | `[CommandParam]` | Named parameters with optional default values |
| **Run in Background** | Bool | Auto-submit as a background job |
| **Tool Override** | CLIToolType? | Force a specific tool (Claude / Codex / Gemini / Aider) |
| **Skill Aliases** | [String] | Skills to auto-attach when this command runs |
| **Agent Count** | Int? | If set, auto-run as `/batch --agents N` |

---

## Named Parameters

Parameters are defined with a name and an optional default value. In your template, reference them as `{paramName}`.

### Example: Deploy Command

**Definition:**

| Field | Value |
|-------|-------|
| Name | `deploy` |
| Template | `Deploy the app to {environment} from branch {branch}. Run smoke tests after deploy.` |
| Parameters | `environment` (default: `staging`), `branch` (default: `main`) |

**Usage:**

```text
/deploy                        → Deploy to staging from main
/deploy production             → Deploy to production from main (positional: first param)
/deploy production feature-x   → Deploy to production from feature-x
```

Parameters are substituted positionally by default. If you type fewer arguments than the template has parameters, the remaining parameters use their default values.

### Example: Code Review Command

**Definition:**

| Field | Value |
|-------|-------|
| Name | `review` |
| Template | `Review the {scope} for {focus}. Focus especially on {focus}.` |
| Parameters | `scope` (default: `current PR changes`), `focus` (default: `bugs and security`) |
| Skill Aliases | `code-review` |

**Usage:**

```text
/review                                → Review current PR changes for bugs and security
/review src/auth.ts                    → Review src/auth.ts for bugs and security
/review src/auth.ts performance        → Review src/auth.ts for performance
```

---

## Run in Background (`runInBackground`)

When `runInBackground` is enabled, invoking the command automatically submits it as a `/submit` background job — no need to prefix with `/submit` manually.

**Example:**

**Definition:**

| Field | Value |
|-------|-------|
| Name | `audit` |
| Template | `Perform a full security audit of {target}. Check for hardcoded secrets, vulnerable dependencies, and unsafe patterns.` |
| Parameters | `target` (default: `the entire codebase`) |
| Run in Background | Yes |

**Usage:**

```text
/audit                     → Submits as background job: audit the entire codebase
/audit src/payments/        → Submits as background job: audit src/payments/
```

A notification fires when the job completes, and the result is injected into your next chatroom message automatically.

---

## Tool Override (`toolOverride`)

Force a specific CLI tool for a command, regardless of what tool the chatroom is configured to use.

**Example:** You mainly use Claude in your chatroom, but you want one command that always uses Aider for git-integrated refactoring:

**Definition:**

| Field | Value |
|-------|-------|
| Name | `refactor` |
| Template | `Refactor {file} to improve readability and reduce complexity.` |
| Parameters | `file` |
| Tool Override | Aider |

```text
/refactor src/auth/login.ts
```

This runs Aider on your Mac (via SSH) regardless of the chatroom's default tool, so you get Aider's git integration for the diff and commit.

---

## Skill Aliases (`skillAliases`)

Skills listed in `skillAliases` are automatically injected into the command's system prompt when it runs. This lets you create domain-specific commands that always have the right context without manually toggling skills.

**Example:**

**Definition:**

| Field | Value |
|-------|-------|
| Name | `perf` |
| Template | `Profile {target} and identify the top bottlenecks. Suggest concrete optimizations.` |
| Parameters | `target` (default: `the app startup path`) |
| Skill Aliases | `profiling, backend` |

```text
/perf                      → Profile startup with profiling + backend skills active
/perf the payment service  → Profile payment service with the same skills
```

---

## Agent Count (`agentCount`)

When `agentCount` is set to a number (2–10), invoking the command automatically runs it as `/batch --agents N`. The Commander decomposes your command's prompt into subtasks for the worker agents.

**Example:**

**Definition:**

| Field | Value |
|-------|-------|
| Name | `fullaudit` |
| Template | `Perform a comprehensive audit of {scope}: security, performance, code quality, and test coverage.` |
| Parameters | `scope` (default: `the entire codebase`) |
| Agent Count | 4 |
| Run in Background | Yes |

```text
/fullaudit                        → 4-agent batch audit of the entire codebase
/fullaudit src/payments/           → 4-agent batch audit of src/payments/
```

Each invocation spawns a Commander + 4 Workers + Synthesizer as a coordinated group in the Jobs panel.

---

## Creating and Managing Smart Commands

### Create a new Smart Command

1. Open the slash command palette (`/`) → tap **Manage Commands**
2. Tap **+** to create a new command
3. Fill in the fields:
   - **Name** (required) — no spaces, no leading `/`
   - **Template** (required) — use `{paramName}` placeholders
   - **Parameters** — tap **Add Parameter** for each placeholder; set a default if desired
   - **Run in Background** — toggle on for long-running tasks
   - **Tool Override** — pick a tool or leave as "Chatroom Default"
   - **Skill Aliases** — comma-separated list of skill names or aliases
   - **Agent Count** — set a number to auto-run as `/batch`, or leave blank for a single-agent run
4. Tap **Save**

### Edit or delete a command

Swipe left on a command in the list → **Edit** or **Delete**.

### Export and share commands

Tap a command → **Export** to save as JSON. Share with teammates and they can **Import** it directly.

---

## Example Smart Command Library

Here are some useful commands to get started:

### `/deploy <env> <branch>`

```text
Template:  Deploy {env} from branch {branch}. Run health checks after deploy.
Params:    env (default: staging), branch (default: main)
Background: Yes
```

### `/test <scope>`

```text
Template:  Run the test suite for {scope}. Report failures with suggested fixes.
Params:    scope (default: the entire project)
Background: Yes
```

### `/docs <file>`

```text
Template:  Write comprehensive documentation for {file}. Include examples and edge cases.
Params:    file
Tool:      Claude
```

### `/review <target> <focus>`

```text
Template:  Review {target} for {focus}.
Params:    target (default: current changes), focus (default: bugs, security, and performance)
Skills:    code-review
```

### `/sweep <area>`

```text
Template:  Do a full sweep of {area}: refactor, add missing tests, update docs, fix lints.
Params:    area (default: the whole codebase)
Background: Yes
Agents:    5
```

---

## Tips

- **Use descriptive parameter names** like `{environment}` rather than `{e}` — they appear as hints in the slash command palette autocomplete.
- **Set sensible defaults** so commands are useful with zero arguments. `/deploy` should "just work" for the most common case.
- **Combine `agentCount` with `runInBackground`** — batch commands are always long-running, so enabling `runInBackground` ensures you don't have to wait for completion before doing other things.
- **`toolOverride` is sticky to the command, not the chatroom.** Your chatroom's default tool is unchanged; only this specific command invocation uses the override.
- **Skills from `skillAliases` stack with the chatroom's active skills.** If the chatroom already has a skill enabled and the command also lists it, it's only injected once.
