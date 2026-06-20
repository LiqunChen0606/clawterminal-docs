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

### Session intelligence

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/recap` | — | Session summary: message count, job stats, recent topics, session cost | `/recap` |
| `/context` | — | Visual context window usage bar with percentage and suggestions | `/context` |
| `/effort low\|medium\|high` | — | Dial thinking depth without switching models | `/effort high` |
| `/btw <question>` | — | Quick one-sentence side question without derailing the main conversation | `/btw what port does Postgres use by default?` |
| `/retry` | — | Remove the last exchange and re-send for a different response | `/retry` |
| `/personality [name]` | — | Switch agent persona: `senior`, `mentor`, `reviewer`, `architect`, `hacker`, or `default`. No argument lists all. | `/personality architect` |
| `/soul <description>` | — | Define a persistent agent identity for this chatroom that survives session resets | `/soul You are a senior engineer at a fintech startup. Security is never optional.` |
| `/think [question]` | — | Request an explicit reasoning chain; thinking blocks render as collapsible in the message bubble | `/think Should we use JWT or session cookies?` |
| `/trace` | — | Re-render the last agent's full thinking trace in collapsible blocks | `/trace` |

### Learning mode commands

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/duck` | — | Switch to Socratic rubber-duck personality — Claude only asks probing questions, never gives direct answers | `/duck` |
| `/duck off` | — | Exit rubber-duck mode and return to normal Claude | `/duck off` |
| `/tutorial` | — | List all interactive tutorial tracks | `/tutorial` |
| `/tutorial <track>` | — | Start a specific track: `beginner`, `terminal`, `workflows`, `agents`, `devops`, or `power` | `/tutorial beginner` |
| `/tutorial reset` | — | Reset progress for all tutorial tracks | `/tutorial reset` |

### Dream Mode

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/dream` or `/dream show` | — | View your learned preference profile | `/dream show` |
| `/dream now` | — | Run a dream cycle immediately (Haiku SSH call) | `/dream now` |
| `/dream reset` | — | Clear the profile and start fresh | `/dream reset` |

### Frugal Mode

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/frugal` | — | Toggle chatroom-level Frugal Mode on/off. Strips skills, memory, pinned files, dream profile, device context, tribal knowledge | `/frugal` |
| `!<message>` | — | One-off frugal send — applies Frugal Mode to this single message without changing the chatroom setting | `!what's the default Postgres port?` |

### Tool Installers

Install, uninstall, or check the status of third-party AI CLI tools on your Mac via SSH. Each subcommand opens a confirmation card with the exact shell command preview before anything runs; on confirm, the work dispatches as a background job (output streams to the Jobs tab, push notification on completion).

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/openclaw` | — | Show the OpenClaw description, repo link, and the list of subcommands | `/openclaw` |
| `/openclaw install` | — | Install the OpenClaw CLI globally via npm and run the onboarding daemon installer | `/openclaw install` |
| `/openclaw uninstall` | — | Stop the OpenClaw launchd daemon and `npm uninstall -g openclaw` | `/openclaw uninstall` |
| `/openclaw status` | — | Print the OpenClaw binary path and version | `/openclaw status` |
| `/hermes` | — | Show the Hermes Agent description, repo link, and the list of subcommands | `/hermes` |
| `/hermes install` | — | Install the Hermes Agent CLI via the upstream `install.sh` script | `/hermes install` |
| `/hermes uninstall` | — | Remove the `~/.local/bin/hermes` symlink and the `~/.hermes-agent/` venv | `/hermes uninstall` |
| `/hermes status` | — | Print the Hermes binary path and version | `/hermes status` |
| `/notifier` | — | Show the ClawNotifier description and the list of subcommands | `/notifier` |
| `/notifier install` | — | Install a launchd daemon on your Mac that fires native macOS notifications when background jobs complete (no cloud, no APNs). Polls `/tmp/.claw_out_*.txt` every 2s for completion sentinels | `/notifier install` |
| `/notifier uninstall` | — | Bootout the launchd agent and remove `~/.clawnotifier` and the launch agent plist | `/notifier uninstall` |
| `/notifier status` | — | Check `launchctl list` for the daemon and tail the daemon log | `/notifier status` |
| `/notifier setup` | — | Open the APNs setup wizard — Mac-hosted Apple Push Notifications for instant native iPhone banners. Requires an Apple Developer account + .p8 auth key | `/notifier setup` |
| `/notifier telegram` | — | Open the Telegram bot setup wizard — universal instant iPhone push via your own Telegram bot. No Apple Developer account required | `/notifier telegram` |
| `/simpletes` | — | Show the SimpleTES description and the list of subcommands | `/simpletes` |
| `/simpletes install` | — | Install SimpleTES on your Mac via `git clone` + `uv sync` (auto-installs `uv` if missing). Pairs with `/research` for evaluation-driven discovery | `/simpletes install` |
| `/simpletes uninstall` | — | Remove `~/.simpletes` | `/simpletes uninstall` |
| `/simpletes status` | — | Confirm SimpleTES is installed by listing the package via `uv pip list` | `/simpletes status` |
| `/channels install` | — | Install the Mac-side bridge for the **Channels** transport (Beta) — drives a Claude chatroom through a documented local bridge instead of an SSH port forward, removing inbound-network setup. Requires a recent Claude Code; the confirmation card prints the `claude` launch command + development flag. Enable in **Settings → AI Intelligence** (off by default). Removes the network setup, not the Mac — `claude` still runs locally, and SSH stays the default for the terminal, file browser, and other AI tools | `/channels install` |

**Repos:** [openclaw/openclaw](https://github.com/openclaw/openclaw) · [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) · [wq-will/SimpleTES](https://github.com/wq-will/SimpleTES)

### Research

Evaluation-driven discovery via parallel propose-evaluate-refine. Two modes: **Native** (default — runs in-app via the Anthropic API, no install required) and **SimpleTES (Mac)** (runs `~/.simpletes` for code-execution evaluators; requires `/simpletes install` first).

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/research <task>` | — | Open the research sheet with a Mode picker (Native vs SimpleTES). **Native mode (default):** in-app propose-evaluate-refine loop via the Anthropic API — about $0.005–0.02 per run on Haiku, with live progress and a full trajectory graph in the result sheet. Best for text-heavy tasks (prompt engineering, naming, design exploration). **SimpleTES mode:** Haiku translates the task into `init_program.py` + `evaluator.py` and dispatches a propose-evaluate-refine search as a background job on your Mac — pick this for code-execution evaluators with measurable metrics. | `/research find a more space-efficient bloom filter` |

### Search Mode

Per-chatroom toggle that routes every regular user message through the Native Research Engine instead of single-shot Claude. When on, each send becomes a parallel propose-evaluate-refine search and the best result is returned with a score + cost. The indigo 🔍 chip in the mode banner shows the room is in search mode.

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/search on` | — | Enable Search Mode for this chatroom — every regular message becomes a parallel propose-evaluate-refine search via the Native Research Engine. Tap the result to open the trajectory graph (chains × rounds, best attempt highlighted in gold). | `/search on` |
| `/search off` | — | Disable Search Mode — messages return to single-shot Claude. | `/search off` |

You can also enable Search Mode at room creation: tap **+** to open the new-chatroom sheet and tick the third toggle ("Search Mode — every message becomes a parallel search").

### Help

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/help` | — | List all available slash commands | `/help` |
| `/docs` | — | Open the Learn Hub to browse tutorials, docs, and examples (all searchable). Optional arg: `tutorials`, `examples`, `interactive`, or a search query. | `/docs` |
| `/docs tutorials` | — | Open the Learn Hub filtered to the Tutorials category | `/docs tutorials` |
| `/docs examples` | — | Open the Learn Hub filtered to the Examples category | `/docs examples` |
| `/docs interactive` | — | Open the Learn Hub filtered to interactive tutorial tracks | `/docs interactive` |
| `/docs <query>` | — | Open the Learn Hub pre-filtered by substring search on titles + summaries | `/docs frugal` |

---

## Developer productivity

### Standup & reporting

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/standup` | — | Auto-write a 3-section standup (Yesterday / Today / Blockers) from the last 24h of chatroom activity + `git log`. Clipboard-ready. Local fallback without API key | `/standup` |
| `/wrapped` or `/wrapped week` | — | Spotify-Wrapped-style 9-page swipeable recap of the last 7 days — top commands, spend, peak hour, topics, longest session, share card | `/wrapped` |
| `/wrapped month` | — | Same as above, 30-day window | `/wrapped month` |

### Shell safety

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/whatif <command>` | — | Predict the 3 most likely outcomes of a shell command before you run it. Color-coded severity (green/yellow/red) with probability %. Uses git status context | `/whatif git reset --hard HEAD~3` |

### Visual memory

| Command | Flags | Description | Example |
|---------|-------|-------------|---------|
| `/screenshot` | — | Capture the current chatroom view as a PNG; auto-tag by topic locally | `/screenshot` |
| `/screenshots` | — | Open the screenshot gallery with thumbnails, search, filters | `/screenshots` |

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
| `/batch <goal>` | `--app`, `--agents N`, `--multi`, `--ckpt`, `--skills alias,...` | With `--app`: Commander decomposes goal → N workers run in parallel → Synthesizer merges results in the visual command center. Without `--app`: forwards the goal to Claude's own native subagent orchestration | `/batch --app --agents 4 Add error handling to all API endpoints` |
| `/team <goal>` | `--app`, `--multi` | With `--app`: wave-based orchestration (Research → Implement → Review) in the Visual Command Center, discoveries propagating between waves. Without `--app`: forwards the goal to Claude's own native subagent orchestration | `/team --app Build a REST API with tests and documentation` |

> **`--app` opens ClawTerminal's own orchestration.** Bare `/team` and `/batch` forward your goal to Claude's native subagent orchestration. The orchestration flags (`--agents`, `--multi`, `--vcs`, `--ckpt`, `--routing`, `--skills`) configure ClawTerminal's own engine, so they require `--app` — typing one without `--app` shows a "did you mean `--app`?" nudge.

**`/batch` flags** (all require `--app`):

| Flag | Description | Example |
|------|-------------|---------|
| `--app` | Run in ClawTerminal's visual command center instead of forwarding to Claude's native orchestration. Required for all flags below | `/batch --app Audit the codebase` |
| `--agents N` | Number of parallel worker agents (2–10, default: 3) | `/batch --app --agents 5 Audit the codebase` |
| `--multi` | Cross-tool assignment — assigns Claude/Codex/Gemini/Aider to subtasks based on strengths | `/batch --app --multi --agents 4 Migrate schema` |
| `--ckpt` | Enable auto-checkpoints on all worker jobs | `/batch --app --ckpt Refactor payment module` |
| `--skills alias,...` | Inject named skills into all worker agents | `/batch --app --skills security Audit the API` |

**`/team` flags** (require `--app`):

| Flag | Description | Example |
|------|-------------|---------|
| `--app` | Run in the Visual Command Center instead of forwarding to Claude's native orchestration. Required for the flag below | `/team --app Build a feature` |
| `--multi` | Cross-tool assignment within each wave | `/team --app --multi Build a feature with tests` |

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

ClawTerminal is a **superset** of the Claude Code command surface. Any slash command your Mac's `claude` session supports — including commands ClawTerminal has no custom card for, and new ones added in future CLI releases — is forwarded straight through and rendered in the rich card view, instead of an "Unknown command" error. You no longer wait for an app update to use a new CLI command.

Forwarding works only in CLI mode with an active tmux session (there's no CLI to forward to in Direct API mode).

Some Claude Code commands are **interactive-only** — they need a real terminal (TTY). Instead of failing, these show a one-tap "Open in Terminal tab" affordance: `/config`, `/mcp`, `/agents`, `/login`, `/model`.

Common forwarded commands:

| Command | Description | Example |
|---------|-------------|---------|
| `/permissions` | List or modify Claude's permissions in this project | `/permissions` |
| `/security-review` | Ask Claude to review recent changes for security issues | `/security-review` |
| `/pr-comments` | Fetch and address open PR review comments | `/pr-comments` |
| `/review` | Run Claude's built-in code review | `/review` |
| `/login` | Log in to Anthropic (re-authenticate Claude CLI) — interactive, opens the Terminal tab | `/login` |
| `/logout` | Log out of Anthropic | `/logout` |
| `/mcp` | Manage MCP server connections — interactive, opens the Terminal tab | `/mcp` |
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
