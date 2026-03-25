# Smart Commands Examples

Smart Commands are user-defined slash commands that go beyond text shortcuts. They support named parameters with defaults, automatic background submission, CLI tool overrides, skill injection, and auto-batch execution.

---

## How to create a Smart Command

1. Type `/` in the chat input → tap **Manage Commands**
2. Tap **+** to create a new command
3. Fill in the fields and tap **Save**

The command is available immediately as `/yourname` in any chatroom.

---

## 1. Simple text expansion

The most basic form: a shortcut for a prompt you type often.

**Example: Quick bug report**

| Field | Value |
|-------|-------|
| Name | `fix` |
| Template | `Please find and fix this bug: {description}` |
| Parameters | `description` |

Usage:
```
/fix broken login button on mobile
```

Sends to Claude:
```
Please find and fix this bug: broken login button on mobile
```

**Example: Code explanation**

| Field | Value |
|-------|-------|
| Name | `explain` |
| Template | `Explain this code in plain English, suitable for a junior developer: {code_or_file}` |
| Parameters | `code_or_file` |

Usage:
```
/explain src/auth/jwt_service.py
```

---

## 2. Named parameters with defaults

Parameters are filled in positionally. If you provide fewer arguments than there are parameters, the remaining ones use their default values.

**Example: Deploy command**

| Field | Value |
|-------|-------|
| Name | `deploy` |
| Template | `Deploy the app to {env} from branch {branch}. Run smoke tests after deploy and report the result.` |
| Parameters | `env` (default: `staging`), `branch` (default: `main`) |

Usage:
```
/deploy                        → Deploy to staging from main (both defaults)
/deploy production             → Deploy to production from main (first param overridden)
/deploy production feature-x   → Deploy to production from feature-x (both overridden)
```

**Example: PR review**

| Field | Value |
|-------|-------|
| Name | `review` |
| Template | `Review {target} for {focus}. Be specific about what needs to change and why.` |
| Parameters | `target` (default: `the current changes`), `focus` (default: `correctness, security, and performance`) |

Usage:
```
/review                                → Review current changes for correctness, security, and performance
/review src/payments/checkout.py       → Review that file with default focus
/review src/payments/checkout.py performance   → Review that file for performance specifically
```

---

## 3. Background auto-submit

When **Run in Background** is enabled, invoking the command automatically submits it as a background job. You get a notification when it's done.

**Example: Security audit (runs in background)**

| Field | Value |
|-------|-------|
| Name | `audit` |
| Template | `Perform a full security audit of {target}. Check for hardcoded secrets, vulnerable dependencies, unsafe input handling, missing authentication, and insecure configurations. Write a prioritized report.` |
| Parameters | `target` (default: `the entire codebase`) |
| Run in Background | Yes |

Usage:
```
/audit                        → Background job: audit the entire codebase
/audit src/payments/          → Background job: audit the payments module
```

The job appears in the Jobs panel immediately. You get a push notification when Claude finishes.

**Example: Sweep command (background + multi-agent)**

| Field | Value |
|-------|-------|
| Name | `sweep` |
| Template | `Do a full sweep of {area}: fix linting issues, remove dead code, add missing type annotations, update stale docstrings, and ensure all public functions have tests.` |
| Parameters | `area` (default: `the whole codebase`) |
| Run in Background | Yes |
| Agent Count | 5 |

Usage:
```
/sweep                        → 5-agent background sweep of the whole codebase
/sweep src/api/               → 5-agent background sweep of src/api/
```

---

## 4. CLI tool override

Force a specific tool for a command, regardless of the chatroom's default tool.

**Example: Git-integrated refactor with Aider**

Your chatroom normally uses Claude, but this command always runs Aider to get git-tracked diffs and commits.

| Field | Value |
|-------|-------|
| Name | `refactor` |
| Template | `Refactor {file} to improve readability and reduce complexity. Use clear variable names, extract helper functions, and add comments for non-obvious logic.` |
| Parameters | `file` |
| Tool Override | Aider |

Usage:
```
/refactor src/auth/login.ts
```

Aider runs on your Mac, makes the changes, and creates a git commit — even though your chatroom is configured for Claude.

**Example: Gemini for large-file analysis**

| Field | Value |
|-------|-------|
| Name | `analyze` |
| Template | `Analyze {target} and produce a comprehensive summary: what it does, key design decisions, potential issues, and suggestions for improvement.` |
| Parameters | `target` (default: `the codebase`) |
| Tool Override | Gemini |

```
/analyze src/core/
```

---

## 5. Skill injection

Skills listed in **Skill Aliases** are injected automatically when the command runs.

**Example: TDD feature implementation**

| Field | Value |
|-------|-------|
| Name | `feature` |
| Template | `Implement {feature_name} using test-driven development. Start with failing tests, then write the minimum code to pass them, then refactor.` |
| Parameters | `feature_name` |
| Skill Aliases | `tdd` |

Usage:
```
/feature user profile page with avatar upload
```

Claude receives the TDD skill's system prompt automatically — no need to toggle the skill in the toolbar first.

**Example: Performance audit with multiple skills**

| Field | Value |
|-------|-------|
| Name | `perf` |
| Template | `Profile {target} and identify the top 5 performance bottlenecks. Suggest concrete optimizations with estimated impact.` |
| Parameters | `target` (default: `the app startup path`) |
| Skill Aliases | `profiling, backend` |

```
/perf the API request handling pipeline
```

---

## 6. Auto-batch (Agent Count)

Setting **Agent Count** makes the command automatically run as `/batch --agents N`.

**Example: Morning briefing (3 parallel agents)**

| Field | Value |
|-------|-------|
| Name | `morning` |
| Template | `Generate a morning briefing for the {project} project: (1) summarize recent git commits, (2) check for any failing tests, (3) list open TODO comments in the codebase.` |
| Parameters | `project` (default: `current`) |
| Agent Count | 3 |
| Run in Background | Yes |

Usage:
```
/morning
```

Three agents run in parallel — one reads git log, one runs the test suite, one greps for TODOs — and the Synthesizer combines a summary.

**Example: Full codebase audit (5 agents)**

| Field | Value |
|-------|-------|
| Name | `fullaudit` |
| Template | `Comprehensive audit of {scope}: security vulnerabilities, performance bottlenecks, code quality issues, outdated dependencies, and missing test coverage.` |
| Parameters | `scope` (default: `the entire codebase`) |
| Agent Count | 5 |
| Run in Background | Yes |

```
/fullaudit
/fullaudit src/payments/        → Focused 5-agent audit of payments
```

---

## Starter command library

Here are five commands to set up immediately:

### `/deploy <env> <branch>`
```
Template:  Deploy {env} from branch {branch}. Run health checks after deploy. Report any issues.
Params:    env (default: staging), branch (default: main)
Background: Yes
```

### `/test <scope>`
```
Template:  Run the test suite for {scope}. Report all failures with the test name, error message, and a suggested fix.
Params:    scope (default: the entire project)
Background: Yes
```

### `/docs <file>`
```
Template:  Write comprehensive documentation for {file}: purpose, parameters, return values, edge cases, and usage examples.
Params:    file
Tool:      Claude
```

### `/check <pr>`
```
Template:  Review {pr} for correctness, security, performance, and style consistency with the codebase.
Params:    pr (default: the current branch changes)
Skills:    code-review
```

### `/daily`
```
Template:  Daily check: (1) run tests and report failures, (2) check for outdated dependencies, (3) grep for FIXME/TODO and list the top 5.
Agents:    3
Background: Yes
```
