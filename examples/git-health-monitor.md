# Git, Health & Monitor Commands

> **Why these matter:** These four commands give you instant visibility into your codebase, your server, and your git history -- without leaving the chatroom. Run `/health` before a refactor to know what you are working with. Run `/monitor` during a deploy to watch it land. Run `/search` to find code by asking questions, not writing regex.

## /git — Visual Branch Graph

```
/git
```

Opens a visual timeline of commits with branch tags. Tap a branch name to checkout.

### What you see

- A scrollable commit timeline with merge arrows connecting branches
- Each branch shown in a distinct color with its name as a tag
- Commit messages, authors, and timestamps on each node
- Tap any branch name to switch to that branch immediately

### Example use cases

```
/git
```
Browse all branches before deciding which to merge or delete.

```
/git
```
After a `/batch --vcs` run, use the git graph to inspect each agent's branch before the auto-merge completes.

---

## /health — Codebase Health Dashboard

```
/health
```

One-tap scan showing: lines of code, source file count, TODO/FIXME count, uncommitted changes, dependencies, last commit age, branch count, largest file.

### Sample output

```
Codebase Health
───────────────────────────────
Lines of code:        42,310
Source files:            284
TODO / FIXME count:       17
Uncommitted changes:       3 files
Open branches:             6
Last commit:           2 hours ago
Dependencies (npm):       48
Largest file:   src/ViewModels/ChatVM.swift  (1,842 lines)
```

### Example use cases

```
/health
```
Quick snapshot before starting a large refactor.

```
/health
```
After a big cleanup sprint — compare LOC and TODO count to verify improvement.

```
/health
```
Before a code review — know the scope of what reviewers are looking at.

---

## /monitor — Live Server Monitor

```
/monitor
```

Real-time dashboard: CPU usage with progress bar, memory usage, disk space, uptime, load average. Tap refresh for latest readings. Sparkline charts show trends.

### Sample output

```
Server Monitor
───────────────────────────────
CPU usage:        38%  ████████░░░░░░░░░░░░
Memory:           6.2 GB / 16 GB  (39%)
Disk:             124 GB / 500 GB (25%)
Uptime:           4 days, 12 hours
Load avg:         1.42  0.98  0.87
```

### Example use cases

```
/monitor
```
Watch CPU and memory during a deploy to confirm load normalizes after a restart.

```
/monitor
```
Check disk space before running a large batch agent job that generates many files.

```
/monitor
```
Spot memory leaks — open monitor before and after a long agent run to compare baseline vs current usage.

---

## /search — AI Code Search

```
/search where is the auth token validated
/search how does the payment flow work
/search find all API endpoints
```

Semantic search: grep for keywords, then Claude ranks and explains the top 5 most relevant matches.

### How it works

1. Your query is parsed for keywords
2. The keywords are used to grep across all source files in the current project directory
3. Claude receives the top grep matches and ranks them by relevance to your plain-English question
4. Results are shown with file paths, line numbers, and a brief explanation of why each match is relevant

### Example use cases

```
/search where is the rate limit error handled
```
Find error handling code without knowing the exact variable or function name.

```
/search how does session resumption work
```
Understand a system you did not write by asking a question about its behavior.

```
/search find all places that write to disk
```
Security audit — locate all file I/O in the codebase before a review.

```
/search which files import the auth module
```
Dependency mapping — find all consumers of a module before refactoring it.

### Tips

- Use plain English — you do not need to know the exact function name or variable
- Narrow scope: if results are too broad, add more context (`/search where is the *Stripe* payment flow handled`)
- Combine with `/batch`: run `/search` first to understand the codebase, then run `/batch` with that context to make targeted changes
