# Workflow Automation

> **Why these matter:** These five features eliminate the small annoyances that interrupt your flow. Resolve merge conflicts with AI instead of a diff tool. Get push notifications when your tests finish instead of polling. Preview what Claude will change before it changes anything. Pull API docs into your prompt without copy-pasting. Keep key files in context automatically.

---

## /conflicts — AI Merge Conflict Resolver

Run `/conflicts` after any merge or rebase that produces conflicts:

```
/conflicts
```

CatClaw scans for files with git merge conflict markers (`<<<<<<<` / `=======` / `>>>>>>>`), reads each conflict block, and sends them to Claude for resolution suggestions. Each file appears as a card showing the ours/theirs sides and Claude's recommended resolution. Tap **Apply** to write the resolved content via SSH.

**After a `/batch --vcs` run:**

```
/batch --vcs --agents 3 Refactor authentication module
```

When agents merge back their branches, conflicts are likely. Run `/conflicts` immediately after:

```
/conflicts
```

Claude sees the full context of each conflict — including the branch names and surrounding code — and produces a resolution that respects both sides' intent.

**Reviewing before applying:**

The conflict card shows the original conflict markers, Claude's resolution, and the file path. You can edit the proposed resolution in the text view before tapping Apply. Tap **Skip** to leave a file unresolved and handle it manually in the terminal.

---

## /notify — Smart Notifications

Set up a monitoring rule with a plain English condition:

```
/notify when tests finish
/notify when server on port 3000 is up
/notify when deploy finishes
/notify when build succeeds
/notify when my_script.sh exits
```

CatClaw polls your Mac via SSH every 30 seconds, evaluating the condition. When met, a push notification fires — even if the app is in the background.

**List active rules:**

```
/notify list
```

**Remove all rules:**

```
/notify cancel
```

**How conditions are evaluated:**

| Condition type | SSH probe used |
|----------------|----------------|
| Port check (`when server on port N is up`) | `nc -z localhost N` |
| Process check (`when tests finish`) | `pgrep -f "pytest\|npm test\|cargo test"` exit code |
| Command check (`when deploy finishes`) | Checks if a known deploy process has exited |
| Generic (`when X`) | Claude generates a one-line shell probe at rule creation time |

Rules persist across app restarts. Each rule is evaluated independently and fires its notification exactly once when the condition first becomes true, then deactivates.

---

## /plan — Enhanced Dry-Run

Toggle plan mode before any message that will make changes:

```
/plan
```

An orange **Plan Mode** banner appears above the input bar. Your next message triggers Claude to generate a structured plan instead of making changes:

```
Refactor the UserAuth class to use async/await throughout
```

Claude responds with a plan card showing:

- **Files to modify** — list with paths and brief description of changes
- **Files to create** — any new files needed
- **Approach** — summary of the strategy
- **Risks** — things that could go wrong or need manual review
- **Estimated complexity** — rough effort estimate

Review the plan. If it looks right, tap **Execute** — CatClaw re-sends the original message in normal mode and Claude proceeds. If the scope is wrong, send a correction message while still in plan mode.

**Use plan mode for:**

- Large refactors touching many files
- Tasks where you want to confirm Claude's interpretation before it starts writing
- Exploring what changes a feature request would require without committing to them

---

## @web — URL Context Injection

Include web content in your messages by prefixing a URL with `@web`:

```
Implement this API: @web https://api.stripe.com/docs/payments
```

```
Check this RFC and update our implementation: @web https://datatracker.ietf.org/doc/html/rfc9110
```

```
What's changed in this changelog that affects us? @web https://github.com/some/lib/blob/main/CHANGELOG.md
```

CatClaw fetches the URL via SSH using `curl` on your Mac (so it uses your Mac's network, cookies, and DNS — not the phone's). HTML is stripped, content is truncated to 4000 chars, and the result is injected as a `<web_content url="...">` block before your message.

**Combine with file references:**

```
Implement this spec and update the existing client: @web https://docs.example.com/api/v2 and check @src/api/client.ts
```

**Works well for:**

- API documentation pages
- GitHub raw files (`https://raw.githubusercontent.com/...`)
- Plain-text changelogs, READMEs, RFCs
- Stack Overflow answers or blog posts you want Claude to reason about

**Does not work well for:**

- Pages requiring login or JavaScript rendering
- Very large pages (truncated at 4000 chars — link to a specific section instead)
- Binary content (images, PDFs)

---

## /pin — File Context Pinning

Keep a file always present in every message's context for the current chatroom:

```
/pin src/models/User.swift
/pin src/config.ts
/pin CLAUDE.md
```

Pinned files are read via SSH at send time and injected as `<pinned_file path="...">` blocks before your message — automatically, every time. A compact **Pins** chip in the chatroom toolbar shows the count of active pins and opens a management sheet.

**List pinned files:**

```
/pin
```

Shows each pinned path and its current size (so you can monitor context usage).

**Remove a pin:**

```
/unpin src/config.ts
```

**Remove all pins:**

Open the Pins sheet from the toolbar and tap **Clear All**.

**Good candidates for pinning:**

| File type | Why to pin |
|-----------|------------|
| `CLAUDE.md` / project instructions | Always keep Claude oriented to project conventions |
| Config / constants file | Referenced constantly; avoid attaching every time |
| Core model or schema file | Shared data structures that inform every change |
| `package.json` / `Podfile` | Dependency list Claude often needs for version-aware answers |
| Current task spec or ticket | Keep the requirements visible for the whole session |

**Tip:** For a focused work session, pin 2–3 key files at the start, then unpin them when switching to a different area of the codebase. Pinning too many large files will consume context budget — check the Pins sheet for sizes.
