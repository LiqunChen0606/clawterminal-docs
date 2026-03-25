# AI Chatroom Examples

Practical examples for chatting with Claude and other AI tools in ClawTerminal. Covers CLI mode vs API mode, switching models, using plan mode, and managing the conversation.

---

## 1. CLI mode vs API mode

ClawTerminal supports two ways to talk to Claude. Understanding the difference helps you get the most out of each.

| | CLI mode | API mode |
|---|----------|----------|
| Runs on | Your Mac (over SSH) | Anthropic's servers (via API key) |
| Requires | SSH connection + Claude Code CLI installed | Anthropic API key in Settings |
| Tool use | Full — Claude can read/write files, run commands | Limited to what the API supports |
| Session persistence | tmux session survives disconnection | No persistence |
| Best for | Coding, file editing, running tests | Quick Q&A when not near your Mac |

**Switch to CLI mode:**
Tap the mode banner at the top of the chatroom → select **CLI**.

**Switch to API mode:**
Tap the mode banner → select **API**. You'll need an Anthropic API key in Settings > API Key.

**Recommended setup for developers:**
Use CLI mode with tmux chatroom enabled (Settings > Advanced > Enable Tmux Chatroom). This gives Claude the ability to read your codebase, run tests, and edit files.

---

## 2. Switch models

Type `/model` followed by a model name to change which model handles your next message. The switch takes effect immediately — no need to start a new conversation.

**Switch to Opus for a complex refactor:**
```
/model claude-opus-4-6
```

**Switch to Sonnet (balanced speed/quality):**
```
/model claude-sonnet-4-6
```

**Switch to Haiku for quick questions:**
```
/model claude-haiku-3-5
```

**Check which model is active:**
```
/context
```

The response shows your current model, session ID, and project directory.

**Tip:** Opus is slower but handles complex multi-file refactors better. Haiku responds in seconds for simple lookups. Switch to Opus for your big task, then switch back to Sonnet for the rest of the session.

---

## 3. Switch CLI tools (Codex, Gemini, Aider)

Each chatroom is bound to a CLI tool. To use a different tool, open the chatroom's Info panel and change the tool setting — or create a separate chatroom for each tool.

**Create an Aider chatroom for git-integrated edits:**
1. Tap **+** to create a new chatroom
2. Name it "Aider — myapp"
3. Tap the Info (i) button
4. Under CLI Tool, select **Aider**
5. Set Project Directory to `~/Projects/myapp`

Now messages in this chatroom run `aider` on your Mac, with its git integration for clean diffs and commits.

**Create a Gemini chatroom for large-file analysis:**
1. Create a new chatroom, name it "Gemini"
2. Info > CLI Tool > **Gemini**
3. Set Project Directory as needed

Gemini is well-suited for reading large files or broad codebase surveys before handing work back to Claude for implementation.

**Practical split:**
- Claude chatroom — architecture decisions, code review, documentation
- Aider chatroom — make the actual edits and commits
- Gemini chatroom — analyze large codebases before planning

---

## 4. Use plan mode before making changes

Plan mode tells Claude to read files and propose a plan but not execute any writes or commands. Use it when you want to review the approach before Claude touches your codebase.

**Example: Plan a database migration before running it**
```
/plan Migrate the users table to add a nullable `display_name` column and backfill it from the `full_name` field. Show me the migration steps and which files will be affected.
```

Claude responds with a structured plan listing what it would do, which files it would edit, and what SQL it would run — without touching anything. Once you're satisfied, send a follow-up:

```
Looks good. Go ahead and implement it.
```

**Example: Plan a refactor**
```
/plan Refactor the authentication module to extract the token validation logic into a separate TokenValidator class. What files will change and what's the migration path?
```

---

## 5. Compact the conversation context

When a conversation grows long, use `/compact` to summarize it and free up context window space. Claude produces a compressed summary and continues from there.

```
/compact
```

**When to use it:**
- After a long debugging session that's been resolved — the history is noise now
- Before starting a new sub-task within the same session
- When Claude starts forgetting earlier parts of the conversation

**What happens:**
Claude reads the full conversation, writes a concise summary of decisions made and current state, then replaces the conversation history with that summary. The session ID is preserved, so `--resume` still works.

---

## 6. Copy and export responses

**Copy Claude's last response to clipboard:**
```
/copy
```

The full text of Claude's most recent message is copied. Paste it into Notes, Slack, or an email.

**Export the full conversation:**
```
/export
```

A share sheet appears with two format options:
- **Markdown** — raw `.md` file, preserves code blocks and headings
- **PDF** — formatted A4 document, suitable for sharing with non-technical stakeholders

**Alternatively**, tap the **Export** button in the scrollable chat toolbar (it appears next to the Memory, Skills, Jobs, and Snippets buttons).

**Example workflow — document a debugging session:**
1. Work through a tricky bug with Claude in CLI mode
2. Once resolved, type `/export`
3. Choose Markdown
4. Share to your team's Slack channel or save to a wiki

---

## 7. Check token usage and cost

```
/cost
```

Shows estimated token usage for the current session and an estimated cost based on the active model's pricing. Useful for keeping API costs in check.

In API mode, a live usage bar appears below the mode banner showing token usage as a percentage of the model's context window, color-coded green → yellow → red.

---

## 8. Run diagnostics if something feels off

```
/doctor
```

Runs a set of checks: SSH connection state, tmux session health, Claude CLI binary detection, session ID validity, and more. The output tells you exactly what's working and what needs attention.

**Common fix after /doctor output:**
If it reports "no active tmux session", your session was killed (Mac restarted, tmux server died). Just send any message — ClawTerminal automatically creates a new session.
