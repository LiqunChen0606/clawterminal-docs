# ClawTerminal — Guides & Tutorials

> **ClawTerminal** is an iOS SSH terminal + Claude AI assistant that connects your iPhone or iPad to your Mac and remote servers.

[![Platform](https://img.shields.io/badge/Platform-iOS%2017%2B-blue)](https://github.com/LiqunChen0606/clawterminal)
[![Claude](https://img.shields.io/badge/AI-Claude%20Opus%204.6-purple)](https://anthropic.com)
[![SSH](https://img.shields.io/badge/Protocol-SSH%2FSFTP-green)](https://github.com/LiqunChen0606/clawterminal)

---

## 📖 Table of Contents

1. [Getting Started](#1-getting-started)
2. [SSH Setup — Connect to Your Mac](#2-ssh-setup--connect-to-your-mac)
3. [SSH Keys (Recommended)](#3-ssh-keys-recommended)
4. [Claude AI Chatroom](#4-claude-ai-chatroom)
5. [Terminal Shortcuts](#5-terminal-shortcuts)
6. [Port Forwarding & Tunnels](#6-port-forwarding--tunnels)
7. [SFTP File Browser](#7-sftp-file-browser)
8. [MCP Servers](#8-mcp-servers)
9. [Tailscale — Remote Access Anywhere](#9-tailscale--remote-access-anywhere)
10. [Tips & Tricks](#10-tips--tricks)
11. [Troubleshooting](#11-troubleshooting)

---

## 1. Getting Started

### Requirements
- iPhone or iPad running **iOS 17** or later
- A Mac (macOS 13 Ventura or later) **OR** any SSH-accessible Linux/BSD server
- (Optional) [Tailscale](https://tailscale.com) for remote access outside your local network

### First Launch
When you open ClawTerminal for the first time you will see the **Welcome Tour**. It walks you through the four main features:

1. **SSH Terminal** — Full colour terminal with tmux, git, vim shortcuts
2. **Claude AI** — Chat with Claude, run Claude Code on your Mac
3. **My Mac Mode** — One-tap connect to your own Mac with AI integration
4. **Background Jobs** — Submit long-running tasks and get notified when done

Tap **Get Started** on the last page to begin, then follow the **Set Up My Mac** wizard.

---

## 2. SSH Setup — Connect to Your Mac

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
   - **Auth Method**: Password or SSH Key (see §3)
4. Tap **Save**, then tap the profile to connect

> **Tip:** Use the **My Mac wizard** (My Mac tab → Set Up My Mac) for a guided setup that handles key generation, authorization, and Tailscale automatically.

---

## 3. SSH Keys (Recommended)

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

## 4. Claude AI Chatroom

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

### Room Memory (Context Document)
Each chatroom has a **Context Document** — a free-text field injected into every system prompt. Use it to describe your project, preferences, or standing instructions.

### Skills
**Skills** are Markdown snippets that augment Claude's system prompt. Enable pre-built skills (code review, bash expert, etc.) or write your own in **Settings → Skills**.

### Background Jobs (`/submit`)
Submit long-running tasks to run in the background while you do something else:

```
/submit refactor the auth module to use async/await
```

Progress updates appear in the **Jobs** panel. A notification fires when the task completes.

---

## 5. Terminal Shortcuts

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

## 6. Port Forwarding & Tunnels

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

## 7. SFTP File Browser

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

## 8. MCP Servers

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

## 9. Tailscale — Remote Access Anywhere

[Tailscale](https://tailscale.com) creates a secure private network between your devices so you can SSH to your Mac from anywhere — coffee shop, hotel, cellular — without opening ports or configuring a VPN.

### Setup via My Mac Wizard (Recommended)

The My Mac setup wizard has built-in Tailscale support — no manual profile creation needed:

1. Open the **My Mac** tab → tap **Set Up My Mac**
2. On Step 2 ("Find Your Mac"), tap the **Tailscale** segment in the picker
3. Enter your Mac's Tailscale hostname (e.g. `my-macbook.tail1234.ts.net`) and your Mac username
4. Choose your auth method:
   - **Password** — enter your Mac login password on the next screen. No SSH key setup required.
   - **SSH Key** — generates a key and walks you through authorizing it on your Mac
5. Tap **Test Connection** then **Finish**

### Manual Setup

1. Install Tailscale on both your Mac and iPhone/iPad (free tier available)
2. Sign in with the same account on both devices
3. In the Tailscale app on your iPhone, find your Mac's MagicDNS name (e.g. `yourMac.tailXXXX.ts.net`)
4. In ClawTerminal → **Connections** tab → tap **+** → enter the Tailscale hostname

Your connection is end-to-end encrypted and requires no port forwarding on your router.

---

## 10. Tips & Tricks

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

## 11. Troubleshooting

### "Connection timed out" or connection keeps spinning
- Confirm **Remote Login** is enabled on your Mac (**System Settings → General → Sharing**)
- Run `ping YourMac.local` on another device to confirm the hostname resolves
- Make sure your iPhone and Mac are on the **same Wi-Fi network**
- If outside your home network, set up Tailscale (§9)
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

## Contributing

Found an error or want to add a tip? Open a pull request against this repo. For app bugs or feature requests, open an issue on the [main app repo](https://github.com/LiqunChen0606/clawterminal).

---

*ClawTerminal — SSH + Claude AI for iOS*
