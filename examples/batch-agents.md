# Batch Multi-Agent Examples

`/batch` spawns a team of AI agents that work in parallel. A Commander agent decomposes your goal into subtasks, N Worker agents execute them simultaneously, and a Synthesizer combines the results. Use it when a task is naturally divisible into independent pieces of work.

---

## `--app` vs bare `/batch`

```
/batch --app <goal>   → ClawTerminal's visual command center (Commander → Workers → Synthesizer)
/batch <goal>         → forwards the goal to Claude's own native subagent orchestration
```

Add `--app` whenever you want ClawTerminal's own multi-agent orchestration and its job-group UI. Without `--app`, the goal goes to Claude's built-in subagent behavior instead.

Orchestration flags (`--agents`, `--multi`, `--vcs`, `--ckpt`, `--routing`, `--skills`) only apply to ClawTerminal's own orchestration, so they require `--app`. Type one without `--app` and you'll get a **"did you mean `--app`?"** nudge:

```
/batch --agents 4 redesign the auth module
→ did you mean --app? Add it to run in the visual command center:
  /batch --app --agents 4 redesign the auth module
```

All the examples below use `--app` to run in ClawTerminal's command center.

---

## How it works

```
/batch --agents 4 redesign the authentication module

         Commander
         (decomposes goal into 4 subtasks)
              |
    +---------+---------+---------+
    |         |         |         |
 Worker 1  Worker 2  Worker 3  Worker 4
 (runs in   (runs in   (runs in  (runs in
  parallel)  parallel)  parallel) parallel)
    |         |         |         |
    +---------+---------+---------+
              |
          Synthesizer
          (merges all results)
```

---

## 1. Basic: parallel workers with default agent count

```
/batch --app Add error handling to all API endpoints in src/api/
```

ClawTerminal spawns 3 workers (the default). The Commander identifies which endpoints exist and assigns groups of them to each worker. Each worker adds try/except blocks, appropriate HTTP error responses, and logging.

**Good for:** Tasks that decompose naturally — "do X to each file/endpoint/module".

---

## 2. Control agent count with --agents

```
/batch --app --agents 5 Write integration tests for the REST API
```

5 workers run in parallel. The Commander assigns one or more API routes to each worker. If the API has 20 endpoints, each worker covers ~4 endpoints.

**Choosing an agent count:**
- 2–3 agents: small tasks, few files, quick wall-clock time
- 4–5 agents: medium tasks, 10–30 files
- 6–10 agents: large tasks (full codebase audits, sweeps across many modules)

Higher agent count means more parallelism but also more token cost. For most tasks, 4 agents is a practical default.

```
/batch --app --agents 2 Translate all Python docstrings to French
```

```
/batch --app --agents 8 Perform a code quality sweep across the entire codebase: fix linting issues, remove dead code, and add missing type annotations
```

---

## 3. Auto checkpoints with --ckpt

```
/batch --app --agents 4 --ckpt Build a REST API for user management
```

Every worker gets `--ckpt` enabled. In the Jobs panel, each worker's Job Detail shows a checkpoint timeline so you can see exactly how far each agent has progressed.

**Useful for:** Long-running batch jobs where you want granular visibility into each worker's progress, not just the overall status.

```
/batch --app --agents 3 --ckpt Refactor the monolithic UserService into separate read and write services. Output [CHECKPOINT: label] after each major step.
```

---

## 4. Cross-tool assignment with --multi

`--multi` lets the Commander assign different CLI tools to different subtasks based on their strengths. ClawTerminal detects which tools are installed on your Mac before the Commander runs.

```
/batch --app --agents 4 --multi Migrate the database schema from Postgres to SQLite
```

The Commander might assign:
- Worker 1 (Gemini) — read and analyze the full schema file (large context)
- Worker 2 (Aider) — rewrite migration scripts with git integration
- Worker 3 (Claude) — update ORM models and type annotations
- Worker 4 (Claude) — write tests for the migration

```
/batch --app --agents 3 --multi Add comprehensive logging to the backend
```

Possible assignment:
- Worker 1 (Gemini) — scan all files to map where logging should go
- Worker 2 (Aider) — add structured logging calls with git-tracked commits
- Worker 3 (Claude) — write the logging configuration and documentation

**When to use --multi:**
- Tasks that span code generation, large-file reading, and git-tracked edits
- Polyglot projects where different tools handle different languages better
- When you want Aider's git integration for the writing phase and Gemini's large context for the reading phase

---

## 5. With skills attached

```
/batch --app --agents 2 --skills tdd Implement shopping cart: add items, remove items, calculate totals, apply discount codes
```

Both workers get the TDD skill injected into their system prompt, so they follow a test-first approach for each piece of functionality.

```
/batch --app --agents 3 --skills security,backend Audit all API authentication endpoints for security vulnerabilities
```

All 3 workers have both the `security` and `backend` skills — they check for OWASP issues, timing attacks, and missing authorization checks with the domain knowledge from those skills.

---

## 6. Combining all flags

```
/batch --app --agents 5 --multi --ckpt --skills backend,testing Implement OAuth2 login flow with JWT tokens, refresh token rotation, and session revocation
```

This spawns:
- 1 Commander that decomposes the OAuth2 goal into 5 subtasks
- 5 Workers with tool assignments based on what's installed
- Checkpoints tracking each worker's progress
- `backend` and `testing` skills injected into every worker
- 1 Synthesizer that assembles the final result

---

## 7. View batch jobs in the Jobs panel

Open the Jobs panel (clipboard icon). Batch runs appear as a **collapsible group**:

```
  OAuth2 login flow implementation  •  5 agents  •  Running
  ▾
    Commander              Complete
    Worker 1 (Claude)      Running — "Write JWT token service..."
    Worker 2 (Aider)       Complete
    Worker 3 (Gemini)      Complete
    Worker 4 (Claude)      Running — "Write refresh token rotation..."
    Worker 5 (Claude)      Queued
    Synthesizer            Waiting
```

Color coding: Commander = gray, Workers = blue, Synthesizer = purple.

Tap any worker to read its individual result and checkpoint timeline.

---

## Choosing between /submit, /orchestrate, and /batch

| Scenario | Recommended |
|----------|-------------|
| Single focused task | `/submit` |
| Multi-perspective review (research + implement + review) | `/orchestrate` |
| Many independent parallel subtasks | `/batch` |
| Tasks with dependencies between phases | `/team` (see agent-teams.md) |

**Example decisions:**

"Add tests for the checkout service" → `/submit` (one task, one agent)

"Redesign the authentication system" → `/orchestrate` (benefits from the Researcher/Implementer/Reviewer roles)

"Add error handling to all 30 API endpoints" → `/batch --agents 6` (30 endpoints split across 6 workers)

"Build a new feature from scratch" → `/team` (Research the codebase first, then Implement, then Review)
