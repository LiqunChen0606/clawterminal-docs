# Advanced Agent Features

## Git Worktree Mode

```
/batch --agents 3 --vcs Refactor the auth module
```

Each agent gets its own git branch and worktree. After all agents complete, branches auto-merge back. Conflicts are reported and passed to the synthesizer for resolution.

### How it works

1. Before dispatching agents, the Commander creates N git branches (one per worker)
2. Each worker agent checks out its assigned branch via a new worktree — all agents work in parallel without touching each other's files
3. When all workers complete, the branches are merged back sequentially
4. If merge conflicts arise, the conflicting files and diff are passed to the Synthesizer agent as additional context for resolution

### Example use cases

```
/batch --agents 3 --vcs Refactor the auth module into smaller files
```
Three agents each try a different decomposition approach. The best parts of each can be cherry-picked by the Synthesizer.

```
/batch --agents 4 --vcs Add unit tests to all service files
```
Agents work on different service files simultaneously in isolated branches — no risk of one agent's test scaffold breaking another's.

```
/batch --agents 2 --vcs --ckpt Migrate database schema to the new format
```
Two agents tackle different tables in parallel. Checkpoints track progress. Worktree isolation means a failed migration on one branch does not affect the other.

### After the run

- Open `/git` to see the merged result and each agent's branch in the commit graph
- If the auto-merge produced a result you do not want, the individual branches are still available — you can reset and try again
- Conflict resolution notes from the Synthesizer appear in the job detail view

---

## Smart Model Routing

```
/batch --agents 4 --routing balanced Build a REST API
/team --routing budget Research our codebase architecture
/batch --agents 3 --routing quality Write production-ready auth system
```

### Routing Presets

| Preset | Commander | Workers | Research agents | Synthesizer |
|--------|-----------|---------|-----------------|-------------|
| `quality` | Opus | Opus | Opus | Opus |
| `balanced` | Opus | Sonnet | Haiku | Opus |
| `budget` | Sonnet | Haiku | Haiku | Sonnet |

### When to use each preset

**`--routing quality`** — production code generation, security-sensitive changes, complex architectural decisions. Every agent uses the most capable model available.

**`--routing balanced`** — the default for most tasks. Commander and Synthesizer use the high-end model for planning and merging. Workers use a mid-tier model for execution. Research agents use the fastest model for information gathering. Typically 40-60% cheaper than Quality for similar results.

**`--routing budget`** — exploratory research, first-pass drafts, tasks where speed matters more than perfection. Lowest cost. Good for `/team` research waves and brainstorming runs.

### Example: research-first, then quality implementation

```
/team --routing budget Research the best approach for migrating our API to GraphQL
```
First, run a cheap research pass to understand the landscape.

```
/batch --agents 3 --routing quality Implement the GraphQL migration based on the research
```
Then run the implementation with full quality, informed by the research findings.

---

## Agent Reasoning Banner

After any background job completes, a purple "Agent Reasoning" card appears in the job detail view showing why the agent made key decisions. Extracted from thinking blocks automatically.

### What it shows

- The agent's high-level approach before it started executing
- Key decision points: why it chose one file over another, why it picked a particular implementation strategy
- Uncertainty it encountered and how it resolved it
- A summary of what it believes it accomplished

### Using reasoning to improve prompts

If the agent misunderstood your task, the reasoning card shows you exactly where the misinterpretation happened. Use that to rewrite your prompt with more specificity before re-running.

If the agent succeeded but took an unexpected approach, the reasoning card explains its logic — which can surface better patterns you had not considered.

---

## Combining Features

```
/batch --agents 4 --vcs --routing balanced --ckpt Build a user dashboard
```

Full power: 4 agents, each in their own git branch, cost-optimized model routing, auto-checkpoint tracking.

```
/batch --agents 3 --vcs --routing quality --skills deploy-checklist Refactor the payment module
```

Worktree isolation + quality models + a custom skill injected into every agent's context.

```
/team --routing balanced --multi Research and implement a caching layer
```

Wave-based team orchestration with cross-tool model assignment and cost-optimized routing.

### Flag compatibility reference

| Flag | Works with `/batch` | Works with `/team` | Notes |
|------|---------------------|---------------------|-------|
| `--agents N` | Yes | No (wave counts set by Commander) | 2–10 agents |
| `--vcs` | Yes | No | Creates git worktrees per worker |
| `--routing preset` | Yes | Yes | quality / balanced / budget |
| `--multi` | Yes | Yes | Cross-tool model assignment |
| `--ckpt` | Yes | No | Auto-checkpoint detection |
| `--skills alias,...` | Yes | No | Inject skills into all agents |
