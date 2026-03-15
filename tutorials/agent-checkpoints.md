# Understanding Agent Checkpoints

> Track the progress of long-running background jobs with structured milestone markers that Claude outputs during execution.

---

## What Are Checkpoints?

When Claude works on a complex background job, it can output **checkpoint markers** --- structured labels that indicate it has completed a significant phase of work. ClawTerminal parses these markers from the job's progress log and displays them as a visual timeline in the Job Detail view.

A checkpoint looks like this in Claude's output:

```text
[CHECKPOINT: Analyzed existing auth module structure]
```

ClawTerminal detects any line matching the pattern `[CHECKPOINT: <label>]` and adds it to the job's checkpoint list with a timestamp.

---

## Why Checkpoints Matter

Without checkpoints, a long-running background job is a black box --- you see a stream of text in the progress log, but it is difficult to tell how far along Claude is or what major steps have been completed. Checkpoints solve this by giving you:

- **At-a-glance progress** --- see which phases are done without reading the full log
- **Time tracking** --- each checkpoint records when it was reached, so you can see how long each phase took
- **Resumability context** --- if a job fails partway through, the checkpoint timeline shows exactly where it stopped

---

## Viewing the Checkpoint Timeline

1. Open the **Jobs** panel (clipboard icon in the chat toolbar)
2. Tap a job that has checkpoints
3. In the **Job Detail** view, scroll down past the Request and Result cards
4. The **Checkpoints** section appears with a vertical timeline:

Each checkpoint entry shows:

| Element | Description |
|---------|-------------|
| Blue dot | Visual marker on the timeline |
| Label text | The checkpoint description (e.g. "Analyzed auth module") |
| Timestamp | When the checkpoint was reached |

The checkpoints appear in chronological order, earliest first.

---

## How Claude Outputs Checkpoints

Checkpoints are parsed from the raw progress log using the pattern:

```text
[CHECKPOINT: <any text here>]
```

Claude outputs these markers naturally when given tasks that have distinct phases. You can also explicitly instruct Claude to use checkpoints in your `/submit` request:

```text
/submit Refactor the authentication module. Output a [CHECKPOINT: label]
marker after completing each major step.
```

### Example Prompt

```text
/submit Perform a full security audit of the codebase.
For each phase, output a [CHECKPOINT: phase name] marker.

Phases:
1. Scan for hardcoded secrets
2. Check dependency vulnerabilities
3. Review authentication flows
4. Audit authorization checks
5. Check input validation
6. Review error handling for info leaks
```

### Example Output (in progress log)

```text
Starting security audit of ~/Projects/myapp...

Scanning all files for hardcoded secrets, API keys, and credentials...
Found 3 files with potential issues.

[CHECKPOINT: Scanned for hardcoded secrets]

Running npm audit and checking CVE databases...
2 high-severity vulnerabilities found in dependencies.

[CHECKPOINT: Checked dependency vulnerabilities]

Reviewing authentication flows in src/auth/...
OAuth2 implementation looks solid. Password hashing uses bcrypt with cost 12.

[CHECKPOINT: Reviewed authentication flows]

...
```

Each `[CHECKPOINT: ...]` line creates an entry in the timeline as soon as it appears in the streaming progress log.

---

## Automatic Checkpoint Detection (`--ckpt`)

Instead of asking Claude to manually output `[CHECKPOINT: ...]` markers, you can enable **automatic checkpoint detection** by adding the `--ckpt` flag to your `/submit` command:

```text
/submit --ckpt refactor the auth module to use async/await
```

When `--ckpt` is enabled, ClawTerminal scans the job's streaming output for natural progress indicators and creates checkpoint entries automatically.

### What Gets Detected

| Pattern | Example | Checkpoint Label |
|---------|---------|-----------------|
| Tool use events | Claude calls `Write(src/auth.ts)` | `Write: src/auth.ts` |
| Checkmark lines | `✅ Tests passing` | `Tests passing` |
| Step/Phase markers | `Step 3: Deploy` | `Step 3: Deploy` |
| Progress keywords | `Build succeeded` | `Build succeeded` |
| File operations | `Created file src/auth.ts` | `Created file src/auth.ts` |
| Test results | `All tests passed` | `All tests passed` |
| Build results | `BUILD SUCCEEDED` | `BUILD SUCCEEDED` |

### Deduplication and Limits

- Duplicate labels are automatically filtered --- if "Build succeeded" appears multiple times, only the first instance creates a checkpoint
- Maximum 50 auto-checkpoints per job to prevent flooding on verbose output
- Manual `[CHECKPOINT: ...]` markers still work alongside auto-checkpoints and always take priority

### Combining Manual and Automatic

You can use both approaches together:

```text
/submit --ckpt Perform a full security audit.
Output [CHECKPOINT: phase name] after each major phase.
```

In this case, both the manual `[CHECKPOINT: ...]` markers AND the automatically detected progress indicators appear in the checkpoint timeline.

### When Auto-Checkpoints Work Best

Auto-checkpoints are most useful for **coding tasks** where Claude uses tools (Write, Edit, Bash) and produces structured output. For pure text tasks (math, writing, analysis), Claude typically doesn't produce the patterns that trigger auto-detection --- use manual `[CHECKPOINT: ...]` markers instead.

### Visual Indicator

When a job has `--ckpt` enabled, the checkpoint timeline header in Job Detail shows **"Checkpoints (auto)"** instead of just "Checkpoints", so you can tell at a glance that auto-detection is active.

---

## Use Cases

### Long-Running Analysis

For jobs that take 10 minutes or more, checkpoints let you check in on progress without reading the entire log:

```text
/submit Analyze all 200+ source files in this project.
Group them by responsibility, identify dead code, and suggest a
reorganization plan. Output [CHECKPOINT: <area>] after each area.
```

### Multi-Phase Tasks

When a job has clearly defined sequential steps, checkpoints mark transitions between them:

```text
/submit Migrate the database schema from v2 to v3.
1. Back up current schema
2. Create migration scripts
3. Run migrations on test database
4. Validate data integrity
5. Generate rollback scripts
Output [CHECKPOINT: Step N complete] after each step.
```

### Orchestrated Jobs

Checkpoints also work inside orchestrated jobs (from `/orchestrate`). Each agent (Researcher, Implementer, Reviewer) can output its own checkpoints, and you can view them individually by tapping each agent's job in the orchestration group.

### Debugging Failed Jobs

When a job fails, the checkpoint timeline shows exactly how far it got:

- If the last checkpoint is "Step 3 complete" and there is no "Step 4 complete", you know the failure occurred during Step 4
- The timestamps show whether the failure was immediate or happened after a long processing period
- This information helps you craft a more targeted retry

---

## Tips

- **Be specific in labels:** Use descriptive checkpoint labels like `[CHECKPOINT: Migrated users table (1,247 rows)]` rather than vague ones like `[CHECKPOINT: Done with step 2]`
- **Not every job needs checkpoints:** For quick jobs (under 2 minutes), checkpoints add overhead without much benefit. Reserve them for jobs with distinct phases
- **Checkpoints are append-only:** Once a checkpoint is recorded, it stays in the timeline even if Claude continues to produce output. They are persisted with the job data
- **Skills can include checkpoint instructions:** If you frequently use checkpoints, create a custom Skill with a system prompt that instructs Claude to always output `[CHECKPOINT: ...]` markers for multi-step tasks
