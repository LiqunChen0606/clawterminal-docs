# ClawTerminal — Guides & Tutorials

> **ClawTerminal** is an iOS SSH terminal + Claude AI assistant that connects your iPhone or iPad to your Mac and remote servers.

[![Platform](https://img.shields.io/badge/Platform-iOS%2017%2B-blue)](https://github.com/LiqunChen0606/clawterminal)
[![Claude](https://img.shields.io/badge/AI-Claude%20Opus%204.6-purple)](https://anthropic.com)
[![SSH](https://img.shields.io/badge/Protocol-SSH%2FSFTP-green)](https://github.com/LiqunChen0606/clawterminal)

---

## 📖 Table of Contents

1. [Getting Started](#1-getting-started)
2. [SSH Setup — Connect to Your Mac (Local Network)](#2-ssh-setup--connect-to-your-mac-local-network)
3. [Tailscale — Remote Access Anywhere (Recommended for remote use)](#3-tailscale--remote-access-anywhere)
4. [SSH Keys](#4-ssh-keys)
5. [Claude AI Chatroom](#5-claude-ai-chatroom) (includes [Background Jobs](#background-jobs-submit) and [Agent Orchestration](#agent-orchestration-orchestrate))
6. [Skills & Marketplace](#6-skills--marketplace)
7. [Slash Commands & @ References](#7-slash-commands---references)
8. [Terminal Shortcuts](#8-terminal-shortcuts)
9. [Port Forwarding & Tunnels](#9-port-forwarding--tunnels)
10. [SFTP File Browser](#10-sftp-file-browser)
11. [MCP Servers](#11-mcp-servers)
12. [Tips & Tricks](#12-tips--tricks)
13. [Troubleshooting](#13-troubleshooting)
14. [Feature Tutorials](#14-feature-tutorials)

---

## 📚 Feature Tutorials

In-depth guides for individual ClawTerminal features. Each tutorial covers a single feature with step-by-step instructions.

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

### First Launch

When you open ClawTerminal for the first time you will see the **Welcome Tour**. It walks you through the four main features:

1. **SSH Terminal** — Full colour terminal with tmux, git, vim shortcuts
2. **Claude AI** — Chat with Claude, run Claude Code on your Mac
3. **My Mac Mode** — One-tap connect to your own Mac with AI integration
4. **Background Jobs** — Submit long-running tasks and get notified when done

Tap **Get Started** on the last page to begin, then follow the **Set Up My Mac** wizard.

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
   - **Username**: your macOS login name (run `whoami` in Terminal to confirm)
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
   - **Username**: your macOS login name (same as what you see in System Settings → Users)
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

### Export & Share Skills

Tap any skill → **Export** to save as JSON. Share with teammates and re-import with **Import**.

---

## 7. Slash Commands & @ References

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

## 8. Terminal Shortcuts

The shortcut bar below the terminal has four categories. Tap the pill labels to switch:

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

## 9. Port Forwarding & Tunnels

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

## 10. SFTP File Browser

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

### Upload from Files App

Tap the **Upload** button (top right) to pick files from the iOS Files app and transfer them to the remote server.

---

## 11. MCP Servers

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

## 12. Tips & Tricks

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

---

## 13. Troubleshooting

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

## 14. Feature Tutorials

See the [tutorials/](tutorials/) directory for in-depth guides on individual features:

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

## Contributing

Found an error or want to add a tip? Open a pull request against this repo. For app bugs or feature requests, open an issue on the [main app repo](https://github.com/LiqunChen0606/clawterminal).

---

ClawTerminal — SSH + Claude AI for iOS
