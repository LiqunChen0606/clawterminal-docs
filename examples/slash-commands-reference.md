# Slash Commands Reference

Every slash command available in ClawTerminal, grouped by category. Type any of these in the chatroom input bar.

---

## App built-in commands

These commands are handled by ClawTerminal itself and work in both CLI and API mode unless noted.

### Conversation management

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/model <name>` | — | Switch the active model mid-conversation | `/model claude-opus-4-6` |
| `/clear` | — | Clear the conversation and start fresh | `/clear` |
| `/compact` | — | Summarize and compress the conversation history | `/compact` |
| `/plan <task>` | — | Read files and propose a plan without executing anything (CLI mode only) | `/plan Migrate users table to PostgreSQL` |
| `/resume` | — | Resume the most recent Claude CLI session for this chatroom (CLI mode only) | `/resume` |

### Session info

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/context` | — | Show current session: project directory, session ID, model | `/context` |
| `/status` | — | Show SSH connection state and chatroom info | `/status` |
| `/cost` | — | Show estimated token usage and cost for this session | `/cost` |
| `/doctor` | — | Run diagnostics: SSH health, tmux session, Claude binary, session ID | `/doctor` |
| `/config` | — | Display current settings dump | `/config` |
| `/diff` | — | Show the last code changes Claude made | `/diff` |

### Content actions

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/copy` | — | Copy Claude's last response to clipboard | `/copy` |
| `/export` | — | Export the conversation (Markdown or PDF via share sheet) | `/export` |
| `/rename <name>` | — | Rename the current chatroom | `/rename Auth Refactor` |

### Memory

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/remember <fact>` | — | Save a fact to cross-session memory | `/remember always use pnpm, not npm` |
| `/forget <keyword>` | — | Remove memories matching the keyword | `/forget pnpm` |
| `/memories` | — | Open the Memory Library to browse and manage saved memories | `/memories` |

### Help

| Command | Flags | Description |
|---------|-------|-------------|
| `/help` | — | List all available slash commands |

---

## Background jobs

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/submit <task>` | `--ckpt`, `--skills alias,...` | Send a task to the background job queue. Notification fires when done. | `/submit Write tests for auth.py` |
| `/tasks` | — | List all background jobs and their status | `/tasks` |

**`/submit` flags:**

| Flag | Description | Example |
|------|-------------|---------|
| `--ckpt` | Enable automatic checkpoint detection on this job | `/submit --ckpt Refactor the database layer` |
| `--skills alias1,alias2` | Inject named skills into this job's context | `/submit --skills tdd Add login feature` |

---

## Agent orchestration

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/orchestrate <goal>` | — | Spawn 3 parallel agents (Researcher, Implementer, Reviewer) + a Synthesizer | `/orchestrate Redesign the auth module` |
| `/batch <goal>` | `--agents N`, `--multi`, `--ckpt`, `--skills alias,...` | Commander decomposes goal → N workers run in parallel → Synthesizer merges results | `/batch --agents 4 Add error handling to all API endpoints` |
| `/team <goal>` | `--multi` | Wave-based orchestration: Research → Implement → Review, with discoveries propagating between waves | `/team Build a REST API with tests and documentation` |

**`/batch` flags:**

| Flag | Description | Example |
|------|-------------|---------|
| `--agents N` | Number of parallel worker agents (2–10, default: 3) | `/batch --agents 5 Audit the codebase` |
| `--multi` | Cross-tool assignment — assigns Claude/Codex/Gemini/Aider to subtasks based on strengths | `/batch --multi --agents 4 Migrate schema` |
| `--ckpt` | Enable auto-checkpoints on all worker jobs | `/batch --ckpt Refactor payment module` |
| `--skills alias,...` | Inject named skills into all worker agents | `/batch --skills security Audit the API` |

**`/team` flags:**

| Flag | Description | Example |
|------|-------------|---------|
| `--multi` | Cross-tool assignment within each wave | `/team --multi Build a feature with tests` |

---

## AI analysis & automation

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/changelog [range]` | — | Generate structured release notes from git log, organized into Features, Improvements, Bug Fixes, and Other Changes | `/changelog v1.0..v1.1` |
| `/gentest <description>` | — | Generate comprehensive tests from a plain English description. Auto-detects your test framework (Jest, Vitest, pytest, XCTest, Mocha). | `/gentest the user authentication flow` |
| `/security` | — | Run a four-check security audit: dependency vulnerabilities, hardcoded secrets pattern grep, `.env` files tracked in git, and `.gitignore` gaps — with AI-prioritized results | `/security` |
| `/tribal [path]` | — | Run a background job to extract unwritten project knowledge: architecture decisions, gotchas, hidden dependencies, naming conventions, historical context | `/tribal` |
| `/spec <feature>` | — | Generate a formal requirements spec in EARS notation with files to create/modify, ordered tasks, acceptance criteria, and risks. Claude asks if you want to execute with agents after generating. | `/spec Add OAuth2 login with JWT refresh tokens` |
| `/graph <question>` | — | Query your codebase structure as a knowledge graph. Analyzes file tree, import map, and class/function definitions to answer architectural questions with file and line references | `/graph what depends on the UserManager class` |
| `/hooks add <name> "<pattern>" "<action>"` | — | Add a file-change hook that polls every 10 seconds and dispatches a background job when files matching the glob pattern are modified. 30-second cooldown per hook. | `/hooks add auto-test "*.swift" "run tests for changed files"` |
| `/hooks list` | — | Show all active hooks with their patterns, actions, and last-triggered time | `/hooks list` |
| `/hooks remove <name>` | — | Remove a hook by name | `/hooks remove auto-test` |
| `/hooks stop` | — | Stop all active hooks | `/hooks stop` |

**`/changelog` range syntax:**

| Range | Description | Example |
|-------|-------------|---------|
| (none) | Last 20 commits | `/changelog` |
| `v1.0..v1.1` | Between two tags | `/changelog v1.0..v1.1` |
| `HEAD~5..HEAD` | Last N commits | `/changelog HEAD~5..HEAD` |
| `main..feature` | Branch diff | `/changelog main..feature-branch` |

---

## CLI pass-through commands (CLI mode only)

These are forwarded directly to the Claude Code CLI running on your Mac. They work only in CLI mode with an active tmux session.

| Command | Description | Example |
|---------|-------------|---------|
| `/permissions` | List or modify Claude's permissions in this project | `/permissions` |
| `/security-review` | Ask Claude to review recent changes for security issues | `/security-review` |
| `/pr-comments` | Fetch and address open PR review comments | `/pr-comments` |
| `/review` | Run Claude's built-in code review | `/review` |
| `/login` | Log in to Anthropic (re-authenticate Claude CLI) | `/login` |
| `/logout` | Log out of Anthropic | `/logout` |
| `/mcp` | Manage MCP server connections | `/mcp` |
| `/init` | Create a CLAUDE.md file in the current project directory | `/init` |

---

## Custom slash commands

When you create Smart Commands (Settings > Custom Commands or via the `/` palette > Manage Commands), they appear here as user-defined slash commands. They show up in the slash command palette with your configured name.

**Examples of typical custom commands:**

| Command | What it does |
|---------|-------------|
| `/deploy <env> <branch>` | Deploy to the specified environment from a branch |
| `/test <scope>` | Run the test suite for a scope and report failures |
| `/review <target>` | Code review with the code-review skill auto-attached |
| `/audit <target>` | Security audit submitted as a background job |
| `/sweep <area>` | 5-agent background sweep for code quality |
| `/morning` | 3-agent daily briefing: git log + tests + TODOs |

Custom commands support named parameters, default values, background auto-submit, CLI tool overrides, skill injection, and auto-batch execution. See [smart-commands.md](smart-commands.md) for details.

---

## @ file references

Type `@` anywhere in your message to open the remote file picker — a live SSH directory browser of your Mac:

1. Type `@` → a file tree appears
2. Navigate to the file
3. Tap it — the file's contents are injected as a `<file_attachment>` block

**Example:**
```
Review this config file for security issues @config/database.yml
```

Works in both CLI and API mode.

---

## Slash command palette

Type `/` in the chat input to open the slash command palette. It shows:
- All built-in commands with a short description
- Your custom commands
- Fuzzy search — type a few letters to filter

Tap any command to insert it, then fill in arguments.

The palette sizes to the number of matching results — it won't take up the full screen when there are only a few options.
