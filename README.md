# ClawTerminal — Guides & Tutorials

> **ClawTerminal** (also known as **CatClaw**) is an iOS SSH terminal + AI chatroom (Claude, Codex, Gemini, Aider) that connects your iPhone, iPad, or Apple Watch to your Mac and remote servers. Run AI agents, manage files, schedule jobs, and collaborate — all from your pocket.

[![Download on the App Store](https://img.shields.io/badge/Download-App%20Store-blue?logo=apple&logoColor=white)](https://apps.apple.com/us/app/clawterminal/id6759690902)
[![Platform](https://img.shields.io/badge/Platform-iOS%2017%2B%20%7C%20iPadOS%20%7C%20watchOS-blue)](https://apps.apple.com/us/app/clawterminal/id6759690902)
[![AI](https://img.shields.io/badge/AI-Claude%20%7C%20Codex%20%7C%20Gemini%20%7C%20Aider-purple)](https://apps.apple.com/us/app/clawterminal/id6759690902)
[![SSH](https://img.shields.io/badge/Protocol-SSH%2FSFTP%2FMosh-green)](https://apps.apple.com/us/app/clawterminal/id6759690902)

**Latest Version:** v1.2.0 (April 2026) — Sprint 46+47 ships `/race` multi-model comparison, `/pr` GitHub PR workflow with AI code review, `/handoff` bidirectional session handoff, `/preview` live web preview via SSH port forwarding, AI code review learning, and slash command autocomplete for all new commands.

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

---

## 📚 Feature Tutorials

In-depth guides for individual ClawTerminal features. Each tutorial covers a single feature with step-by-step instructions.

### New Features (Sprint 46 + 47)

| Tutorial | Description |
|----------|-------------|
| [Multi-Model Comparison (`/race`)](tutorials/race.md) | Race 2–4 AI models on the same prompt simultaneously. Side-by-side results on iPad, swipeable cards on iPhone, with an AI-generated comparison summary. Includes same-model thinking lenses (`--copies`, `--lenses`): Adversarial, Pragmatic, Principled, User-First, Skeptic, Optimizer |
| [GitHub PR Workflow (`/pr`)](tutorials/pr-workflow.md) | Full GitHub PR lifecycle: auto-generate PR title and body, AI-powered code review with severity-colored items (red/yellow/blue/green), CI status checks, and per-chatroom review learning with thumbs-up/down ratings |
| [Session Handoff (`/handoff`)](tutorials/handoff.md) | Bidirectional handoff between phone and Mac: send your current session to a Mac tmux window, or discover and pick up active Mac sessions on your phone. Uses Claude's `--resume` flag for seamless context continuity |
| [Live Web Preview (`/preview`)](tutorials/preview.md) | SSH-tunneled live preview of your dev server in-app. Auto-detects port from package.json, .env, or vite.config. Multi-port tab switching, `--start` auto-launch, console log panel, screenshot+annotate, responsive viewport modes |
| [AI Code Review Learning](tutorials/pr-workflow.md#part-3-review-learning) | Thumbs-up/down feedback on individual review items teaches CatClaw your team's preferences. Set focus areas with `/pr focus security,tests,performance`. Builds up over 5–10 reviews |

### New Features & Improvements (Sprint 44)

| Area | Change |
|------|--------|
| **iPhone Slide-Out Drawer** | The bottom TabView on iPhone has been replaced with a slide-out drawer sidebar (similar to the ChatGPT and Claude apps). Tap the hamburger menu button (top-left) to open a 280pt drawer with spring animation. Tap the dimmed overlay or swipe to dismiss. All tabs (My Mac, Terminal, Connections, Settings) are now accessed from the drawer. |
| **iPad Sidebar Fix** | The `NavigationSplitView` detail view now properly updates when switching tabs in the sidebar. Previously, tapping a different sidebar row could leave the detail view stale. |
| **Agent Teams Discovery Improvements** | Worker output limit raised from 3K to 10K characters, capturing significantly more context from each agent. Discovery parsing fallback improved from 200-character truncation to sentence-based chunking for more meaningful extractions. When SSH is unavailable during discovery extraction, partial discoveries are now created from available output instead of being silently dropped. |
| **Adaptive Background Job Polling** | Background job polling interval now adapts based on activity: 3 seconds while the job is actively producing output, 8 seconds after 30 seconds of idle, and 15 seconds after 2+ minutes of idle. This reduces SSH command load and battery usage for long-running jobs without sacrificing responsiveness. |
| **Jobs Tab Auto-Collapse** | Batch and team job groups now auto-collapse in the Jobs tab, reducing visual clutter when many groups are present. Individual jobs can still be expanded on demand. |
| **Job Detail Auto-Scroll** | Auto-scroll in the job detail view is now disabled by default. Previously the view would continuously scroll to the bottom as new output arrived, making it difficult to read earlier output. |
| **`/usage` Command** | New `/usage` slash command added as an alias for `/cost`. Displays current token usage and cost information for the active session. |
| **`/help` Redesign** | The `/help` output has been redesigned with grouped code-block tables, organizing commands by category for easier scanning and discovery. |
| **Background Heartbeat Notifications** | When the app is backgrounded with running jobs, periodic heartbeat notifications are sent to keep the user informed that jobs are still in progress. |

### Bug Fixes & Improvements (Post-Sprint 43)

| Area | Fix |
|------|-----|
| **SSH auto-reconnect** | Latency monitor now retries 3x with 15-second backoff before giving up. `ensureHealthySSH()` actively triggers a reconnect rather than just waiting. |
| **Claude CLI "not logged in"** | App detects auth errors and shows step-by-step recovery: go to the Terminal tab, run `claude`, type `/login`. |
| **Skills injection** | App-only skills (e.g. Marketplace packs that use iOS-side callbacks) injected into the CLI preamble now include a disclaimer so Claude CLI does not try to invoke them as tools. |
| **Background job cross-context** | `/submit` and `/batch` jobs now inject the 3 most recently completed job results into new jobs, so agents can reference prior work without manual copy-paste. |
| **First-time setup** | A connection failure during the My Mac wizard no longer flips back to the first wizard page. Instead it shows an inline error message with a **Retry** button so you can fix the issue without re-entering details. |
| **File picker** | Two separate `.fileImporter` modifiers were merged into one (SwiftUI only supports one per view). The attachment dialog now correctly opens the iOS Files app for markdown, code, PDFs, and other file types. |
| **WiFi/Tailscale username** | The Username field no longer pre-fills "mobile". It shows empty with a `whoami` tip — run `whoami` in Terminal on your Mac and enter the result. |
| **Terminal keyboard dismiss** | The chevron-down button is now present on the extended keyboard bar (terminal mode), consistent with the chatroom input bar. |

### New Features (Sprint 43)

| Tutorial | Description |
|----------|-------------|
| [Agent Teams (`/team`)](tutorials/agent-teams.md) | Wave-based orchestration: Research → Implement → Review waves with parallel agents, discovery propagation between waves, visual command center with animated flow graph and live discovery feed |

### New Features (Sprint 42)

| Tutorial | Description |
|----------|-------------|
| [Batch Multi-Agent Orchestration (`/batch`)](tutorials/batch-multi-agent.md) | Commander decomposes your goal, N parallel Workers execute it, Synthesizer merges results. Supports `--agents`, `--multi`, `--ckpt`, `--skills` |
| [Smart Commands](tutorials/smart-commands.md) | User-defined slash commands with named parameters, background auto-submit, tool overrides, skill injection, and auto-batch execution |

### New Features (Sprint 29+)

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
| `/batch <goal> [--agents N] [--multi] [--ckpt] [--skills alias,...]` | Spawn a **Commander + N Worker agents + Synthesizer**. Commander decomposes the goal dynamically. `--multi` assigns different tools (Claude/Codex/Gemini/Aider) to different subtasks. See [Batch Multi-Agent Orchestration](#batch-multi-agent-orchestration-batch). |
| `/team <goal> [--multi]` | Wave-based orchestration — Commander decomposes into sequential waves (Research → Implement → Review), agents run in parallel within each wave, discoveries propagate between waves. Visual command center with animated flow graph and live discovery feed. See [Agent Teams](#agent-teams-team). |
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
| `/remember <fact>` | Save a fact to **cross-session memory** — injected into every future chatroom session |
| `/forget <keyword>` | Remove memories matching the keyword |
| `/memories` | Open the **Memory Library** to browse, search, and manage saved memories |
| `/help` | List all available slash commands |

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
| **v1.1.1** | April 2, 2026 | iPhone slide-out drawer sidebar, iPad sidebar fix, adaptive background job polling, Agent Teams discovery improvements, SSH robustness, keyboard polish, `/help` redesign, background heartbeat notifications. Now available in **80+ countries worldwide** (expanded from initial US/Canada launch). |
| **v1.1.1 Sprint 45** | April 10, 2026 | Subagent isolation, progressive skill disclosure, auto-compaction, memory search performance, auto-skill suggestions, critical bug fixes. |
| **v1.2.0 Sprint 46** | April 2026 | `/race` multi-model comparison (models + thinking lenses), `/pr` GitHub PR workflow with AI code review, review learning, and CI checks, `/handoff` bidirectional session handoff. |
| **v1.2.0 Sprint 47** | April 2026 | `/preview` live web preview via SSH port forwarding (auto-detect port, `--start`, multi-port, console logs, screenshot annotation, responsive viewport modes), orange `--flag` highlighting, Job tab category pills (All/Jobs/Race/Agents/Scheduled), slash command autocomplete for all new commands. |

---

### New in Sprint 45 (April 2026)

Sprint 45 focused on agent polish and memory/skills v2, inspired by patterns from Hermes Agent and OpenClaw.

**🔒 Subagent Profile Isolation**
Each `/team` and `/batch` agent now gets a private scratch workspace at `~/.catclaw/workspaces/{groupID}/{role}/`. Agents running in parallel can't step on each other's scratch files anymore. Workers are told about their isolated workspace in the prompt and use it for drafts before committing to the main project.

**🎯 Progressive Skill Disclosure**
Skills now support keyword triggers. Instead of every enabled skill dumping its full content into every message, you can mark a skill with keywords like `deploy, staging, release` and turn off "Always inject". The skill's name + description still appears in the index on every turn, but the full content only loads when your message contains a matching keyword. Cuts preamble token usage by ~50% for users with many skills.

**✂️ Auto-Compaction**
When your conversation grows past ~400K characters (roughly 100K tokens, 50% of a 200K context window), the app silently collapses older messages into a summary marker — keeping the first 2 and last 6 messages intact. Zero API calls; the summary is built locally. Long sessions no longer hit the context limit.

**⚡ Faster Memory Search**
Added an inverted keyword index to `MemoryStore` for O(1) candidate lookup (was O(n) per query). New `search(query:limit:)` API supports full-text search across all memories with exact + prefix + substring matching. SQLite FTS5 migration deferred — pragmatic call because typical users have <1000 memories.

**🪄 Auto-Skill Suggestions**
After a standalone `/submit` job completes with 5+ tool uses, a yellow banner appears above the input bar asking: "That job used N tools — save as reusable command `/foo`?" Tap Save to open the command editor with the slug and request pre-filled. Deduped by request fingerprint to avoid spam.

**🐛 Critical Bug Fixes**
- Background job completion messages now persist across app restarts. Previously, the `✅ Background job finished` assistant bubble stayed in the main chat until you relaunched the app — then it disappeared (the result stayed in the Jobs tab). Fixed by calling `notifyMessagesUpdated()` after appending job-completion messages.
- Background job polling now waits up to 60 seconds for SSH reconnect before continuing. Previously, polling would race through cycles during WiFi changes and phone wake events, sometimes falsely declaring the tmux session dead.

### Sprint 45 Polish (mid-April)

A week of post-initial-release polish focused on bugs found during real use and additional UX refinements.

**🔗 Connection Stability**
- Eliminated false "Reconnecting..." flashes during active use. The latency monitor previously tore down the connection on a single 5-second probe timeout — which happened under normal contention when background jobs and foreground messages shared exec channels. Now requires 2 consecutive failures, skips probing entirely while SSH is in active use, and all probe timeouts were raised from 3-5 seconds to 10-12 seconds (a busy Mac can legitimately take that long to respond).
- First-tap saved connection failures are now silently retried. When you tap "Reconnect to [name]" in the wizard, the first attempt occasionally failed due to mDNS warmup on `.local` hostnames, NIO cleanup races, or Tailscale routing delays. The app now waits 1.5 seconds and retries once automatically — most first-tap failures now succeed transparently.

**📝 Persisted Background Job Messages**
Background job completion messages (the ✅ banner in the main chat) now persist across app restarts. Previously, the result stayed in the Jobs tab but disappeared from the main chatroom on relaunch — because `notifyMessagesUpdated()` was missing from the job-completion path.

**🔌 Background Jobs Tolerate Reconnects**
Polling loops now wait up to 60 seconds for `sshService` to become non-nil before continuing the poll cycle. Previously, a 45-second WiFi reconnect window could burn through 15 polls and trigger false "session died" errors.

**✏️ Edit Existing Skills**
A blue pencil icon now appears on each installed skill row, opening the full editor with Name, Description, Content, and the Progressive Disclosure section (keywords + "Always inject" toggle). Before this, there was no way to edit an existing skill — you could only create new ones or delete.

**🧠 Per-Session Memory Management**
Major memory UX overhaul. Open the **Memories** button in any chatroom and you now get:
- **Filter tabs** at the top: `[This Session | Global | All]`
  - *This Session* — project-specific memories + all globals (what the AI actually sees on this turn)
  - *Global* — memories that apply to every session across all projects
  - *All* — every memory everywhere, with orange folder badges showing which project each one belongs to
- **Tap any memory to edit it** — opens a full editor with multiline content, category picker, keywords field, and a **scope toggle**: "Global Memory" ↔ "This Project Only". Flipping the toggle is the one-tap way to share a memory across sessions or scope it back down.
- **New "+" button in the toolbar** creates memories without needing the `/remember` slash command.
- **Delete from inside the editor** with confirmation.

**🔍 Smart Memory Search**
Typing in the memory search bar now runs a ranked full-text search using the Sprint 45 inverted keyword index. Results appear under a purple "Smart Search" badge header instead of category-grouped view, scored by exact match (×3), prefix match (×1.5), substring match (×2), recency, and frequency. Works across all projects when on the "All" filter tab.

**🪄 Auto-Skill Suggestion Improvements**
The "Save as reusable command?" banner now tracks tool use count incrementally as the job runs, rather than counting at completion (where the capped 10KB progress log had already discarded earlier markers). Threshold lowered from 5 tools to 3 — many useful jobs only use 3-4 tools (read, bash, write is already worth saving).

### Cost Tracking & Trajectory Timeline (late April)

Two major additions for background jobs — both inspired by patterns from the Cline VS Code agent.

**💰 Per-Job Cost Tracking**

Every `/submit`, `/batch`, and `/team` job now tracks token usage and estimates USD cost in real time:

- **Jobs tab row** shows a small green cost chip next to the status pill (e.g. `$0.037`)
- **Job detail view** has a new **Cost & Usage** card showing input tokens, output tokens, estimated USD, and the model ID
- **Settings → Cost & Budget** lets you set an optional budget cap per job:
  - No Limit
  - $0.50
  - $2.00
  - $5.00
  - $10.00

If a job exceeds your budget cap, the Cost & Usage card turns red with a "Over Budget" warning badge. The cap is purely informational — it does not block or pause jobs, just flags them for your awareness.

Pricing tables are public list prices as of April 2026 for Claude Opus/Sonnet/Haiku, GPT-4o, o3/o4-mini, and Gemini 2.5 Pro / 2.0 Flash. Actual Anthropic/OpenAI billing may differ by a few percent — this is for awareness, not accounting.

**📈 Trajectory Timeline**

Every background job now captures a trajectory of what the agent actually did during execution:

- **Tool calls** (blue wrench icon) — each time the AI invoked Read, Bash, Edit, Grep, etc.
- **Tool results** (cyan return icon) — the output from those tool calls
- **Assistant text** (purple bubble icon) — the AI's explanatory messages
- **Thinking blocks** (indigo brain icon) — the AI's internal reasoning
- **Checkpoints** (orange flag icon) — milestone markers from `[CHECKPOINT: ...]` output

Open any background job's detail view and scroll to the new **Trajectory** card. You'll see a color-coded vertical timeline with connector lines showing every step the agent took, a one-line summary per step, and a monospaced detail snippet (tool input or text preview) underneath.

This was previously invisible — the Jobs tab only showed the final result and a 10KB-capped progress log. Now you can scroll back through a failed `/team` run and see exactly what Agent 3 was doing at minute 7.

The trajectory is capped at 200 steps per job (to avoid unbounded growth on very long runs) and persists across app restarts.

### Per-Project Skill Variants (late April)

Skills can now be customized for individual projects without touching the global version. Every enabled skill stays globally available for all your sessions, but specific projects can have their own variant that overrides the content only when working in that project.

**🌿 Manual customization** — open any skill in the editor (tap the blue pencil on a skill row), scroll to the new **"Project Variants"** section, and tap **"Customize for [project-name]"**. A nested editor opens pre-filled with the global content; edit it to reflect project-specific conventions, tool versions, file layouts, or preferences. The variant is saved alongside the global skill — the global content is never modified.

When a skill is injected into Claude's context, the app checks: does this skill have a variant for the current project? If yes, the variant is used; if no, the global content is used. Variants are marked with a 🌿 badge or *(project variant)* tag so Claude knows they're project-specific.

**✨ Opt-in auto-customization** — turn on **Settings → Smart Skills → "Auto-customize skills per project"** to enable automatic variant suggestions. The app analyzes your conversation after each assistant response and, when it detects project-specific facts that would make a skill more effective (tool versions, conventions, patterns, file layouts), dispatches a lightweight Claude Haiku analysis in the background. If Haiku determines the skill would benefit from customization, you'll see a green banner above the input bar:

> 🌿 Project variant for `deploy-staging`
> Claude suggests customizing this skill for the current project.
> [Review] [Dismiss]

Tap **Review** to open a side-by-side comparison sheet showing the original global content and the proposed variant. Tap **Apply** to save — the global content is never modified; only the project variant is written.

**Rate limits:**
- Max 1 analysis per 10 minutes per chatroom (prevents spam)
- Each (skill, project) pair is analyzed at most once per session
- Only enabled skills are considered
- Skills that already have a variant for the current project are skipped
- Uses Claude Haiku for cost efficiency (cheapest model in the pricing table)

**Safety guarantees:**
- The global skill content is **never** modified by any automatic or manual flow
- Every proposed variant requires **explicit user approval** before it's written
- Default is **OFF** (opt-in) — you must explicitly turn on auto-customization in Settings
- Variants are stored in `~/Documents/chatrooms.json` alongside the global skill definition

### Smarter Background Notifications (mid-April)

The 10-minute background refresh now detects **actual job completions** instead of just posting "N jobs still running" on every wake. When the app discovers a job that finished while you were away, it posts a "Job Complete" notification with the result preview and View/Copy action buttons — the same notification you'd get in the foreground. Running job notifications no longer repeat every 10 minutes for the same jobs.

---

### Editing Skills

Skills are where you give Claude (or Codex/Gemini/Aider) repeatable context. To edit an existing skill — including any pre-installed ones:

1. Open a chatroom on the **My Mac** tab
2. Tap the **Skills** button in the toolbar (book icon)
3. On any installed skill row, tap the **blue pencil icon** on the right
4. The editor opens with all fields: Name, Description, Content, and the new **Progressive Disclosure** section

In Progressive Disclosure you can:
- **Always inject** toggle: ON = legacy behavior (full content in every message). OFF = only the name/description goes in the index; full content loads on keyword match.
- **Keywords** field (shown when Always Inject is off): comma-separated trigger words like `deploy, staging, release, rollout`.

Example setup for a "Deploy Staging" skill:
- **Name**: `Deploy Staging`
- **Description**: `Safe deploy procedure for staging environment`
- **Content**: (your markdown instructions)
- **Always inject**: OFF
- **Keywords**: `deploy, staging, rollout, release`

Now the full content only gets injected when you say something like "deploy to staging" or "start the rollout" — saving tokens on every other turn.

---

### Triggering the Auto-Skill Suggestion Banner

After a `/submit` background job completes with 5+ tool uses, the app shows a yellow banner above the input bar asking if you want to save the job as a reusable slash command. Here are example requests that should trigger it:

```
/submit Run all the tests, fix any failures, commit the fixes, and push to the current branch
```

```
/submit Deploy the staging environment: pull latest, run migrations, restart services, and verify health
```

```
/submit Do a security audit: scan dependencies, check auth endpoints, review JWT handling, and write a report
```

```
/submit Check git status, list all branches, show the last 5 commits, run npm test, and report findings
```

Each uses file reads, bash commands, git operations, etc. — enough tools to trigger the 5+ threshold.

**Why you might not see it:**
- The job used fewer than 5 tools (simple one-liner jobs)
- You already saw it for a similar request (dedup by fingerprint of the first 100 chars)
- The job was a `/batch` or `/team` child (only standalone jobs trigger it)

---

### New in Sprint 46 (April 2026)

Sprint 46 adds three major workflow commands: multi-model racing, a full GitHub PR workflow with AI review, and bidirectional session handoff.

**Multi-Model Comparison (`/race`)**

Race Claude, Codex, and Gemini on the same prompt simultaneously and compare their answers in a swipeable or side-by-side layout. An AI summary highlights the key differences. Use `--models` to pick which models compete, or `--copies`/`--lenses` to race the same model with different thinking perspectives. Full details: [Multi-Model Comparison](#multi-model-comparison-race).

```text
/race Write a fizzbuzz function
/race --models claude,codex,gemini Explain event sourcing
/race --copies 3 How should we handle database migrations?
/race --lenses adversarial,pragmatic,optimizer Design a caching strategy
```

**GitHub PR Workflow (`/pr`)**

A complete pull request lifecycle from your phone. Auto-generate PR titles and bodies from `git diff`, get AI-powered code reviews with severity-colored items, list open PRs, and check CI status — all via the `gh` CLI installed on your Mac. Full details: [GitHub PR Workflow](#github-pr-workflow-pr).

```text
/pr create
/pr review 42
/pr list
/pr checks 42
```

**Session Handoff (`/handoff`)**

Start coding on your phone during your commute, sit down at your desk and `/handoff mac` to continue in a full terminal — no copy-pasting, no context loss. Or run `/handoff` to discover active Mac sessions and pick them up on your phone. Full details: [Session Handoff](#session-handoff-handoff).

```text
/handoff mac     # Send current phone session to Mac tmux
/handoff         # Discover Mac sessions, pick one up on phone
```

---

### New in Sprint 47 (April 2026)

Sprint 47 adds live web preview, UI polish, and slash command autocomplete for all new commands.

**Live Web Preview (`/preview`)**

The first mobile tool with a real live web preview. `/preview` auto-detects your dev server port from `package.json`, `.env`, or `vite.config`, then creates an SSH port-forwarding tunnel so you can browse your app directly from your phone. Add `--start` to launch the dev server automatically, use `--port N` for a specific port, and `/preview stop` to close the tunnel. Full details: [Live Web Preview](#live-web-preview-preview).

```text
/preview              # Auto-detect port, open preview
/preview --port 3000  # Specific port
/preview --start      # Start dev server + preview
/preview stop         # Close tunnel
```

**Slash Command Autocomplete**

All Sprint 46 and Sprint 47 commands — `/race`, `/pr`, `/handoff`, and `/preview` — now appear in the slash command suggestion palette when you type `/` in any chatroom. Each entry shows the command name, a short description, and the available flags. The palette uses fuzzy search so typing `/rac`, `/han`, or `/prev` finds the right command immediately.

**Orange `--flag` Highlighting**

As you type in the chatroom input bar, any `--flag` argument (such as `--agents`, `--models`, `--port`, `--ckpt`, `--skills`, `--start`, `--copies`, `--lenses`) turns orange automatically. This makes it easy to spot flags at a glance and confirms the app recognizes the flag before you send the message.

```text
/batch --agents 4 --multi implement the payment flow
        ^^^^^^^^^  ^^^^^^^ these turn orange as you type
```

**Job Tab Categories**

The Background Jobs panel now has a color-coded pill tab bar at the top to filter the job list by type:

| Tab | Color | Shows |
|-----|-------|-------|
| All | Gray | Every job |
| Jobs | Blue | Single `/submit` background jobs |
| Race | Orange | `/race` multi-model comparison runs |
| Agents | Purple | `/batch`, `/team`, and `/orchestrate` groups |
| Scheduled | Green | Recurring scheduled jobs |

Tap any pill to filter. The active tab is highlighted. This replaces the previous flat list which became unwieldy when many job types were mixed together.

---

## Contributing

Found an error or want to add a tip? Open a pull request against this repo. For app bugs or feature requests, open an issue on the [main app repo](https://github.com/LiqunChen0606/clawterminal).

---

ClawTerminal — SSH + Claude AI for iOS
