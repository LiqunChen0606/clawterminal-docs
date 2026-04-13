# ClawTerminal — Guides & Tutorials

> **ClawTerminal** (also known as **CatClaw**) is an iOS SSH terminal + AI chatroom (Claude, Codex, Gemini, Aider) that connects your iPhone, iPad, or Apple Watch to your Mac and remote servers. Run AI agents, manage files, schedule jobs, and collaborate — all from your pocket.

[![Download on the App Store](https://img.shields.io/badge/Download-App%20Store-blue?logo=apple&logoColor=white)](https://apps.apple.com/us/app/clawterminal/id6759690902)
[![Platform](https://img.shields.io/badge/Platform-iOS%2017%2B%20%7C%20iPadOS%20%7C%20watchOS-blue)](https://apps.apple.com/us/app/clawterminal/id6759690902)
[![AI](https://img.shields.io/badge/AI-Claude%20%7C%20Codex%20%7C%20Gemini%20%7C%20Aider-purple)](https://apps.apple.com/us/app/clawterminal/id6759690902)
[![SSH](https://img.shields.io/badge/Protocol-SSH%2FSFTP%2FMosh-green)](https://apps.apple.com/us/app/clawterminal/id6759690902)

**Latest Version:** v1.5.0 (April 2026)

### What's New

| Feature | Description | Learn More |
|---------|-------------|------------|
| **Multi-Model Race** | Race 2-4 AI models on the same prompt. Compare responses side-by-side with AI-generated analysis. Same-model "thinking lenses" for diverse perspectives. | [Examples](examples/race.md) · [Tutorial](tutorials/race.md) |
| **GitHub PR Workflow** | Create PRs, run AI code reviews, post to GitHub — all from your phone. Review learning remembers what your team cares about. | [Examples](examples/pr-workflow.md) · [Tutorial](tutorials/pr-workflow.md) |
| **Session Handoff** | Start coding on your phone, continue on your Mac — or pick up a Mac session on your phone. Bidirectional, seamless. | [Examples](examples/handoff.md) · [Tutorial](tutorials/handoff.md) |
| **Live Web Preview** | Preview your web app via SSH tunnel. Auto-detects port, multi-port tabs, console log capture, responsive viewport modes, screenshot + annotate. | [Examples](examples/preview.md) · [Tutorial](tutorials/preview.md) |
| **Smart Model Routing** | Auto-assigns cost-appropriate models per agent role. Three presets: Quality, Balanced, Budget. Use `--routing balanced` with `/batch` or `/team`. | [Examples](examples/agent-advanced.md) |
| **Agent Reasoning** | See *why* your agents made decisions — extracted reasoning shown as a collapsible banner on job results. | [Examples](examples/agent-advanced.md) |
| **Git Worktree Mode** | `/batch --vcs` gives each agent a real git branch. Results auto-merge back with conflict reporting. | [Examples](examples/agent-advanced.md) |
| **AI Code Search** | `/search [query]` — semantic search across your codebase powered by AI. | [Examples](examples/git-health-monitor.md) |
| **Git Graph** | `/git` — visual branch/commit graph with timeline, branch tags, tap to checkout. | [Examples](examples/git-health-monitor.md) |
| **Codebase Health** | `/health` — one-tap dashboard: LOC, file count, TODOs, uncommitted changes, dependencies, largest file. | [Examples](examples/git-health-monitor.md) |
| **Server Monitor** | `/monitor` — live CPU, memory, disk, uptime, load average with sparkline charts. | [Examples](examples/git-health-monitor.md) |
| **Merge Conflict Resolver** | `/conflicts` finds all unmerged files, shows ours/theirs blocks, and generates AI-powered resolution suggestions with one-tap Apply. | [Examples](examples/workflow-automation.md) |
| **Smart Notifications** | `/notify when tests finish` — set natural language monitoring rules; CatClaw polls via SSH and sends a push notification when the condition is met. | [Examples](examples/workflow-automation.md) |
| **Plan Mode** | `/plan` — Claude generates a structured plan (files, changes, risks) before touching anything. Review then tap Execute to proceed. | [Examples](examples/workflow-automation.md) |
| **@web Context** | `@web https://url` fetches a URL via SSH, strips HTML, and injects up to 4000 chars into your message context. | [Examples](examples/workflow-automation.md) |
| **File Pinning** | `/pin filepath` keeps specific files always included in every message's context. `/unpin` to remove. | [Examples](examples/workflow-automation.md) |
| **Analytics Dashboard** | `/dashboard` shows aggregate stats across all chatrooms and jobs: success rates, token spend, daily activity, most-used commands — all local, no external service. | [Examples](examples/dashboard-workflows.md) |
| **Workflow Pipelines** | `/workflow [name]` runs named multi-step pipelines defined as JSON DAGs. Steps with no dependencies run in parallel; dependent steps wait. Visual DAG with live per-node status. | [Examples](examples/dashboard-workflows.md) |
| **Auto-Recovery for Failed Jobs** | When a background job fails, CatClaw classifies the error and auto-retries network/dependency failures. Other failures show a one-tap "Retry with AI fix?" banner. | [Examples](examples/dashboard-workflows.md) |
| **Auth Error Auto-Switch** | When Claude CLI reports an auth error, the app automatically switches to the Terminal tab and shows a recovery banner with `/login` instructions. No manual tab-switching needed. | [Examples](examples/dashboard-workflows.md) |

---

## Why ClawTerminal vs Claude Code Remote Control / Channels

Anthropic's Claude Code offers "Remote Control" (SSH tunnel to claude.ai) and "Channels" (Slack/Discord bridge). ClawTerminal takes a fundamentally different approach: a native iOS app with direct SSH and tmux-based session management. Here is how they compare:

| Feature | ClawTerminal | Claude Code Remote Control | Claude Code Channels |
|---------|-------------|---------------------------|---------------------|
| **Session persistence** | tmux survives Mac sleep, app backgrounding, SSH drops — pick up right where you left off | Dies when terminal closes | Dies when bridge process dies |
| **Mac sleep resilience** | SSH auto-reconnects, tmux session persists with full output | Process suspends, WebSocket drops | Bridge dies, must restart |
| **Real terminal** | Full PTY — vim, htop, tmux, interactive programs | No (command execution only) | No |
| **File browser** | SFTP with batch operations, upload/download, breadcrumb navigation | No | No |
| **Multi-tool AI** | Claude + Codex + Gemini + Aider in dedicated chatrooms | Claude only | Claude only |
| **Multi-agent orchestration** | `/batch --agents N` with Commander + Workers + Synthesizer, cross-tool assignment | Background agents (desktop only) | No |
| **Agent Teams** | `/team` — wave-based orchestration with visual command center, animated flow graph, discovery feed, per-agent status cards | Desktop-only CLI text output | No |
| **Multi-model comparison** | `/race` — run 2–4 AI models on the same prompt simultaneously, side-by-side results with AI summary | No | No |
| **GitHub PR workflow** | `/pr create`, `/pr review`, `/pr list`, `/pr checks` — full PR lifecycle with AI-powered code review | No (desktop only) | No |
| **Session handoff** | `/handoff` — start on phone, continue on Mac; or pick up an active Mac session on your phone | No | No |
| **Live web preview** | `/preview` — SSH port-forwarded browser preview of your dev server, auto-detects port from config files | No | No |
| **Smart model routing** | Auto-assigns cost-appropriate models per agent role (Quality/Balanced/Budget presets). Use `--routing balanced`. | No | No |
| **Git worktree isolation** | `--vcs` flag for agent branch isolation. Each agent works in its own branch; results auto-merge back with conflict reporting. | `/worktree` (desktop) | No |
| **Agent reasoning** | Extracted agent reasoning shown as a collapsible banner on job results | No | No |
| **Visual git graph** | `/git` — visual branch/commit timeline with branch tags; tap to checkout | No | No |
| **Codebase health dashboard** | `/health` — one-tap scan: LOC, file count, TODOs, uncommitted changes, dependencies, largest file | No | No |
| **Live server monitor** | `/monitor` — real-time CPU, memory, disk, uptime, load average with sparkline charts | No | No |
| **AI code search** | `/search [query]` — semantic AI-powered search across your codebase with ranked explanations | No | No |
| **Merge conflict resolver** | `/conflicts` — AI-powered conflict resolution with ours/theirs view and one-tap Apply | No | No |
| **Smart notifications** | `/notify when tests finish` — natural language SSH-polled monitoring rules with push notifications | No | No |
| **Plan mode** | `/plan` — structured dry-run plan (files, changes, risks) before any writes; tap Execute to proceed | No | No |
| **@web context** | `@web https://url` — fetch a URL via SSH and inject content into your message context | No | No |
| **File pinning** | `/pin filepath` — keep specific files always in context for every message in a chatroom | No | No |
| **Analytics dashboard** | `/dashboard` — aggregate stats: job success rates, token spend, daily activity sparkline, most-used commands, drill-down filtered views | No | No |
| **Workflow pipelines** | `/workflow [name]` — named multi-step pipelines as JSON DAGs, parallel steps, dependency ordering, live visual DAG with per-node status | No | No |
| **Auto-recovery for failed jobs** | Classifies job failures (network timeout, missing dependency, auth, etc.) and auto-retries or shows one-tap "Retry with AI fix?" banner | No | No |
| **Scheduled/recurring jobs** | Hourly, daily, weekly recurring with auto-submission | No | No |
| **Security model** | End-to-end SSH encryption, keys stored in iOS Keychain | Localhost only (requires SSH tunnel to reach claude.ai) | Code passes through third-party platforms (Slack, Discord) |
| **Setup** | Just SSH credentials (password or key) | CLI install + tunnel configuration | Bot token + bridge setup + runtime |
| **Native mobile app** | Full iOS/iPadOS/watchOS app, optimized for touch | Web browser on claude.ai | Messaging app (Slack/Discord) |
| **Offline / local network** | Works on local WiFi without internet (SSH only) | Requires active claude.ai connection | Requires platform API |
| **watchOS** | Voice dictation to submit jobs + monitor progress from wrist | No | No |
| **iPad** | Split view sidebar, Stage Manager multi-window, shared SSH sessions | No | No |
| **Code snippets** | Save, search, run from a persistent library | No | No |
| **AI autocomplete** | Ghost text suggestions in terminal as you type | No | No |
| **AI error diagnosis** | Floating "Ask Claude" pill auto-detects terminal errors | No | No |
| **Cross-session memory** | `/remember` facts persist across all sessions | No | No |
| **Shared chatroom** | Host/join via room codes, real-time guest viewing | No | No |
| **Conversation export** | PDF and Markdown export with share sheet | No | No |
| **Port forwarding** | Local, remote, and dynamic SOCKS5 tunnels | No (tunnel is one-way) | No |
| **Mosh transport** | UDP-based Mosh for high-latency connections | No | No |

**Bottom line:** ClawTerminal is a full mobile development environment. Claude Code Remote Control and Channels are remote interfaces to a single tool. If you want persistent, resilient, multi-tool AI access from your phone or tablet with real terminal capabilities, ClawTerminal is purpose-built for that.

---

## 📖 Table of Contents

- [Why ClawTerminal vs Claude Code Remote Control / Channels](#why-clawterminal-vs-claude-code-remote-control--channels)
- [Examples](#-examples) — copy-paste-ready examples for every feature

1. [Getting Started](#1-getting-started)
2. [SSH Setup — Connect to Your Mac (Local Network)](#2-ssh-setup--connect-to-your-mac-local-network)
3. [Tailscale — Remote Access Anywhere (Recommended for remote use)](#3-tailscale--remote-access-anywhere)
4. [SSH Keys](#4-ssh-keys)
5. [Claude AI Chatroom](#5-claude-ai-chatroom) (includes [Background Jobs](#background-jobs-submit), [Agent Orchestration](#agent-orchestration-orchestrate), [Batch Multi-Agent](#batch-multi-agent-orchestration-batch), [Agent Teams](#agent-teams-team), [Multi-Model Comparison](#multi-model-comparison-race), [GitHub PR Workflow](#github-pr-workflow-pr), [Session Handoff](#session-handoff-handoff), and [Live Web Preview](#live-web-preview-preview))
6. [Skills & Marketplace](#6-skills--marketplace)
7. [Smart Commands](#7-smart-commands)
8. [Slash Commands & @ References](#8-slash-commands---references)
9. [Terminal Shortcuts](#9-terminal-shortcuts)
10. [Port Forwarding & Tunnels](#10-port-forwarding--tunnels)
11. [SFTP File Browser](#11-sftp-file-browser)
12. [MCP Servers](#12-mcp-servers)
13. [Tips & Tricks](#13-tips--tricks)
14. [Troubleshooting](#14-troubleshooting)
15. [Feature Tutorials](#15-feature-tutorials)
- [Version History](#version-history)

---

## 💡 Examples

Practical, copy-paste-ready examples for every ClawTerminal feature. Each file covers a specific area with real-world scenarios you can try immediately.

| Example | What's inside |
|---------|---------------|
| [SSH Terminal](examples/ssh-terminal.md) | Connect via WiFi, Tailscale, Mosh; SSH config import; port forwarding; extended keyboard shortcuts |
| [AI Chatroom](examples/ai-chatroom.md) | CLI vs API mode, model switching, plan mode, compact, export |
| [Background Jobs](examples/background-jobs.md) | `/submit` one-shot jobs, `--ckpt` checkpoints, `--skills` injection, scheduled jobs |
| [Batch Agents](examples/batch-agents.md) | `/batch` with agent count, checkpoints, multi-tool, and skills flags |
| [Agent Teams](examples/agent-teams.md) | `/team` wave-based orchestration, `--multi` cross-tool, Visual Command Center |
| [Smart Commands](examples/smart-commands.md) | Custom slash commands with parameters, background auto-submit, tool overrides, skill injection |
| [Memory & Skills](examples/memory-skills.md) | `/remember`, `/forget`, `/memories`; enabling skills; per-message skills |
| [SFTP & Files](examples/sftp-files.md) | Browse, download, batch delete, Snap & Code workflow |
| [Collaboration](examples/collaboration.md) | Host/join shared rooms, relay server setup, guest watch view |
| [Multi-Model Comparison](examples/race.md) | `/race` examples — compare Claude, Codex, and Gemini side-by-side on the same prompt |
| [GitHub PR Workflow](examples/pr-workflow.md) | `/pr create`, `/pr review`, `/pr list`, `/pr checks` — full examples with review learning |
| [Session Handoff](examples/handoff.md) | `/handoff mac` and `/handoff` pickup — start on phone, continue on Mac |
| [Live Web Preview](examples/preview.md) | `/preview` auto-detect, port override, `--start`, and `stop` examples |
| [Slash Commands Reference](examples/slash-commands-reference.md) | Every slash command with syntax, flags, and examples — grouped by category |
| [Git, Health & Monitor](examples/git-health-monitor.md) | `/git` visual branch graph, `/health` codebase dashboard, `/monitor` live server stats, `/search` AI code search |
| [Advanced Agent Features](examples/agent-advanced.md) | Git worktree mode (`--vcs`), smart model routing (`--routing`), agent reasoning banner, combining all flags |
| [Workflow Automation](examples/workflow-automation.md) | `/conflicts` merge conflict resolver, `/notify` smart notifications, `/plan` enhanced dry-run, `@web` URL injection, `/pin` file pinning |

---

## 📚 Feature Tutorials

In-depth guides for individual ClawTerminal features. Each tutorial covers a single feature with step-by-step instructions.

### Multi-Model & AI Comparison

| Tutorial | Description |
|----------|-------------|
| [Multi-Model Comparison (`/race`)](tutorials/race.md) | Race 2–4 AI models on the same prompt simultaneously. Side-by-side results on iPad, swipeable cards on iPhone, with an AI-generated comparison summary. Includes same-model thinking lenses (`--copies`, `--lenses`): Adversarial, Pragmatic, Principled, User-First, Skeptic, Optimizer |
| [AI Code Review Learning](tutorials/pr-workflow.md#part-3-review-learning) | Thumbs-up/down feedback on individual review items teaches CatClaw your team's preferences. Set focus areas with `/pr focus security,tests,performance`. Builds up over 5–10 reviews |

### Developer Workflow

| Tutorial | Description |
|----------|-------------|
| [GitHub PR Workflow (`/pr`)](tutorials/pr-workflow.md) | Full GitHub PR lifecycle: auto-generate PR title and body, AI-powered code review with severity-colored items (red/yellow/blue/green), CI status checks, and per-chatroom review learning with thumbs-up/down ratings |
| [Session Handoff (`/handoff`)](tutorials/handoff.md) | Bidirectional handoff between phone and Mac: send your current session to a Mac tmux window, or discover and pick up active Mac sessions on your phone. Uses Claude's `--resume` flag for seamless context continuity |
| [Live Web Preview (`/preview`)](tutorials/preview.md) | SSH-tunneled live preview of your dev server in-app. Auto-detects port from package.json, .env, or vite.config. Multi-port tab switching, `--start` auto-launch, console log panel, screenshot+annotate, responsive viewport modes |

### Agent Orchestration

| Tutorial | Description |
|----------|-------------|
| [Agent Teams (`/team`)](tutorials/agent-teams.md) | Wave-based orchestration: Research → Implement → Review waves with parallel agents, discovery propagation between waves, visual command center with animated flow graph and live discovery feed |
| [Batch Multi-Agent Orchestration (`/batch`)](tutorials/batch-multi-agent.md) | Commander decomposes your goal, N parallel Workers execute it, Synthesizer merges results. Supports `--agents`, `--multi`, `--ckpt`, `--skills`, and `--vcs` for git worktree isolation |
| [Smart Commands](tutorials/smart-commands.md) | User-defined slash commands with named parameters, background auto-submit, tool overrides, skill injection, and auto-batch execution |

### Additional Features

| Tutorial | Description |
|----------|-------------|
| [Scheduling Recurring Background Jobs](tutorials/scheduled-jobs.md) | Set up hourly, daily, or weekly job schedules with automatic re-submission |
| [Understanding Agent Checkpoints](tutorials/agent-checkpoints.md) | Track progress of long-running jobs with manual `[CHECKPOINT]` markers or automatic `--ckpt` detection |
| [Using ClawTerminal on iPad](tutorials/ipad-multi-window.md) | NavigationSplitView sidebar, Stage Manager multi-window, shared SSH sessions |
| [Multi-CLI Tool Support](tutorials/multi-cli-tools.md) | Use Aider, Codex, or custom CLI tools alongside Claude in dedicated chatrooms |
| [Cross-Session AI Memory](tutorials/cross-session-memory.md) | Persistent `/remember` facts injected into every chatroom session automatically |
| [Shared Chatroom (Session Sharing)](tutorials/shared-chatroom.md) | Share your AI chatroom session via room codes — guests watch in real-time |

### Additional Feature Tutorials

| Tutorial | Description |
|----------|-------------|
| [AI Error Diagnosis](tutorials/ai-error-diagnosis.md) | Floating "Ask Claude" pill for instant terminal error diagnosis |
| [Snap & Code: Camera to Code](tutorials/snap-and-code.md) | Photograph a mockup or error, Claude generates and runs code |
| [Conversation Export (PDF/Markdown)](tutorials/conversation-export.md) | Save conversations as Markdown files or formatted PDFs |
| [SFTP Batch Operations](tutorials/sftp-batch-operations.md) | Multi-select files for bulk download and delete |
| [Custom Terminal Themes](tutorials/custom-terminal-themes.md) | Design custom color schemes with full 16-color ANSI palette editor |
| [Inline Code Execution](tutorials/inline-code-execution.md) | Run code blocks from Claude's responses with one tap |
| [Code Snippet Library](tutorials/code-snippets.md) | Save and reuse code blocks from conversations |
| [Smart Notifications](tutorials/smart-notifications.md) | Notifications for job completion, long responses, and to-do completion |

---

## 1. Getting Started

### Requirements

- iPhone or iPad running **iOS 17** or later
- A Mac (macOS 13 Ventura or later) **OR** any SSH-accessible Linux/BSD server
- **tmux** installed on your Mac/server (required for chatroom features)
- (Optional) [Tailscale](https://tailscale.com) for remote access outside your local network

#### Why tmux?

ClawTerminal runs Claude and other CLI tools inside **tmux sessions** on your Mac. This is what makes the chatroom resilient — if your phone sleeps, disconnects, or iOS backgrounds the app, the AI keeps running in tmux and ClawTerminal picks up where it left off when you reconnect. Background jobs (`/submit`), scheduled jobs, and agent orchestration all depend on tmux.

**Without tmux:** Chatroom messages fall back to direct SSH exec channels, which drop when the connection is interrupted — you may lose partial responses.

#### Install tmux

**macOS (Homebrew):**

```bash
brew install tmux
```

**Ubuntu / Debian:**

```bash
sudo apt install tmux
```

**Verify installation:**

```bash
tmux -V
# Expected output: tmux 3.x (any version 2.6+ works)
```

> **Note:** tmux is usually pre-installed on most Linux servers. On macOS, you need to install it via Homebrew.

### First Launch & Welcome Tour

When you open ClawTerminal for the first time you will see the **Welcome Tour** -- an 8-page interactive walkthrough covering all major features:

1. **Welcome to CatClaw** — Overview of the app and what it does
2. **Powerful SSH Terminal** — PTY, tabs, autocomplete, error diagnosis, Mosh, port forwarding
3. **Multi-Tool AI Chatroom** — Claude, Codex, Gemini, Aider with tool-call cards, diffs, thinking blocks, memory, skills, and commands
4. **Background Jobs & Agents** — `/submit`, `/batch` with N parallel agents, scheduled jobs, checkpoints
5. **SFTP File Browser** — Visual file management, batch operations, Snap & Code
6. **My Mac Workspace** — Auto-detect via Bonjour, auto-key setup, dual terminal + AI mode, Tailscale
7. **iPad, Watch & Beyond** — Split view, multi-window, watchOS dictation, shared chatrooms, relay server
8. **Ready to Go** — Links to Settings and a reminder that you can revisit the tour

Tap **Get Started** on the last page to begin, then follow the **Set Up My Mac** wizard.

> **Revisit the tour anytime:** Go to **Settings → About → Welcome Tour** to replay it.

---

## 2. SSH Setup — Connect to Your Mac (Local Network)

> For connecting while on the **same Wi-Fi network** as your Mac. If you're away from home, see §3 Tailscale instead.

### Enable Remote Login on your Mac

1. Open **System Settings → General → Sharing**
2. Turn on **Remote Login**
3. Note your Mac's local hostname (shown as `YourMac.local`) or IP address

### Add a Connection Profile

1. In ClawTerminal, tap the **Connections** tab (bottom bar)
2. Tap **+** (top right)
3. Fill in:
   - **Name**: e.g. "My MacBook"
   - **Hostname**: `YourMac.local` or IP address
   - **Port**: `22` (default)
   - **Username**: your macOS login name. Run `whoami` in Terminal on your Mac to confirm it. The field starts empty — ClawTerminal no longer pre-fills "mobile".
   - **Auth Method**: Password or SSH Key (see §4)
4. Tap **Save**, then tap the profile to connect

> **Tip:** Use the **My Mac wizard** (My Mac tab → Set Up My Mac) for a guided setup that handles key generation, authorization, and Tailscale automatically.

---

## 3. Tailscale — Remote Access Anywhere

[Tailscale](https://tailscale.com) creates a secure private network between your devices so you can SSH to your Mac from **anywhere** — coffee shop, hotel, cellular — without opening ports or configuring a VPN. It's the easiest way to connect ClawTerminal to your Mac remotely.

> **Best path for most users:** Tailscale + Password login. Zero terminal commands needed on your Mac.

---

### Step 1 — Install Tailscale on your Mac

1. Download **Tailscale** from the [Mac App Store](https://apps.apple.com/app/tailscale/id1475387142) or [tailscale.com/download](https://tailscale.com/download)
2. Open Tailscale → sign in with Google, GitHub, or Microsoft account (free)
3. Tailscale is now running. Click the menu bar icon to see your Mac's **Tailscale hostname** (looks like `your-mac.tail1234.ts.net`) — copy this

### Step 2 — Enable Remote Login on your Mac

1. Open **System Settings → General → Sharing**
2. Turn on **Remote Login**
3. Make sure your macOS user account is listed under "Allow access for"

> This is a one-time setup. You never need to open Terminal.

### Step 3 — Install Tailscale on your iPhone/iPad

1. Download **Tailscale** from the [iOS App Store](https://apps.apple.com/app/tailscale/id1470499037)
2. Sign in with the **same account** you used on your Mac
3. Your Mac will appear in the Tailscale device list — confirm it shows **Connected**

### Step 4 — Connect in ClawTerminal (Password method — recommended)

1. Open **ClawTerminal** → tap the **My Mac** tab → **Set Up My Mac**
2. On the "Find Your Mac" step, tap **Tailscale**
3. Enter:
   - **Hostname**: your Mac's Tailscale hostname (e.g. `your-mac.tail1234.ts.net`)
   - **Username**: your macOS login name (run `whoami` in Terminal on your Mac — the field starts empty; do not leave it blank)
4. Choose **Password** as the auth method
5. Enter your **macOS login password** on the next screen
6. Tap **Test Connection** — you should see a green checkmark
7. Tap **Finish**

That's it. ClawTerminal will remember this profile and reconnect automatically after sleep.

---

### Alternative: SSH Key Auth (more secure, no password to re-enter)

If you prefer passwordless login using an SSH key:

1. Follow Steps 1–3 above to get Tailscale running
2. In ClawTerminal → **Settings → SSH Keys** → **Generate New Key** → give it a name
3. Tap the key → **Copy Public Key**
4. On your Mac, open Terminal and run the following to authorize the key:

   ```bash
   mkdir -p ~/.ssh && pbpaste >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys && chmod 700 ~/.ssh
   ```

5. Back in ClawTerminal → Set Up My Mac → Tailscale → enter hostname + username → choose **SSH Key** → select the key you just created
6. Tap **Test Connection** → **Finish**

---

### Why Tailscale + Password beats the old way

| Method | Works outside home | No router config | No terminal setup | Encrypted | Works on cellular |
| --- | --- | --- | --- | --- | --- |
| Local Wi-Fi only | ❌ | ✅ | ✅ | ✅ | ❌ |
| Port forwarding | ⚠️ Risky | ❌ | ❌ | ❌ | ⚠️ |
| **Tailscale + Password** | ✅ | ✅ | ✅ | ✅ | ✅ |

Tailscale's free tier supports up to 3 devices — more than enough for iPhone + Mac.

---

### Troubleshooting Tailscale

#### "Connection timed out" with Tailscale hostname

- Open the Tailscale app on your iPhone and confirm your Mac shows **Connected** (not "Offline")
- If your Mac is asleep, wake it first — Tailscale can't connect to a sleeping machine
- Try using your Mac's Tailscale IP address instead of the hostname (visible in the Tailscale app)

#### Mac shows as offline in Tailscale

- On your Mac, click the Tailscale menu bar icon → **Connect**
- Make sure Tailscale is set to launch at login: Tailscale menu → Preferences → Launch at Login

#### Password authentication rejected

- Double-check your macOS login password (not your Apple ID password)
- Confirm **Remote Login** is still enabled in System Settings → Sharing

---

## 4. SSH Keys

SSH keys are more secure than passwords and allow passwordless login.

### Generate a Key on your iPhone/iPad

1. Go to **Settings → SSH Keys**
2. Tap **Generate New Key**
3. Give it a name (e.g. "My iPhone")
4. The key pair is generated and stored securely in the iOS **Keychain**

### Authorize the Key on your Mac

After generating, tap the key → **Copy Public Key**, then on your Mac run:

```bash
mkdir -p ~/.ssh && echo "PASTE_PUBLIC_KEY_HERE" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

Or use the **MyMac Setup Wizard** — it handles this automatically over SSH.

---

## 5. Claude AI Chatroom

ClawTerminal integrates Claude AI in two modes:

### Direct API Mode

- Uses the Anthropic API directly from your device
- Set your API key in **Settings → Claude API Key**
- Choose your model (Opus 4.6, Sonnet 4.6, Haiku 4.5)
- Supports streaming, thinking mode, file attachments

### CLI Mode (My Mac)

- Runs `claude` CLI over SSH via a persistent tmux session on your Mac
- Requires [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed on your Mac:

```bash
npm install -g @anthropic-ai/claude-code
```

- Supports multi-turn conversations, tool use, MCP servers, `--resume` continuity

### Multi-CLI Tool Support

ClawTerminal supports multiple CLI tools beyond Claude Code. Create dedicated chatrooms for different tools:

| Tool | Description |
|------|-------------|
| **Claude** (default) | Claude Code CLI — full tool use, MCP servers, `--resume` |
| **Aider** | AI pair programming with git integration |
| **Codex** | OpenAI Codex CLI |
| **Custom** | Any CLI tool — configure the binary path and invocation command |

To use a different tool: create a new chatroom → tap **Info (i)** → **CLI Tool** → select the tool. Each tool type has its own tmux session, binary path detection, and output parsing.

See the [Multi-CLI Tool Support tutorial](tutorials/multi-cli-tools.md) for setup details.

---

### Developer Setup (Recommended)

Two settings dramatically improve the chatroom experience for developers. Configure these before your first session.

#### Enable Tmux Chatroom Session

**Settings → Claude → Tmux Chatroom Session** → toggle **ON**.

Without tmux: each message runs `claude --resume` over a direct SSH exec channel. If your phone sleeps, the network blips, or iOS backgrounds the app, the channel drops and the response may be lost.

With tmux enabled:

- Each chatroom maps to a **named tmux session** on your Mac (visible as `claw_<room>` in `tmux ls`).
- Claude keeps running in that tmux session even when your phone disconnects — it finishes and writes output to a file.
- On reconnect, the app re-attaches and streams any output you missed.
- `/submit` background jobs run in their own tmux windows and are completely immune to phone sleep or disconnection.

#### Use CLI Mode (not API Mode)

The chatroom has two modes, shown in the **Info (ⓘ)** panel:

- **CLI mode** (recommended when connected) — runs `claude` on your Mac over SSH. Uses your Mac's Claude Code plan, the project's `CLAUDE.md`, local tools (`Bash`, `Read`, `Edit`, etc.), and full filesystem access.
- **API mode** — calls the Anthropic API directly from the iPhone. Useful without a Mac connection, but Claude has no access to your local tools or files.

CLI mode is the default when you are connected to your Mac. Keep it on.

---

### Room Memory (Context Document)

Each chatroom has a **Context Document** — a free-text field injected into every system prompt. Use it to describe your project, preferences, or standing instructions.

### Copying & Selecting Text from Responses

**Copy the whole response:** Long-press the **Claude avatar** (the claw icon beside any response) → **Copy All**.

**Select a portion of the text:** Long-press the Claude avatar → **Select Text…** — a full-screen sheet opens with the response text in a native iOS text view. Long-press any word to get iOS selection handles, drag to extend the selection, then tap **Copy** from the callout. Tap **Done** to close the sheet.

> **Why a separate sheet?** iOS scroll views intercept long-press gestures before text selection can activate, so in-place word selection is unreliable inside a scrollable chat list. The sheet sidesteps this entirely with a dedicated UITextView.

**Share the response:** Long-press the Claude avatar → **Share…** to send the full text to any app via the iOS share sheet.

---

### Background Jobs (`/submit`)

Submit long-running tasks to run in the background while you do something else — tasks can run for **up to 2 hours** on your Mac:

```text
/submit refactor the auth module to use async/await
```

Add `--ckpt` to enable **automatic checkpoint detection** — ClawTerminal scans the job's output for progress indicators (tool use, test results, file operations, step markers) and creates checkpoint timeline entries automatically:

```text
/submit --ckpt refactor the auth module to use async/await
```

You can also use **manual checkpoints** by asking Claude to output `[CHECKPOINT: label]` markers, or combine both approaches. See the [Agent Checkpoints tutorial](tutorials/agent-checkpoints.md) for details.

Progress updates appear in the **Jobs** panel. A notification fires when the task completes. The result is automatically injected into your next chatroom message so Claude has context without you having to paste anything.

---

### Agent Orchestration (`/orchestrate`)

For complex tasks that benefit from multiple perspectives, `/orchestrate` spawns **three parallel AI agents** that work simultaneously, then synthesizes their results into a single actionable summary.

```text
/orchestrate redesign the authentication system to use OAuth2
```

#### How it works

```text
┌─────────────────────────────────────────────────┐
│  /orchestrate redesign auth to use OAuth2       │
└──────────────────┬──────────────────────────────┘
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
  ┌──────────┐ ┌──────────┐ ┌──────────┐
  │Researcher│ │Implementer│ │ Reviewer │
  │  (blue)  │ │  (green)  │ │ (orange) │
  └─────┬────┘ └─────┬────┘ └─────┬────┘
        │             │            │
        │   All 3 run in parallel  │
        │   as background jobs     │
        └──────────┬──┬────────────┘
                   │  │
                   ▼  ▼
            ┌──────────────┐
            │ Synthesizer  │
            │  (purple)    │
            │              │
            │ Reads all 3  │
            │ results and  │
            │ posts summary│
            └──────────────┘
```

1. **Researcher** — Analyzes the codebase, reads relevant files, identifies patterns and constraints
2. **Implementer** — Writes the actual code changes, creates new files, modifies existing ones
3. **Reviewer** — Reviews the proposed changes for bugs, security issues, and best practices

After all three complete, a **Synthesizer** agent reads their combined output and posts a unified summary to your chatroom.

#### Viewing orchestration jobs

Open the **Jobs** panel to see orchestration groups. Jobs from the same `/orchestrate` command are grouped together under a collapsible header with:

- A purple **Orchestration** badge
- The original goal text
- Role chips color-coded by agent type:

| Role | Color | Purpose |
|------|-------|---------|
| Researcher | Blue | Codebase analysis and context gathering |
| Implementer | Green | Code writing and file modifications |
| Reviewer | Orange | Code review and quality checks |
| Synthesizer | Purple | Combines all results into actionable summary |

#### When to use `/orchestrate` vs `/submit`

| | `/submit` | `/orchestrate` |
|---|-----------|----------------|
| **Agents** | 1 | 3 + synthesizer |
| **Best for** | Single focused task | Complex multi-faceted work |
| **Speed** | Faster (one agent) | Slower (waits for all 3) |
| **Examples** | "fix the login bug" | "redesign the auth system" |
| | "add unit tests for UserService" | "audit security across the app" |
| | "update the README" | "plan and implement dark mode" |

> **Tip:** Both `/submit` and `/orchestrate` respect your current `/model` selection. If you've switched to Opus via `/model claude-opus-4-6`, all spawned agents use Opus.

---

### Batch Multi-Agent Orchestration (`/batch`)

`/batch` is the next evolution of agent orchestration. A **Commander** agent decomposes your goal into subtasks, **N parallel Workers** execute them simultaneously, and a **Synthesizer** merges their results into a single actionable summary.

```text
/batch --agents 4 implement the new payment flow
```

#### Flags

| Flag | Description | Default |
|------|-------------|---------|
| `--agents N` | Number of parallel worker agents (2–10) | `3` |
| `--multi` | Cross-tool assignment — Commander assigns Claude, Codex, Gemini, or Aider to each subtask based on its strengths | off |
| `--ckpt` | Enable automatic checkpoint detection on all worker jobs | off |
| `--skills alias1,...` | Attach named skills to all worker agents | none |

#### `--multi`: Cross-Tool Assignment

When `--multi` is set, ClawTerminal SSHs to your Mac and detects which CLI tools are installed. The Commander then assigns each subtask to the best-suited tool:

| Tool | Strengths |
|------|-----------|
| **Claude** | Code reasoning, architecture, documentation |
| **Codex** | Focused code generation and completions |
| **Gemini** | Long-context analysis, large file reads |
| **Aider** | Git-integrated refactoring, multi-file edits |

```text
/batch --agents 4 --multi migrate the database schema from Postgres to SQLite
```

#### Viewing batch jobs

Open the **Jobs** panel to see the batch group. The Commander, all Workers (color-coded blue), and the Synthesizer (purple) appear under a collapsible group header showing the original goal and overall status. Tap any job to view its assigned subtask, output, tool assignment, and checkpoint timeline.

#### When to use `/batch` vs `/orchestrate` vs `/submit`

| | `/submit` | `/orchestrate` | `/batch` |
|---|-----------|----------------|----------|
| **Agents** | 1 | 3 (fixed roles) | 2–10 (dynamic) |
| **Task decomposition** | You write the task | Fixed: Researcher / Implementer / Reviewer | Commander decomposes dynamically |
| **Multi-tool** | No | No | Yes (`--multi`) |
| **Checkpoints** | `--ckpt` | Not supported | `--ckpt` |
| **Best for** | Single focused task | Multi-perspective review | Complex goals needing flexible decomposition |

See the [Batch Multi-Agent Orchestration tutorial](tutorials/batch-multi-agent.md) for a full walkthrough.

---

### Agent Teams (`/team`)

`/team` is the most structured form of multi-agent work in ClawTerminal. A **Commander** agent decomposes your goal into a series of **waves** — sequential phases where multiple agents run in parallel. Discoveries from each wave are automatically extracted and injected into the next wave's prompts, so every stage builds on what was learned before.

```text
/team Write a Python calculator with add, subtract, multiply, divide, input validation, and a REPL loop
```

#### How waves work

```text
┌──────────────────────────────────────────────────┐
│  /team Write a Python calculator…                │
└────────────────────┬─────────────────────────────┘
                     │ Commander decomposes goal
                     │
          ┌──────────▼──────────┐
          │    Wave 1: Research  │  (parallel agents)
          │  Agent A  │ Agent B  │
          └──────────┬──────────┘
                     │ Discoveries extracted
                     ▼
          ┌──────────────────────┐
          │  Wave 2: Implement   │  (parallel agents)
          │  Agent C  │ Agent D  │  ← receives Wave 1 discoveries
          └──────────┬──────────┘
                     │ Discoveries extracted
                     ▼
          ┌──────────────────────┐
          │   Wave 3: Review     │  (parallel agents)
          │  Agent E  │ Agent F  │  ← receives Wave 1 + 2 discoveries
          └──────────┬──────────┘
                     │
                     ▼
               Final summary
               posted to chatroom
```

1. **Wave 1 — Research**: Agents explore the codebase, gather context, and identify constraints. Their discoveries (key findings, file paths, design decisions) are extracted automatically.
2. **Wave 2 — Implementation**: Agents receive the Research wave's discoveries and implement the solution. Their output (files created, patterns used, edge cases found) is extracted for the next wave.
3. **Wave 3 — Review**: Agents receive all prior discoveries and review the implementation for correctness, security, and quality.

#### Flags

| Flag | Description | Default |
|------|-------------|---------|
| `--multi` | Cross-tool assignment — Commander assigns Claude, Codex, Gemini, or Aider to agents based on their strengths within each wave | off |

#### Visual Command Center

While a team is running, the **Team** toolbar button opens the **Visual Command Center** — a full-screen view showing:

- **Animated flow graph** — pulsing nodes for each agent and wave, animated data-flow lines showing discoveries moving between waves
- **Live discovery feed** — real-time stream of discoveries extracted from completed agents
- **Per-agent status cards** — progress ring, current status (queued / running / completed / failed), assigned tool, and a preview of recent output

The Team toolbar button is always visible in the chatroom toolbar. When no team is running, tapping it shows an empty state with usage instructions.

#### When to use `/team` vs `/batch` vs `/orchestrate`

| | `/orchestrate` | `/batch` | `/team` |
|---|----------------|----------|---------|
| **Structure** | Fixed 3 roles | Dynamic decomposition | Wave-based sequential phases |
| **Parallelism** | All 3 at once | All workers at once | Parallel within each wave |
| **Knowledge sharing** | None | None | Discoveries flow between waves |
| **Multi-tool** | No | `--multi` | `--multi` |
| **Visual UI** | Jobs panel only | Jobs panel only | Animated command center |
| **Best for** | Quick multi-perspective review | Flexible parallel execution | Complex tasks where each phase informs the next |

See the [Agent Teams tutorial](tutorials/agent-teams.md) for a full walkthrough with examples.

---

### Multi-Model Comparison (`/race`)

`/race` runs the same prompt through 2–4 AI models **simultaneously** so you can compare their answers side-by-side. It is the fastest way to see how Claude, Codex, and Gemini each approach a problem before choosing which direction to take.

```text
/race Write a fizzbuzz function
/race --models claude,codex Explain the observer pattern
/race --models claude,codex,gemini What are the security implications of this JWT implementation?
```

#### How `/race` works

1. Your prompt is dispatched to each selected model as a background job.
2. Results stream back in parallel — you see responses as they arrive.
3. Once all models respond, an AI-generated **comparison summary** appears below, highlighting key differences in approach, correctness, and style.

#### `/race` layout by device

| Device | Layout |
|--------|--------|
| **iPad** | Side-by-side columns — all models visible at once |
| **iPhone** | Swipeable cards — swipe left/right to compare |

#### `/race` flags

| Flag | Description | Default |
|------|-------------|---------|
| `--models m1,m2,...` | Comma-separated list of models to race (2–4) | `claude,codex` |
| `--copies N` | Race the same model N times, each with a randomly selected thinking lens | — |
| `--lenses lens1,lens2,...` | Race the same model with specific thinking lenses (see table below) | — |

`--models` and `--copies`/`--lenses` are mutually exclusive — pick one approach per race.

#### Same-Model Thinking Lenses (`--copies` / `--lenses`)

Race the same model against itself with different system-level perspectives:

```text
/race --copies 3 How should we structure the error handling in this service?
/race --lenses adversarial,pragmatic,optimizer Design a rate limiting strategy
```

| Lens | Focus |
|------|-------|
| `adversarial` | What could break? Edge cases, attack vectors, failure modes |
| `pragmatic` | Simplest path to ship. Maintainability over perfection |
| `principled` | Best practices, design patterns, SOLID principles |
| `user-first` | End user experience, performance, accessibility |
| `skeptic` | Hidden assumptions, unstated requirements, scope creep |
| `optimizer` | Performance, memory, cost efficiency, redundancy |

Lenses are useful when you already know which model to use but want to stress-test an idea from multiple angles before committing.

#### When to use `/race`

- Evaluating a new algorithm or data structure approach
- Comparing documentation quality across models
- Getting a second opinion on a proposed solution before committing
- Stress-testing a design with adversarial or skeptic lenses before a PR
- Quick benchmarking of response style for a specific domain

#### `/race` requirements

- The models you select must be installed and authenticated on your Mac (e.g. `codex` requires the OpenAI Codex CLI; `gemini` requires the Gemini CLI)
- `--copies` / `--lenses` only require the currently active model — no additional installs needed
- See [Multi-CLI Tool Support](tutorials/multi-cli-tools.md) for tool setup

---

### GitHub PR Workflow (`/pr`)

`/pr` gives you a full GitHub pull request workflow from your phone. Create PRs, get AI-powered code reviews, check CI status — all over SSH without switching to a desktop browser.

**Requirement:** The `gh` CLI must be installed and authenticated on your Mac:

```bash
# Install
brew install gh

# Authenticate (one-time, on your Mac)
gh auth login
```

#### Create a PR

```text
/pr
/pr create
```

ClawTerminal reads `git diff` from your current project directory, sends it to Claude, and Claude generates a PR title and body following your project's conventions. The PR is then created via `gh pr create`. You'll see the PR URL in the chatroom when it's done.

#### AI Code Review

```text
/pr review 42
```

ClawTerminal fetches the PR diff via `gh` and sends it to Claude for review. Results appear as color-coded review cards:

| Color | Severity | Example |
|-------|----------|---------|
| Red | Bug | "This function doesn't handle the null case on line 47" |
| Yellow | Suggestion | "Consider extracting this logic into a helper function" |
| Blue | Question | "Why is this timeout set to 5000ms instead of the default?" |
| Green | Praise | "Nice use of async/await throughout this module" |

Each card has a thumbs-up / thumbs-down button. Your ratings train CatClaw's review preferences for this chatroom — see [AI Code Review Learning](#ai-code-review-learning) below.

#### List & Check CI

```text
/pr list              # List open PRs in the current repo
/pr checks 42         # Show CI status and check results for PR #42
```

#### Set Review Focus

```text
/pr focus security,tests,performance
```

Future reviews in this chatroom emphasize the listed areas. Run `/pr focus` with no arguments to reset to default (all areas equal weight).

---

### AI Code Review Learning

Every `/pr review` result has thumbs-up / thumbs-down buttons on each review item. Your ratings teach CatClaw what matters to your team:

- **Thumbs up** — reinforce this type of feedback; look for similar issues in future reviews
- **Thumbs down** — deprioritize this category; it's either too noisy or not relevant for your project

Preferences are stored **per-chatroom** — a backend services room and a frontend room can have different review styles. Switch focus areas with `/pr focus` to tune emphasis without clearing your learned ratings.

**Example workflow:**

```text
# First review — Claude looks at everything
/pr review 15

# Thumbs down on style nits, thumbs up on security and null-safety findings

# Second review on a later PR — Claude leads with security and null-safety,
# buries style suggestions at the bottom
/pr review 22
```

---

### Session Handoff (`/handoff`)

`/handoff` lets you seamlessly continue a coding session between your phone and your Mac — without losing context, output history, or the current directory.

#### Hand off from phone to Mac

```text
/handoff mac
```

ClawTerminal exports the current phone session's context (project directory, active Claude session ID, last N messages) and opens a new tmux window on your Mac pre-populated with that context. Sit down at your Mac, attach the tmux session, and keep going in a full terminal — the Claude session resumes with `--resume`, so conversation history is intact.

#### Pick up a Mac session on your phone

```text
/handoff
```

ClawTerminal SSHs to your Mac and lists all active tmux sessions running Claude, along with:
- Session name and project directory
- Time running
- Last line of output (preview)

Tap any session to attach it to the current chatroom. The phone session inherits the Claude session ID so conversation history continues seamlessly.

#### Use case: commute-to-desk handoff

```
Morning commute (phone):
  /submit Refactor the auth module — convert callbacks to async/await
  → (job runs in background while you commute)

Arrive at desk (Mac):
  /handoff mac
  → tmux window opens on your Mac, Claude session already attached
  → continue with full keyboard, multiple panes, editor open
```

---

### Live Web Preview (`/preview`)

`/preview` opens a live, in-app browser preview of the dev server running on your Mac — connected through an SSH port-forwarding tunnel. It is the first mobile tool that gives you a real live web preview while coding.

```text
/preview              # Auto-detect port and open preview
/preview --port 3000  # Specify port explicitly
/preview --start      # Auto-start dev server then open preview
/preview stop         # Close the SSH tunnel
```

#### Auto-detection

ClawTerminal scans the project directory for port configuration in this order:

1. `package.json` — `scripts.dev`, `scripts.start` (extracts port from `--port` flag or `PORT=` prefix)
2. `.env` / `.env.local` — `PORT=` or `VITE_PORT=` variables
3. `vite.config.ts` / `vite.config.js` — `server.port` value
4. Falls back to `3000` if none found

#### `--start` flag

When `--start` is used, ClawTerminal runs your dev server start command (detected from `package.json scripts.dev`) in a background tmux window before opening the tunnel. The preview waits for the server to be ready (HTTP 200) before opening.

```text
/preview --start      # Equivalent to: npm run dev (in tmux) + /preview
```

#### Under the hood

`/preview` creates a **local port-forwarding tunnel** from your iPhone to the Mac:

```
iPhone browser → SSH tunnel → Mac localhost:PORT → dev server
```

The same tunnel infrastructure used by the Port Forwarding tab (§10), but wired automatically from a single slash command.

#### Close the preview

```text
/preview stop
```

Closes the SSH tunnel. The dev server continues running on your Mac unless you stop it separately.

---

## 6. Skills & Marketplace

**Skills** are Markdown snippets injected into Claude's system prompt to give it specialized knowledge, project context, or standing instructions. ClawTerminal ships with a built-in marketplace of 30 curated packages.

### Browse & Install from the Marketplace

1. In a chatroom, tap the **Skills** button in the toolbar → **Marketplace** tab
2. Browse by category:

   | Category | What's inside |
   |----------|--------------|
   | **Community** | Docker, Git, Python, Node.js, Homebrew, jq, curl, SSH Power User, and more |
   | **Anthropic Workflows** | 14 Superpowers skills (brainstorming, TDD, systematic debugging, plan execution, parallel agents, code review, git worktrees…) — auto-enabled for new chatrooms |
   | **LSP** | TypeScript, Python, and other language-server integrations |

3. Tap **Install** on any package — it appears in the **My Skills** tab
4. Toggle skills on or off per chatroom from **My Skills**

### Write a Custom Skill

1. Skills button → **My Skills** → tap **+**
2. Write Markdown — Claude reads this as system prompt context. Example:

   ```markdown
   ## My Project
   You are working on a FastAPI backend in ~/Projects/myapp.
   Always use async/await. Tests live in tests/. Use pytest.
   Run `make dev` to start the dev server.
   ```

3. Tap **Save** — the skill is now available across all your chatrooms

### Import a Skill

- **From Files app**: Skills → My Skills → **Import** → pick any `.md` file
- **From GitHub URL**: Skills → My Skills → **Import from URL** → paste a raw GitHub URL to a `.md` file

  ```text
  https://raw.githubusercontent.com/example/skills/main/docker.md
  ```

### Save Tokens with `--skills` (Per-Message Injection)

Globally enabled skills are injected into **every** message, which consumes tokens even when irrelevant. For skills you only need occasionally, use **per-message injection** instead:

1. **Disable** the skill globally (toggle off in My Skills)
2. **Set an alias** — tap the skill → give it a short alias like `tdd`, `debug`, or `docker`
3. **Inject on demand** — append `--skills alias` to any message:

   ```
   Fix the auth bug --skills tdd,debug
   ```

   Or with `/submit` and `/batch`:

   ```
   /submit Refactor the API layer --skills tdd
   /batch --agents 3 --skills security Build payment flow
   ```

This way the skill content is only sent when you need it — saving tokens on every other message. This is especially important for large skills (like the Anthropic Superpowers pack) that can add thousands of tokens per turn.

**Tip:** Keep small, always-relevant skills enabled globally (e.g., project context). Keep large, task-specific skills as aliases for on-demand use.

### Export & Share Skills

Tap any skill → **Export** to save as JSON. Share with teammates and re-import with **Import**.

---

## 7. Smart Commands

ClawTerminal lets you define your own `/command` shortcuts that go beyond simple text templates. **Smart Commands** support:

- **Named parameters** with optional defaults — e.g. `/deploy production feature-x`
- **Run in Background** — auto-submit as a `/submit` background job
- **Tool Override** — force a specific CLI tool (Claude/Codex/Gemini/Aider) regardless of chatroom default
- **Skill Aliases** — auto-attach skills to the command's system prompt
- **Agent Count** — auto-run as `/batch --agents N` with Commander + Workers + Synthesizer

### Creating a Smart Command

1. Type `/` in any chatroom to open the slash command palette
2. Tap **Manage Commands** (or go to the Commands section in the toolbar)
3. Tap **+** to create a new command
4. Fill in the name, template with `{paramName}` placeholders, parameters, and optional smart fields
5. Tap **Save**

### Example: One-Tap Full Audit

| Field | Value |
|-------|-------|
| Name | `fullaudit` |
| Template | `Perform a comprehensive audit of {scope}: security, performance, code quality, and test coverage.` |
| Parameters | `scope` (default: `the entire codebase`) |
| Agent Count | 4 |
| Run in Background | Yes |

Now `/fullaudit` spawns 4 parallel agents that audit your codebase, and `/fullaudit src/payments/` focuses on a subdirectory.

See the [Smart Commands tutorial](tutorials/smart-commands.md) for a full reference and more examples.

---

## 8. Slash Commands & @ References

Type `/` at the start of any message in a chatroom to see available commands. These work in both API mode and CLI mode unless noted.

### Slash Commands

| Command | Description |
|---------|-------------|
| `/submit <task> [--ckpt] [--skills alias1,...]` | Send a long-running task to the **background job queue**. Add `--ckpt` for automatic checkpoint detection. A notification fires when done. |
| `/resume` | Resume the most recent Claude Code CLI session for this chatroom (CLI mode only) |
| `/plan <task>` | Switch to **plan mode** — Claude reads files and proposes a plan but cannot write or execute (CLI mode only) |
| `/compact` | Ask Claude to summarise the conversation so far and compact the context window |
| `/init` | Ask Claude to create a `CLAUDE.md` file in the current project directory with project-specific instructions |
| `/model <name>` | Switch the active model mid-conversation, e.g. `/model claude-opus-4-6` |
| `/clear` | Clear the current conversation and start fresh |
| `/orchestrate <goal>` | Spawn **3 parallel AI agents** (Researcher, Implementer, Reviewer) plus a Synthesis agent that combines their results. See [Agent Orchestration](#agent-orchestration-orchestrate) below. |
| `/batch <goal> [--agents N] [--multi] [--ckpt] [--skills alias,...] [--vcs] [--routing preset]` | Spawn a **Commander + N Worker agents + Synthesizer**. Commander decomposes the goal dynamically. `--multi` assigns different tools (Claude/Codex/Gemini/Aider) to different subtasks. `--vcs` gives each agent its own git branch (worktree mode). `--routing quality/balanced/budget` auto-assigns cost-appropriate models per role. See [Batch Multi-Agent Orchestration](#batch-multi-agent-orchestration-batch). |
| `/team <goal> [--multi] [--routing preset]` | Wave-based orchestration — Commander decomposes into sequential waves (Research → Implement → Review), agents run in parallel within each wave, discoveries propagate between waves. `--routing quality/balanced/budget` auto-assigns models per wave role. Visual command center with animated flow graph and live discovery feed. See [Agent Teams](#agent-teams-team). |
| `/cost` | Show estimated token usage and cost for the current session |
| `/diff` | Show the last code changes Claude made |
| `/context` | Display current session context (project dir, session ID, model) |
| `/status` | Show connection state and chatroom info |
| `/doctor` | Run diagnostics on your current setup |
| `/export` | Export the current conversation |
| `/copy` | Copy Claude's last response to the clipboard |
| `/rename <name>` | Rename the current chatroom |
| `/tasks` | List all background jobs and their status |
| `/config` | Display current settings |
| `/race <prompt> [--models m1,m2,...]` | Race 2–4 AI models on the same prompt simultaneously. Results appear side-by-side (iPad) or as swipeable cards (iPhone), with an AI summary comparing strengths and trade-offs. Default models: claude + codex. Specify others with `--models claude,codex,gemini`. |
| `/pr [create]` | Auto-generate a GitHub PR title and body from `git diff`, then create the PR via the `gh` CLI installed on your Mac. Requires `gh` authenticated on your Mac. |
| `/pr review <number>` | AI-powered code review for the given PR number. Review items are color-coded by severity (red bugs, yellow suggestions, blue questions, green praise). Thumbs-up/down on each item teaches CatClaw your team's preferences. |
| `/pr list` | List open pull requests in the current repo. |
| `/pr checks <number>` | Show CI status and check details for the given PR number. |
| `/pr focus <areas>` | Set review focus areas for this chatroom, e.g. `/pr focus security,tests,performance`. Future reviews emphasize the selected areas. |
| `/handoff mac` | Hand your current phone chatroom session to a Mac tmux window. The terminal session is recreated on the Mac so you can continue in a full desktop terminal. |
| `/handoff` | Discover active Claude sessions on your Mac and pick one up on your phone. Shows a list of running tmux sessions with their last output preview — tap one to attach. |
| `/preview [--port N] [--start] [stop]` | Open a live web preview of your dev server via SSH port forwarding. Auto-detects the port from `package.json`, `.env`, or `vite.config`. Add `--port 3000` for a specific port. Add `--start` to auto-launch the dev server before opening the preview. Run `/preview stop` to close the tunnel. |
| `/git` | Open a **visual git branch graph** — commit timeline with branch tags. Tap a branch name to checkout. |
| `/health` | Run a **codebase health scan** — lines of code, file count, TODO/FIXME count, uncommitted changes, dependencies, last commit age, branch count, and largest file. Results shown in a one-tap dashboard. |
| `/monitor` | Open a **live server monitor** — real-time CPU usage, memory, disk space, uptime, and load average. Sparkline charts show trends. Tap refresh for latest readings. |
| `/search <query>` | **AI-powered semantic search** across your codebase. Greps for keywords, then Claude ranks and explains the top 5 most relevant matches. |
| `/voice` | Start **voice input** — dictate your message and tap send when done. Works in any chatroom or background job. |
| `/remember <fact>` | Save a fact to **cross-session memory** — injected into every future chatroom session |
| `/forget <keyword>` | Remove memories matching the keyword |
| `/memories` | Open the **Memory Library** to browse, search, and manage saved memories |
| `/conflicts` | Scan for git merge conflicts, show AI resolution suggestions per file, one-tap Apply to write the resolved content. |
| `/notify <condition>` | Set a natural language monitoring rule (e.g. `/notify when tests finish`). CatClaw polls via SSH every 30s and sends a push notification when the condition is met. |
| `/notify list` | Show all active notification rules. |
| `/notify cancel` | Remove all active notification rules. |
| `/plan` | Toggle **plan mode** — your next message generates a structured plan (files, changes, risks, complexity) instead of making changes. Tap **Execute** in the plan card to proceed. |
| `/pin <filepath>` | Keep a file always included in context for every message in this chatroom. |
| `/pin` | List currently pinned files. |
| `/unpin <filepath>` | Remove a file from the always-include pin list. |
| `/dashboard` | Open the **analytics dashboard** — aggregate stats across all chatrooms and jobs: success rates, token spend, daily activity sparkline (last 14 days), most-used commands, most-active chatrooms. Tap any card to drill into a filtered job list. |
| `/workflow [name]` | Run a named multi-step **pipeline** defined in `workflows.json`. Steps with no dependencies run in parallel; dependent steps wait. Rendered as a live DAG. |
| `/workflow list` | List all saved workflow pipelines. |
| `/workflow save [name]` | Save the last N slash commands as a new pipeline. |
| `/help` | List all available slash commands |

### @ References

Type `@web https://url` anywhere in a message to fetch a URL and inject its content:

1. CatClaw fetches the URL via SSH (`curl`) — no device traffic, uses your Mac's network
2. HTML is stripped and the content is truncated to 4000 chars
3. The result is injected as a `<web_content url="...">` block before your message

Great for documentation pages, API references, GitHub raw files, or any plain-text URL. Combine with file references: `Implement this spec: @web https://api.example.com/docs and also check @src/client.ts`.

### @ File References

Type `@` anywhere in your message to open the **remote file picker** — a live SSH directory browser on your Mac:

1. Type `@` → a picker appears showing your Mac's file tree
2. Navigate to the file you want to attach
3. Tap it — the file's contents are injected as a `<file_attachment>` block in your message

Claude can then read, discuss, or modify that file. Works great for attaching config files, source files, or logs without copy-pasting.

---

## 9. Terminal Shortcuts

The shortcut bar below the terminal has four categories. Tap the pill labels to switch.

> **Dismiss keyboard:** Tap the **chevron-down button** at the right edge of the extended keyboard bar to dismiss the keyboard without losing focus. This is available in both the terminal and the chatroom input.

### Shell

| Button | Sends | Effect |
|--------|-------|--------|
| `^C` | `0x03` | SIGINT — interrupt running process |
| `^D` | `0x04` | EOF — logout / end stdin |
| `^L` | `0x0C` | Clear screen |
| `^Z` | `0x1A` | Suspend process to background |
| `^R` | `0x12` | Reverse history search |
| `^A` | `0x01` | Jump to line start |
| `^E` | `0x05` | Jump to line end |
| `^W` | `0x17` | Delete word backwards |
| `!!` | `!!\n` | Repeat last command |
| `sudo!!` | `sudo !!\n` | Run last command as root |
| `exit` | `exit\n` | Close shell |

### Tmux

All tmux shortcuts send `Ctrl-B` prefix followed by the key:

| Button | Key | Action |
|--------|-----|--------|
| `new` | `c` | New window |
| `split \|` | `%` | Vertical split |
| `split —` | `"` | Horizontal split |
| `detach` | `d` | Detach session |
| `prev` | `p` | Previous window |
| `next` | `n` | Next window |
| `zoom` | `z` | Toggle pane zoom |
| `kill` | `x` | Kill pane |

### Git

| Button | Sends |
|--------|-------|
| `status` | `git status\n` |
| `diff` | `git diff\n` |
| `log` | `git log --oneline -10\n` |
| `push` | `git push\n` |
| `pull` | `git pull\n` |
| `add .` | `git add .\n` |
| `commit` | `git commit -m ""` (cursor inside quotes) |
| `stash` | `git stash\n` |
| `stash pop` | `git stash pop\n` |

### Vim

| Button | Sends | Action |
|--------|-------|--------|
| `:wq` | `:wq\n` | Save and quit |
| `:q!` | `:q!\n` | Quit without saving |
| `:w` | `:w\n` | Save |
| `insert` | `i` | Enter insert mode |
| `normal` | ESC | Return to normal mode |
| `undo` | `u` | Undo |
| `/search` | `/` | Start search |
| `dd` | `dd` | Delete line |
| `yy` | `yy` | Yank (copy) line |
| `ZZ` | `ZZ` | Save and quit |

---

## 10. Port Forwarding & Tunnels

Port forwarding lets you securely access services on your Mac or remote network through the SSH tunnel.

### Add a Tunnel

1. Connect to an SSH profile
2. In **My Mac** tab → **Tunnels** sub-tab
3. Tap **+** → choose tunnel type:

| Type | Description |
|------|-------------|
| **Local** | Forward `localhost:<localPort>` on your device to `host:port` on the server |
| **Remote** | Forward a port on the remote server back to your device |
| **Dynamic** | SOCKS5 proxy on your device routing all traffic through the SSH server |

### Example: Access a local dev server

- Type: **Local**
- Local Port: `3000`
- Remote Host: `localhost`
- Remote Port: `3000`

Then open `http://localhost:3000` in Safari on your iPhone to hit the dev server running on your Mac.

---

## 11. SFTP File Browser

Browse, upload, and download files over SFTP without leaving the app:

1. Connect to an SSH profile
2. In **My Mac** tab → **Files** sub-tab
3. Navigate directories, tap files to preview or download
4. Swipe left on a file to rename or delete

### Long-Press Context Menu

Long-press any file or folder in the browser to get quick actions:

| Action | Available on | What it does |
|--------|-------------|--------------|
| **Copy Path** | Files & Folders | Copies the full absolute path (e.g. `/Users/you/Projects/myapp`) to the clipboard |
| **Copy Name** | Files & Folders | Copies just the file or folder name |
| **Open Folder** | Folders only | Navigates into the folder |

Long-press any **breadcrumb chip** in the path bar to copy the current directory path.

> **Tip:** Use **Copy Path** on a project folder, then paste it into a chatroom's **Project Directory** field (Info tab → Project directory) to point Claude at the right working directory. The project path is remembered for the entire conversation session — even after reconnects.

### Create a Folder or File

Tap the green **+** button in the breadcrumb bar to create new items in the current directory:

| Option | What happens |
|--------|-------------|
| **New Folder** | Prompts for a folder name, creates it, and navigates into it |
| **New File** | Prompts for a file name, creates an empty file, and auto-opens the inline text editor |

This lets you scaffold a new project structure or create config files entirely from your iPhone without opening a terminal session.

### Upload from Files App

Tap the **Upload** button (top right) to pick files from the iOS Files app and transfer them to the remote server. Supported types include Markdown, source code, PDFs, images, and any file type the iOS Files app can browse.

---

## 12. MCP Servers

ClawTerminal supports [Model Context Protocol (MCP)](https://modelcontextprotocol.io) servers, giving Claude access to external tools like file systems, databases, and APIs.

### Add an MCP Server

1. Go to **Settings → MCP Servers**
2. Tap **+**
3. Configure:
   - **Name**: display name
   - **Transport**: SSH (runs server on your Mac) or HTTP/SSE
   - **Command**: command to start the server on your Mac

```bash
# Example: filesystem server
npx -y @modelcontextprotocol/server-filesystem /Users/you/Projects
```

Enabled MCP servers are listed as available tools in every Claude chatroom.

---

## 13. Tips & Tricks

- **Markdown in the input field**: Type markdown directly — `**bold**`, `*italic*`, `` `inline code` ``, triple backticks for code blocks, `- bullet`, `1. numbered`, `> quote`. Claude's responses render all of these natively.
- **Multi-tab terminal**: Tap **+** in the Terminal tab to open multiple SSH sessions simultaneously — great for running a server in one tab and editing code in another
- **Theme picker**: Go to **Settings → Terminal Theme** to choose from Catppuccin, Solarized Dark, One Dark, Monokai, Gruvbox Dark
- **Font size**: Pinch-to-zoom in the terminal adjusts font size on the fly
- **Quick reconnect**: Recently used profiles appear on the My Mac welcome screen for one-tap reconnect
- **Auto-reconnect after sleep**: If your phone goes to sleep and drops the SSH connection, ClawTerminal silently restores it the next time you open the app — no need to manually disconnect and reconnect
- **Copy SFTP path → chatroom**: In the Files tab, long-press any folder → **Copy Path**, then paste it into a chatroom's **Info → Project Directory** field. Claude will use that directory for the whole conversation, even across reconnects
- **Session continuity**: Once Claude runs its first command in a chatroom, the project directory is locked for that session. This ensures `--resume` always finds the right conversation history even after your phone sleeps
- **Context Document**: Write project-specific instructions in the chatroom's Context Document so Claude always has the right background
- **Auto Memory**: Enable Auto Memory in a chatroom — Claude will write a persistent memory file on your Mac and read it at the start of each session
- **iCloud Sync**: Enable iCloud sync in Settings to sync your connection profiles to all your Apple devices
- **Favorites**: Star a connection profile to pin it to the top of the Connections list
- **Color Tags**: Assign color tags to connection profiles for quick visual identification
- **Home Screen Widget**: Add the **Quick Connect** widget (small or medium) to your home screen for one-tap SSH connection to your most-used profiles
- **Mosh transport**: For high-latency or unreliable connections, switch a profile's transport to **Mosh** (connection form → Transport). Mosh uses UDP for instant local echo and survives IP changes
- **watchOS companion**: Open the ClawTerminal Watch app to dictate jobs via Siri, monitor running jobs, and see completion status — all from your wrist
- **Message pinning**: Long-press any message → **Pin** to keep important messages accessible. Use the pin filter in the banner to show only pinned messages
- **Welcome Tour**: Revisit the 8-page onboarding walkthrough anytime from **Settings → About → Welcome Tour**
- **SSH config import**: Import connection profiles from your Mac's `~/.ssh/config` file via the ellipsis menu in the My Mac header — no manual re-entry needed
- **Relay server**: Enable the relay server in Settings to collaborate with teammates via shared chatroom room codes
- **Agent Teams toolbar button**: The **Team** button is always visible in the chatroom toolbar. Tap it during a `/team` run to open the Visual Command Center (animated flow graph, discovery feed, per-agent status). When no team is running, it shows an empty state with usage instructions — you do not need a team running to find the button.
- **Memories toolbar button**: The chatroom toolbar has a single **Memories** button (brain icon) that opens the Memory Library. Earlier versions had two separate memory tabs — these are now merged. Use `/remember`, `/forget`, and `/memories` commands or tap the brain icon directly.
- **SFTP create folder/file**: Tap the green **+** button in the SFTP breadcrumb bar to create a new folder or file directly from your iPhone without opening a terminal session.
- **Background job context**: Completed job results are automatically available to subsequent jobs. Use `/submit "build on the refactoring from the previous job"` and the last 3 completed results are injected into the new job's context automatically.
- **Slide-out drawer (iPhone)**: On iPhone, tap the hamburger menu button (top-left) to open the navigation drawer. All tabs — My Mac, Terminal, Connections, Settings — are accessible from here. Tap the dimmed overlay or swipe to dismiss.
- **Multi-model comparison**: Use `/race` when you are unsure which approach is best — race Claude against Codex or Gemini and let the AI summary tell you the trade-offs. Great for algorithm selection, documentation wording, and architectural decisions.
- **Same-model lenses for stress-testing**: Use `/race --lenses adversarial,skeptic` before proposing a design to your team — the Adversarial lens will find failure modes you hadn't considered, and Skeptic will challenge hidden assumptions.
- **PR review on your commute**: Fetch a colleague's PR on your phone with `/pr review 42` and leave thumbs-up/down feedback while commuting. Your ratings are remembered and improve future reviews for that chatroom.
- **Commute-to-desk handoff**: Use `/submit` to kick off a long refactor before you leave your desk. On your commute, monitor it in the Jobs tab. When you arrive, run `/handoff mac` to continue the conversation in a full terminal with full context intact.
- **Preview before push**: Run `/preview --start` to auto-launch your dev server and open a live preview on your phone — useful for a final visual check before running `/pr create`.
- **Console logs for debugging**: In the preview sheet, tap the terminal icon to see JavaScript console output. Tap "Send errors to Claude" to get a diagnosis without leaving the app.
- **Annotate UI bugs**: In the preview sheet, tap the camera icon, draw on the screenshot, and send it to Claude with a description. Claude reads the annotation and proposes a specific CSS fix.
- **Job tab filter**: Use the color-coded pill tabs in the Jobs panel (All / Jobs / Race / Agents / Scheduled) to quickly find what you're looking for when many jobs are in flight.
- **Orange flag hints**: As you type flags like `--agents 4` or `--models claude,codex`, the flag names turn orange in the input bar — a quick visual confirmation that the app recognizes the flag before you send.
- **Git graph for branch navigation**: Run `/git` to see a visual timeline of commits and branches. Tap any branch name to check it out — no need to remember branch names or type git commands manually.
- **Codebase health before a refactor**: Run `/health` before starting a large refactor to get a quick snapshot of LOC, TODO count, and uncommitted changes. Run it again after to compare.
- **Server monitor during deployments**: Keep `/monitor` open during a deploy to watch CPU and memory react in real time. The sparkline chart shows whether load normalizes after a restart.
- **Semantic code search**: Use `/search` with plain English questions instead of regex — e.g. `/search where is the payment flow handled` or `/search find all API rate limit logic`. Claude ranks and explains the top matches.
- **Worktree mode for safe experiments**: Add `--vcs` to any `/batch` run to give each agent its own git branch. If the results are bad, delete the branches with no impact on your working tree. If they are good, the merge happens automatically.
- **Routing presets for cost control**: Use `--routing budget` for exploratory research tasks where speed and cost matter more than quality, and `--routing quality` for production code generation where accuracy is critical.
- **Agent reasoning as a sanity check**: After any background job completes, open the job detail and read the Agent Reasoning card. If the agent misunderstood the task, the reasoning card usually shows why — which helps you write a clearer prompt next time.
- **Resolve merge conflicts after worktree batch runs**: After `/batch --vcs`, if the auto-merge produces conflicts, run `/conflicts` to get per-file AI resolution suggestions. Tap Apply on each resolved file — no need to open a terminal.
- **Monitor long test suites without polling**: Run `/notify when tests finish` before starting a long test run, then put your phone down. CatClaw sends a push notification the moment the test process exits — no need to check the Jobs tab manually.
- **Plan mode before risky refactors**: Toggle `/plan`, then describe the refactor. Review the file list and risk notes before anything is written. If the scope looks wrong, adjust your prompt and plan again — costs nothing until you tap Execute.
- **Inject API docs without copy-paste**: Use `@web https://docs.example.com/api` to pull a reference page directly into your message. Pair it with a code question and Claude sees the spec alongside your source file.
- **Pin your config file for every message**: Run `/pin src/config.ts` in a chatroom that touches configuration constantly. The file content is always in context — no need to attach it manually each time.
- **Weekly analytics review**: Run `/dashboard` at the start of each week to see which commands you use most, how much you've spent on tokens, and whether your job success rate is trending up or down. Drill into "failed" to see which job types need better prompts.
- **Build a deployment pipeline**: Create a `deploy.json` workflow with steps for linting, testing, building, and deploying. Steps that can run in parallel (lint + type-check) share a wave; the deploy step depends on build. Run `/workflow deploy` to execute the full pipeline with a live DAG view.
- **Auto-recovery saves retries**: When a job fails due to a network blip or missing npm package, CatClaw retries automatically. Open the job detail to see the retry lineage card — it shows the original failure reason and what the recovery prompt changed. This is especially useful for long `/batch` runs where individual workers may hit transient errors.
- **Auth recovery is one tap**: If the Claude chatroom shows an auth banner, it will automatically switch to the Terminal tab. Just type `claude`, then `/login`, and follow the OAuth flow — no need to remember which tab to go to or what command to run.

### Keep Your Mac Awake for SSH

Your Mac must stay awake for SSH connections to work. In **Mac System Settings**, search for **"Prevent automatic sleeping when the display is off"** and toggle it **ON**. This keeps your Mac accessible even with the lid closed, as long as it's plugged into power.

---

## 14. Troubleshooting

### "Connection timed out" or connection keeps spinning

- Confirm **Remote Login** is enabled on your Mac (**System Settings → General → Sharing**)
- Run `ping YourMac.local` on another device to confirm the hostname resolves
- Make sure your iPhone and Mac are on the **same Wi-Fi network**
- If outside your home network, set up Tailscale (§3)
- Check your Mac's firewall isn't blocking port 22

### "Connection failed" during My Mac setup wizard

This can happen if your Mac is only reachable via an IPv6 link-local address on your current network. ClawTerminal automatically falls back to the `.local` mDNS hostname — if you see this error, tap **Try Again**. If it persists, try entering your Mac's hostname manually (e.g. `Liquns-MacBook-Pro.local`) or use its IPv4 address from **System Settings → Wi-Fi → Details**.

### SSH key authentication fails

- Confirm the public key was appended to `~/.ssh/authorized_keys` on the Mac
- Check permissions:

  ```bash
  chmod 600 ~/.ssh/authorized_keys
  chmod 700 ~/.ssh
  ```

- Restart sshd on your Mac:

  ```bash
  sudo launchctl unload -w /System/Library/LaunchDaemons/ssh.plist
  sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist
  ```

### Claude chatroom shows no response (CLI mode)

- Ensure **tmux** is installed on your Mac: `which tmux` (if missing, install with `brew install tmux`)
- Ensure `claude` CLI is installed on your Mac: `which claude`
- The CLI chatroom requires an active SSH connection to your Mac
- Check running sessions: `tmux ls` on your Mac
- If you see **"⚠️ Rate limit reached"** — Claude's API has temporarily throttled your account. Wait 30–60 seconds and send again. ClawTerminal detects the timeout automatically and stops waiting instead of hanging
- If the chatroom was open while your phone slept, the connection is restored automatically on wake — you don't need to disconnect and reconnect manually

### "Claude is not logged in" or auth error in chatroom

ClawTerminal detects when the Claude CLI reports that you are not authenticated and shows a step-by-step recovery prompt inline. To fix it:

1. Tap the **Terminal** tab to open a raw SSH session to your Mac
2. Run `claude` to start the Claude CLI interactively
3. Type `/login` and follow the browser OAuth flow
4. Return to the chatroom — it will work on the next message

This only needs to be done once per Mac. The session token is stored in Claude CLI's local config and survives restarts.

### Port forwarding not working

- Confirm the service is actually running and bound to the expected port on the remote host
- Ensure no firewall is blocking the local port on your device
- Each local port can only be used by one tunnel at a time

### App feels slow or unresponsive after connection

- Close unused terminal tabs (swipe left on the tab label)
- Heavy terminal output (e.g. `cat` of large files) is batched — give it a moment
- Restart the SSH connection from the Connections tab if the session hangs

### Chatroom lost my project directory or context after reconnect

- This should no longer happen — ClawTerminal locks the project directory to the active Claude session ID when the first message is sent, and persists it across reconnects and app restarts
- If it does happen, open the chatroom's **Info** tab, verify the **Project Directory** path is set correctly, then send a new message to relock it

---

## 15. Feature Tutorials

See the [tutorials/](tutorials/) directory for in-depth guides on individual features:

- **[Multi-Model Comparison (`/race`)](tutorials/race.md)** — Race 2–4 AI models on the same prompt simultaneously with side-by-side results and an AI-generated comparison summary
- **[GitHub PR Workflow (`/pr`)](tutorials/pr-workflow.md)** — Auto-generate PRs from git diff, AI code review with severity-colored items and review learning, CI status checks
- **[Session Handoff (`/handoff`)](tutorials/handoff.md)** — Bidirectional handoff between phone and Mac; start on your commute, continue at your desk without losing context
- **[Live Web Preview (`/preview`)](tutorials/preview.md)** — SSH port-forwarded live web preview of your dev server with auto-detection, auto-start, and one-command teardown
- **[Agent Teams (`/team`)](tutorials/agent-teams.md)** — Wave-based orchestration with Research → Implement → Review phases, discovery propagation between waves, and visual command center with animated flow graph
- **[Batch Multi-Agent Orchestration (`/batch`)](tutorials/batch-multi-agent.md)** — Commander + N parallel Workers + Synthesizer, with `--agents`, `--multi` cross-tool assignment, `--ckpt`, and `--skills` flags
- **[Smart Commands](tutorials/smart-commands.md)** — User-defined slash commands with named parameters, background auto-submit, tool overrides, skill injection, and auto-batch execution
- **[Scheduling Recurring Background Jobs](tutorials/scheduled-jobs.md)** — Automate repetitive tasks with hourly, daily, weekly, or custom schedules
- **[Understanding Agent Checkpoints](tutorials/agent-checkpoints.md)** — Track long-running job progress with manual `[CHECKPOINT]` markers or automatic `--ckpt` detection
- **[Multi-CLI Tool Support](tutorials/multi-cli-tools.md)** — Use Aider, Codex, or custom CLI tools alongside Claude
- **[Using ClawTerminal on iPad](tutorials/ipad-multi-window.md)** — Sidebar layout, Stage Manager multi-window, shared SSH sessions
- **[AI Error Diagnosis](tutorials/ai-error-diagnosis.md)** — One-tap terminal error diagnosis via floating "Ask Claude" pill
- **[Snap & Code: Camera to Code](tutorials/snap-and-code.md)** — Photograph a mockup or error screen, get running code
- **[Conversation Export](tutorials/conversation-export.md)** — Save conversations as Markdown or PDF
- **[SFTP Batch Operations](tutorials/sftp-batch-operations.md)** — Multi-select files for bulk download and delete
- **[Custom Terminal Themes](tutorials/custom-terminal-themes.md)** — Design your own 16-color ANSI terminal palette
- **[Inline Code Execution](tutorials/inline-code-execution.md)** — Run code blocks from Claude's responses with one tap
- **[Code Snippet Library](tutorials/code-snippets.md)** — Save and reuse code blocks from conversations
- **[Smart Notifications](tutorials/smart-notifications.md)** — Intelligent alerts for job completion, long responses, and to-do completion
- **[Cross-Session AI Memory](tutorials/cross-session-memory.md)** — Save facts with `/remember`, auto-injected into every chatroom session
- **[Shared Chatroom](tutorials/shared-chatroom.md)** — Share your AI session via room codes for real-time watch-only viewing

---

## Version History

| Version | Date | Highlights |
|---------|------|------------|
| **v1.0** | March 16, 2026 | Initial release (US/Canada). SSH terminal, Claude AI chatroom, SFTP, background jobs, scheduled jobs, iPad multi-window, watchOS companion. |
| **v1.1.0** | March 25, 2026 | Agent Teams (`/team`), `/batch` multi-agent orchestration, smart commands, SFTP create folder/file, 8-page welcome tour, SSH auto-reconnect hardening, cross-session memory improvements. |
| **v1.1.1** | April 2, 2026 | iPhone slide-out drawer sidebar, iPad sidebar fix, adaptive background job polling, Agent Teams discovery improvements, SSH robustness, keyboard polish, `/help` redesign, background heartbeat notifications. Now available in **80+ countries worldwide**. |
| **v1.2.0** | April 2026 | Multi-model race (`/race`) with thinking lenses, GitHub PR workflow (`/pr`) with AI code review and review learning, bidirectional session handoff (`/handoff`), live web preview (`/preview`), smart model routing, git worktree isolation (`--vcs`), agent reasoning banners, per-job cost tracking, trajectory timeline, per-project skill variants, subagent isolation, progressive skill disclosure, auto-compaction, smart memory search. |
| **v1.3.0** | April 2026 | Visual git branch graph (`/git`), codebase health dashboard (`/health`), live server monitor (`/monitor`), AI semantic code search (`/search`), voice input (`/voice`). Git worktree mode and smart model routing now fully documented with examples. Agent reasoning banner available on all background job results. |
| **v1.4.0** | April 2026 | AI merge conflict resolver (`/conflicts`), smart notifications (`/notify`), enhanced plan mode (`/plan` with Execute button), `@web` URL context injection, file context pinning (`/pin`/`/unpin`). |
| **v1.5.0** | April 2026 | Analytics dashboard (`/dashboard`), workflow pipelines (`/workflow`) with visual DAG execution, auto-recovery for failed jobs (error classification + auto-retry + retry lineage), auth error auto-switch to Terminal tab with recovery banner. Auto-approve skill banner removed. |

---

## Advanced Features Reference

### Progressive Skill Disclosure

By default, every enabled skill's full content is injected into every message — which consumes tokens even when the skill isn't relevant. Progressive disclosure lets you mark a skill as keyword-triggered: the skill's name and description always appear in the index, but the full content only loads when your message contains a matching keyword.

To configure a skill for progressive disclosure:

1. Open the **Skills** toolbar button in any chatroom
2. Tap the blue pencil icon on any installed skill row to open the editor
3. In the **Progressive Disclosure** section, toggle **Always inject** off
4. Add comma-separated **keywords** that trigger the full content, e.g. `deploy, staging, rollout, release`

Example: a "Deploy Staging" skill with keywords `deploy, staging, rollout` stays dormant on every normal coding turn. The moment you write "deploy to staging", the full instructions load automatically. For users with many skills, this can cut preamble token usage by roughly 50%.

---

### Auto-Skill Suggestions

After a standalone `/submit` job completes with 3 or more tool uses, a yellow banner appears above the input bar offering to save that job pattern as a reusable slash command. Tap **Save** to open the command editor with the name and template pre-filled — then add parameters and customize as needed.

The suggestion fires only for standalone jobs (not `/batch` or `/team` children), and only once per unique request pattern. If you dismiss it, the same job pattern won't prompt again.

---

### Per-Job Cost Tracking

Every `/submit`, `/batch`, and `/team` job tracks token usage and estimates USD cost in real time.

- The **Jobs tab** shows a green cost chip next to each job's status pill
- The **job detail view** has a **Cost & Usage** card with input tokens, output tokens, estimated USD, and model ID
- **Settings → Cost & Budget** lets you set an optional soft cap (No Limit / $0.50 / $2.00 / $5.00 / $10.00). Jobs over the cap are flagged with a red "Over Budget" badge — the cap is informational and does not pause or cancel jobs

Pricing is based on public list prices as of April 2026 for Claude Opus/Sonnet/Haiku, GPT-4o, o3/o4-mini, and Gemini 2.5 Pro / 2.0 Flash.

---

### Trajectory Timeline

Every background job captures a step-by-step record of what the agent did during execution. Open any job's detail view and scroll to the **Trajectory** card to see a color-coded timeline:

| Color | Icon | Event type |
|-------|------|-----------|
| Blue | Wrench | Tool call (Read, Bash, Edit, Grep, etc.) |
| Cyan | Return | Tool result |
| Purple | Bubble | Assistant text |
| Indigo | Brain | Thinking block |
| Orange | Flag | Checkpoint marker |

Each step shows a one-line summary and a monospaced detail snippet. The trajectory is capped at 200 steps per job and persists across app restarts. For failed multi-agent runs, this lets you scroll back and see exactly what each agent was doing at any point in time.

---

### Per-Project Skill Variants

Skills can be customized for individual projects without modifying the global version. When a skill is injected into Claude's context, the app checks whether a variant exists for the current project — if yes, the variant is used instead of the global content.

**Manual customization:** Open any skill in the editor (tap the blue pencil icon), scroll to **Project Variants**, and tap **Customize for [project-name]**. A nested editor opens pre-filled with the global content. Edit to reflect project-specific conventions, tool versions, or file layouts. The global content is never modified.

**Auto-customization (opt-in):** Enable **Settings → Smart Skills → Auto-customize skills per project**. After each assistant response, the app checks whether the conversation reveals project-specific facts that would improve an enabled skill. If so, it runs a lightweight background analysis and — if a useful customization is found — shows a green banner offering to review the proposed variant. Every proposed variant requires explicit approval before it is saved. The global content is never touched automatically.

---

### Memory Management

The **Memories** button (brain icon) in any chatroom toolbar opens the Memory Library. Use the filter tabs at the top to browse by scope:

| Tab | What it shows |
|-----|--------------|
| This Session | Project-specific memories plus all globals — what Claude actually sees |
| Global | Memories that apply across all sessions and projects |
| All | Every memory, with folder badges showing which project each belongs to |

Tap any memory to edit its content, category, keywords, or scope (Global vs. This Project Only). The search bar runs a ranked full-text search: exact matches score highest, followed by prefix and substring matches, with recency and frequency bonuses. Results appear under a "Smart Search" header instead of the normal category-grouped view.

To create a memory without a slash command, tap the **+** button in the toolbar. To remove a memory, open it and tap **Delete**.

---

### Background Notifications

When ClawTerminal is backgrounded with running jobs, the system checks periodically for completions. When a job finishes while you are away, you receive a "Job Complete" notification with a result preview and **View** / **Copy** action buttons — the same notification you would see in the foreground. Notifications for still-running jobs are sent only when they are new, preventing repeated "N jobs running" reminders for the same jobs.

---

## Contributing

Found an error or want to add a tip? Open a pull request against this repo. For app bugs or feature requests, open an issue on the [main app repo](https://github.com/LiqunChen0606/clawterminal).

---

ClawTerminal — SSH + Claude AI for iOS
