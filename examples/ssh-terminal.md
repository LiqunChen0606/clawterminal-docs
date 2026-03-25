# SSH Terminal Examples

Practical examples for connecting to your Mac and working in the terminal. All examples assume you have enabled Remote Login on your Mac (System Settings > General > Sharing > Remote Login).

---

## 1. Connect to a Mac via local WiFi

The simplest setup — iPhone and Mac on the same home or office network.

**Profile settings:**
- Host: `macbook.local` (or your Mac's hostname, found in System Settings > General > About)
- Port: `22`
- Username: your macOS username (e.g. `alice`)
- Auth: Password or SSH key

**What to expect:**
After tapping Connect, you land in a full zsh/bash shell. Run `ls`, `vim`, `git` — anything you would in Terminal.app.

**If hostname does not resolve:**
Use the Mac's IP address instead. Find it in System Settings > Network > Wi-Fi > Details.

```
Host: 192.168.1.42
Port: 22
Username: alice
```

---

## 2. Connect via Tailscale (remote access anywhere)

Tailscale creates a private network between your devices. This is the recommended setup for connecting to your Mac from a coffee shop, airplane, or anywhere outside your home network.

**Step 1 — Install Tailscale on your Mac and log in:**

```bash
brew install tailscale
sudo tailscaled &
sudo tailscale up
```

**Step 2 — Enable Remote Login on your Mac** (System Settings > General > Sharing > Remote Login).

**Step 3 — Install Tailscale on your iPhone** from the App Store.

**Step 4 — Find your Mac's Tailscale hostname:**
Open the Tailscale app on your iPhone. Your Mac appears as something like `macbook.tail12345.ts.net`.

**Profile settings in ClawTerminal:**
- Host: `macbook.tail12345.ts.net`
- Port: `22`
- Username: your macOS username
- Auth: Password (recommended for simplicity)

**What to expect:**
Connection works from any network — LTE, airport WiFi, hotel — as long as both devices have Tailscale running.

---

## 3. Import SSH config from ~/.ssh/config

If you already manage connections in `~/.ssh/config` on your Mac, ClawTerminal can import them all at once.

**Example `~/.ssh/config` on your Mac:**

```
Host devserver
    HostName dev.example.com
    User deploy
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

Host staging
    HostName 10.0.1.50
    User ubuntu
    Port 22
```

**Import steps:**
1. Connect to your Mac via SSH first (so ClawTerminal can read the file)
2. Tap the ellipsis (...) menu in the My Mac header
3. Tap **Import SSH Config**
4. ClawTerminal reads `~/.ssh/config` and shows all non-wildcard hosts
5. Select the hosts you want — tap **Select All** to grab everything
6. Tap **Import N** — they appear in your connection list immediately

After import, `devserver` and `staging` are ready to connect with one tap.

---

## 4. Use Mosh for mobile-friendly connections

Mosh keeps your session alive through network changes and sleep. Ideal for commuting or switching between WiFi and LTE.

**Install Mosh on your Mac:**

```bash
brew install mosh
```

**Profile settings in ClawTerminal:**
1. Create or edit a connection profile
2. Under **Transport**, switch from SSH to **Mosh**
3. Host, port, and username stay the same

**What to expect:**
- Switching from WiFi to LTE does not drop the session
- Brief network interruptions (subway tunnel, elevator) reconnect silently
- The terminal shows `[mosh]` in the title bar

**Note:** Mosh requires UDP ports 60000–61000 to be open on your Mac's firewall, and port forwarding may need adjusting if you're behind a router.

---

## 5. Port forwarding — local tunnel (access Mac's dev server on iPhone)

Forward a port from your Mac to your iPhone so you can test a local web server in Safari.

**Example: Mac running a React dev server on port 3000**

1. Open a connection profile
2. Tap **Port Forwarding** > **Add Tunnel**
3. Set:
   - Direction: **Local**
   - Local port: `3000`
   - Remote host: `localhost`
   - Remote port: `3000`
4. Save and connect

After connecting, open `http://localhost:3000` in Safari on your iPhone — it loads your Mac's dev server.

**Common local tunnel uses:**
- `3000` → React / Next.js dev server
- `8080` → Django / Flask / FastAPI
- `5173` → Vite
- `5432` → PostgreSQL (for a database GUI on iPhone)

---

## 6. Port forwarding — remote tunnel (expose iPhone app to the internet)

A remote tunnel lets you expose a service running on your iPhone to the public internet through your Mac as a jump host. Less common, but useful for webhook testing.

**Example: Expose a local Flask server running at port 5000**

1. Open Port Forwarding > Add Tunnel
2. Set:
   - Direction: **Remote**
   - Remote port: `5000`
   - Local host: `localhost`
   - Local port: `5000`
3. Save and connect

Anyone who can reach your Mac at port 5000 is now forwarded to your iPhone.

---

## 7. Extended keyboard shortcuts

The extended keyboard bar sits above the iOS keyboard. Tap the pill labels (Shell / Tmux / Git / Vim) to switch categories.

### Shell shortcuts

| Button | What it sends | When to use |
|--------|--------------|-------------|
| `^C` | Ctrl+C (SIGINT) | Stop a running process |
| `^D` | Ctrl+D (EOF) | Log out of the shell |
| `^L` | Ctrl+L | Clear the screen |
| `^Z` | Ctrl+Z | Suspend a process to background |
| `^R` | Ctrl+R | Search command history |
| `^A` | Ctrl+A | Jump to start of line |
| `^E` | Ctrl+E | Jump to end of line |
| `^W` | Ctrl+W | Delete the previous word |
| `!!` | `!!` + Enter | Re-run the last command |
| `sudo !!` | `sudo !!` + Enter | Re-run last command as root |

### Practical examples

**You started a server and want to stop it:**
Tap `^C`

**You ran a long command and wish you had used sudo:**
Tap `sudo !!`

**You're in vim and want to exit:**
Switch to the Vim tab → tap `:wq`

**You want to split a tmux window:**
Switch to the Tmux tab → tap `split |` for a vertical split or `split —` for horizontal

### Git shortcuts (Git tab)

```
status    → git status
diff      → git diff
log       → git log --oneline -10
push      → git push
pull      → git pull
add .     → git add .
commit    → git commit -m ""  (cursor inside quotes, ready to type message)
stash     → git stash
stash pop → git stash pop
```

---

## 8. Dismiss keyboard without losing terminal focus

Tap the **chevron-down button** at the right edge of the extended keyboard bar. The keyboard slides away, giving you full screen for the terminal output. Tap anywhere in the terminal or the input bar to bring it back.

This is useful when Claude is streaming a long response and you want to read without the keyboard taking up half the screen.
