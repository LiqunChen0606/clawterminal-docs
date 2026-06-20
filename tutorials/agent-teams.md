# Agent Teams (`/team`) — Tutorial

ClawTerminal **Agent Teams** is a wave-based multi-agent orchestration system designed for complex tasks where each phase of work needs to build on the discoveries of the previous one. A Commander agent decomposes your goal into sequential waves (Research → Implementation → Review). Agents within each wave run in parallel. When a wave completes, discoveries are extracted from its output and injected into the next wave's prompts — so knowledge accumulates as the team progresses.

The entire run is visualized in the **Visual Command Center**, a mobile-first animated UI showing agent flow, live discoveries, and per-agent status — something you cannot get from Claude Code or any desktop-only tool.

---

## How to Use `/team`

Type `/team` followed by your goal in any ClawTerminal chatroom:

```text
/team <your goal>
```

For cross-tool assignment (Claude for reasoning, Aider for git edits, Gemini for large file reads):

```text
/team --multi <your goal>
```

---

## Bare `/team` vs `/team --app`

There are now two ways to run a team, and the difference matters:

- **`/team --app <goal>`** opens ClawTerminal's **Visual Command Center** — the wave/Kanban agent graph described in this tutorial, with the animated flow graph, live discovery feed, and per-agent status cards. Use `--app` whenever you want the in-app orchestration and its mobile-first visualization.
- **Bare `/team <goal>`** (no `--app`) forwards your goal to **Claude's own native subagent orchestration** instead. You get Claude's built-in team behavior rather than ClawTerminal's visual command center.

If you type an orchestration flag (`--agents`, `--multi`, `--vcs`, `--ckpt`, `--routing`, `--skills`) **without** `--app`, ClawTerminal shows a **"did you mean `--app`?"** nudge — those flags configure ClawTerminal's own orchestration, so they're only meaningful alongside `--app`. All of those flags work normally once you add `--app`:

```text
/team --app --multi --vcs Refactor the payment module to use the repository pattern
```

The rest of this tutorial describes the `--app` experience — the Visual Command Center, waves, and discovery propagation.

### Example

```text
/team --app Write a Python calculator with add, subtract, multiply, divide, input validation, and a REPL loop
```

ClawTerminal will:

1. Spawn a Commander agent that reads your goal and designs a wave plan
2. Execute Wave 1 (Research) — agents explore the codebase and gather context
3. Extract discoveries from Wave 1 results
4. Execute Wave 2 (Implementation) — agents receive Wave 1 discoveries and build the solution
5. Extract discoveries from Wave 2 results
6. Execute Wave 3 (Review) — agents receive all prior discoveries and verify correctness
7. Post a final summary to your chatroom

---

## The Three Waves

### Wave 1 — Research

Research agents explore the problem space and gather everything the implementation needs to know:

- Scan existing codebase for relevant files, patterns, and conventions
- Identify constraints (Python version, existing libraries, style guide)
- Note edge cases and requirements not explicitly stated in the goal

**Example discoveries from Wave 1 for the Python calculator:**
- `Project uses Python 3.11+ (f-strings, match/case available)`
- `No existing math utilities — build from scratch`
- `Input validation should handle non-numeric strings and division by zero`
- `REPL should support q/quit/exit to terminate`

### Wave 2 — Implementation

Implementation agents receive Wave 1 discoveries as context, so they already know the constraints and design decisions. They write code, create files, and complete the actual work:

- Write the requested code with full knowledge of project conventions
- Handle edge cases identified in Wave 1
- Create or modify files as needed

**Example discoveries from Wave 2:**
- `calculator.py created at ~/Projects/calculator/calculator.py`
- `Operations: add, subtract, multiply, divide implemented as pure functions`
- `Input validation handles ValueError (non-numeric) and ZeroDivisionError`
- `REPL accepts q, quit, exit to stop`

### Wave 3 — Review

Review agents receive all prior discoveries (Wave 1 + Wave 2) and verify the work:

- Check that all requirements from the goal are satisfied
- Look for bugs, edge cases, and security issues
- Verify code style and documentation
- Suggest improvements

The final wave's results are synthesized into a summary posted to your chatroom.

---

## How Discoveries Propagate Between Waves

After each wave completes, ClawTerminal extracts a structured list of discoveries from each agent's output. These are short factual observations — file paths created, design decisions made, constraints found, bugs identified. They are injected into the next wave's system prompt as a `<team_discoveries>` block:

```
<team_discoveries>
Wave 1 — Research:
• Project uses Python 3.11+
• No existing math utilities found
• Input validation must handle ValueError and ZeroDivisionError
• REPL should support q/quit/exit

Wave 2 — Implementation:
• calculator.py written at ~/Projects/calculator/calculator.py
• add(), subtract(), multiply(), divide() implemented as pure functions
• REPL loop implemented with try/except for robust input handling
</team_discoveries>
```

This means each wave is smarter than the last — agents do not repeat work already done, and they act on findings that would have been impossible to know before prior waves ran.

---

## The Visual Command Center

While a team is running (and any time after), tap the **Team** button in the chatroom toolbar to open the Visual Command Center.

### What you see

**Animated flow graph**

A live graph of nodes and connections representing your team:

- Each agent appears as a circular node labeled with its wave and role
- Nodes pulse when the agent is actively running
- Animated data-flow lines appear between waves when discoveries are being extracted and injected
- Completed nodes show a checkmark; failed nodes show a warning badge

**Live discovery feed**

A scrolling real-time stream of discoveries as they are extracted from completed agents. Each entry shows:

- Which wave and agent produced it
- The discovery text
- A timestamp

**Per-agent status cards**

Below the flow graph, each agent has a status card showing:

- A circular progress ring indicating completion percentage
- Current status: Queued / Running / Completed / Failed
- Assigned tool (Claude, Codex, Gemini, or Aider when `--multi` is set)
- A preview of the agent's most recent output lines

### When no team is running

The Team toolbar button is always visible. When no team is active, tapping it shows an empty state with brief instructions on how to start a team with `/team`.

---

## The `--multi` Flag: Cross-Tool Assignment

By default all agents use the chatroom's current CLI tool (usually Claude). With `--multi`, the Commander assigns the most suitable tool to each agent based on its role in the wave:

| Tool | Best for |
|------|----------|
| **Claude** | Code reasoning, architecture decisions, documentation, review |
| **Codex** | Focused code generation and completions |
| **Gemini** | Long-context analysis, reading large files, broad codebase surveys |
| **Aider** | Git-integrated refactoring, multi-file edits with commit tracking |

```text
/team --app --multi Refactor the payment module to use the repository pattern
```

With `--multi`, a Research wave might assign Gemini to read all payment-related files at once, an Implementation wave might assign Aider to make the git-tracked edits, and a Review wave might assign Claude to check architecture correctness.

ClawTerminal automatically detects which tools are installed on your Mac before the Commander runs.

---

## Comparison: Agent Teams vs Other Orchestration Options

| | `/orchestrate` | `/batch` | `/team` | Claude Code Agent Teams |
|---|----------------|----------|---------|------------------------|
| **Structure** | Fixed: Researcher / Implementer / Reviewer | Dynamic decomposition | Wave-based sequential phases | Desktop CLI only |
| **Parallelism** | All 3 at once | All workers at once | Parallel within each wave | Parallel (no phases) |
| **Knowledge sharing** | None — agents are independent | None | Discoveries flow between waves | None |
| **Multi-tool** | No | `--multi` flag | `--multi` flag | No |
| **Visual UI** | Jobs panel list | Jobs panel list | Animated command center with flow graph, discovery feed, per-agent status | CLI text output only |
| **Mobile-first** | Yes | Yes | Yes — designed for iPhone/iPad | No — desktop terminal only |
| **Best for** | Quick multi-perspective review of a single topic | Flexible parallel execution of many subtasks | Complex tasks where each phase needs findings from the last | N/A (not available on mobile) |

**Key difference from Claude Code Agent Teams:** ClawTerminal Agent Teams run entirely on your Mac over SSH — your phone is the command center. The Visual Command Center is purpose-built for mobile: an animated touch-friendly UI that shows you exactly what your agents are doing and what they have discovered, all on a 6-inch screen. Claude Code Agent Teams produce desktop-only CLI text with no equivalent mobile interface.

---

## Comparison with `/batch`

| | `/batch` | `/team` |
|---|----------|---------|
| **Decomposition** | Commander writes subtasks, all workers start at once | Commander writes waves, waves execute sequentially |
| **Knowledge flow** | Workers are independent | Wave N discoveries feed into Wave N+1 |
| **Best for** | Many parallel independent subtasks | Tasks where research must complete before implementation |
| **Example** | "Audit security across 20 endpoints" (each endpoint is independent) | "Build a new feature" (understand first, then implement, then review) |

Both support `--multi` and both post a final summary to your chatroom. Use `/batch` when tasks are independent; use `/team` when each phase depends on the previous one.

---

## Tips

- **Keep goals concrete** — `/team` works best when the goal has a clear deliverable. Vague goals like "improve the app" produce vague waves. Specific goals like "add rate limiting to the API with a Redis backend" produce focused waves.
- **Watch the discovery feed** — If you see discoveries that look wrong (e.g. Wave 1 identified the wrong project directory), you can cancel the team and retry with a more specific goal or a corrected Project Directory in the chatroom Info panel.
- **Use `--multi` for large codebases** — Assigning Gemini to Research waves lets it ingest many files at once before Claude takes over for implementation.
- **Review wave is your safety net** — If Wave 2 agents made a mistake, Wave 3 agents catch it. The final summary always reflects what the Review wave found.
- **The Team button is always there** — Even after a team finishes, tap the Team button to replay the flow graph and re-read the discovery feed. This is useful for understanding what happened in a long run.
