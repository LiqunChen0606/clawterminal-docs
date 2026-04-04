# One Slash Command, Six AI Agents: How CatClaw Terminal Turns Your iPhone and iPad Into a Coding War Room

## SSH terminal + AI chatroom + multi-agent orchestration — one native app, zero laptops required

---

### The moment that started it all

Picture this: It's 11 PM. You're on the couch. Your phone buzzes — the CI pipeline is on fire. 🔥

You know exactly what's wrong. It's that one config file. A two-line fix. But your laptop is upstairs, charging, lid closed. Your cat is on your lap. Moving is not an option.

So you open a basic SSH app on your phone, squint at the tiny terminal, type the fix... and then realize you actually *don't* know what's wrong. You need an AI to look at the logs. But the AI chat is a different app. You're copy-pasting terminal output between apps like it's 2019.

I lived this scenario too many times. So I built the app I wished existed.

**CatClaw Terminal** is a native iOS app that combines a full SSH terminal with an AI chatroom — Claude, Codex, Gemini, or Aider — and lets you orchestrate teams of AI agents, all from your iPhone or iPad.

It's free on the [App Store](https://apps.apple.com/us/app/clawterminal/id6759690902) in 80+ countries. And yes, the cat can stay on your lap.

<!-- TODO: Record typing cat animation GIF and upload to clawterminal-docs/assets/
![CatClaw typing cat mascot](https://raw.githubusercontent.com/LiqunChen0606/clawterminal-docs/main/assets/typing_cat.gif)
*Meet the CatClaw mascot. She types when you type. She thinks when Claude thinks. She naps when nothing's happening.*
-->


---

## 🤔 "Wait — you code on your *phone*?"

I get this reaction a lot. Let me be clear: **I don't write code on my phone.** I tell AI agents to write code on my Mac, and I supervise from my phone.

There's a big difference between typing `for i in range(10):` on a 6-inch screen (please don't) and typing:

```
/team Refactor the auth module to use JWT tokens, update all tests,
and write a migration guide
```

...and then watching six AI agents do the work while you're in line at Starbucks.

That's the idea behind CatClaw. Your Mac is the muscle. Your phone is the remote control.

```
┌─────────────┐          SSH          ┌─────────────────┐
│  Your iPhone │ ◄──────────────────► │    Your Mac      │
│  (CatClaw)   │     WiFi/Tailscale   │  (Claude, tmux)  │
│              │                      │                  │
│  "Fix it"    │  ──────────────────► │  *fixing...*     │
│              │  ◄────────────────── │  "Done! Here's   │
│  📱          │     push notification│   the diff"      │
└─────────────┘                      └─────────────────┘
```

---

## What you actually get

### 🖥️ A real terminal (not a toy)

This is a full terminal built on SwiftTerm. Vim works. Tmux works. Htop works. Your `.zshrc` loads. Colors render correctly. If it works in Terminal.app, it works here.

But then we added some things Terminal.app can't do:

- **AI autocomplete** — ghost text suggestions appear as you type, like Copilot but for your shell
- **Error diagnosis** — see a red error? Tap the floating "Ask Claude" pill. It reads the last 30 lines and explains what went wrong
- **Shortcut bars** — Shell (^C, ^D, ^Z), Tmux (split, zoom, navigate), Git (status, diff, push), Vim (:wq, dd, /search) — one tap instead of remembering Ctrl combos

### 🤖 AI chatroom (the good part)

Here's where it gets fun. Connect to your Mac, and you're chatting with Claude (or Codex, Gemini, Aider) *running directly on your machine*. Not through an API sandbox — the AI has full access to your files, your git history, your running processes.

```
You: "Why is the user service crashing on startup?"

Claude: [reads 4 files, checks logs, finds the issue]
        "The DATABASE_URL env var is missing from your
         docker-compose.yml. Here's the fix:"
        [shows color-coded diff card]

You: [taps "Run" button on the code block]

Done. From your phone. In bed.
```

Tool calls render as tappable cards — file edits show as color-coded diffs (green = added, red = removed), not walls of JSON. Thinking blocks are collapsible. Code blocks have **Run** and **Save to Snippets** buttons.

### ⚡ Background jobs (fire and forget)

This is my favorite feature for lazy productivity:

```
/submit Run the full test suite and fix any failures
```

The task spins up in a tmux session on your Mac. You put your phone away. Five minutes later: *buzz* — "Job Complete." Tap the notification to see the result, or tap "Copy" to grab the output.

You can even schedule recurring jobs:

- `/submit` with a daily schedule → "Run linting every morning at 9 AM"
- Auto-checkpoints track progress in real-time
- The app adapts its polling: checks every 3 seconds when things are happening, backs off to every 15 seconds when idle (your battery will thank you)

### 🐙 Agent Teams (the showstopper)

One slash command. Six AI agents. Three waves. One result.

```
/team Audit our API for security vulnerabilities and fix them
```

Here's what happens behind the scenes:

```
         ┌──────────────┐
         │  Commander    │  "I'll split this into 3 waves"
         │  (planning)   │
         └──────┬───────┘
                │
    ┌───────────┼───────────┐
    ▼           ▼           ▼
┌────────┐ ┌────────┐ ┌────────┐
│Agent 1 │ │Agent 2 │ │Agent 3 │  Wave 1: Research
│"Check  │ │"Scan   │ │"Review │  (running in parallel)
│ auth"  │ │ inputs"│ │ deps"  │
└────┬───┘ └────┬───┘ └────┬───┘
     │          │          │
     └──── discoveries ───┘
                │
    ┌───────────┼───────────┐
    ▼           ▼           ▼
┌────────┐ ┌────────┐ ┌────────┐
│Agent 4 │ │Agent 5 │ │Agent 6 │  Wave 2: Fix
│"Patch  │ │"Add    │ │"Update │  (informed by Wave 1)
│ SQLi"  │ │ CSRF"  │ │ deps"  │
└────┬───┘ └────┬───┘ └────┬───┘
     │          │          │
     └──────────┼──────────┘
                ▼
         ┌──────────────┐
         │ Synthesizer   │  "Here's everything we did"
         │ (final report)│
         └──────────────┘
```

The `--multi` flag makes it even wilder — it assigns different AI tools to different agents based on their strengths. Claude analyzes. Codex implements. Gemini researches. It's like building a little AI consultancy that runs on your Mac while you're at lunch.

You watch all of this unfold in an animated flow graph. Nodes pulse. Lines animate. It's like a mini mission control on your phone.

<!-- TODO: Record flow graph animation GIF and upload to clawterminal-docs/assets/
![Agent Team flow graph animation](https://raw.githubusercontent.com/LiqunChen0606/clawterminal-docs/main/assets/flow_graph.gif)
*The animated flow graph — purple commander, color-coded waves, indigo synthesizer. All running on your phone.*
-->

### 📁 File browser (yes, on your phone)

Full SFTP browser. Tap through directories, create folders, edit files with syntax highlighting, batch-select and delete. There's even a "Snap & Code" mode — take a photo of a UI mockup, and the AI generates the code.

---

## 🥊 "But what about Claude Code?"

Fair question. **Claude Code** is Anthropic's official CLI — and it's fantastic. If you're at your desk with a terminal open, Claude Code is the best way to use Claude for coding. Full stop.

CatClaw doesn't replace Claude Code. **It wraps it.** CatClaw literally runs `claude` as a process on your Mac via SSH. Same AI, same capabilities — but now you can control it from a touchscreen while your Mac sits in another room (or another city, if you're using Tailscale).

| | Claude Code | CatClaw |
|---|---|---|
| 🖥️ Platform | macOS/Linux terminal | iPhone/iPad app |
| 🤖 AI tools | Claude only | Claude + Codex + Gemini + Aider |
| 🎨 Interface | Text in a shell | Touch UI with diff cards & flow graphs |
| 📋 Background jobs | DIY with tmux | `/submit` + push notifications |
| 🐙 Multi-agent | Limited (sub-agents) | `/team` with wave orchestration |
| ⌚ watchOS | Nope | Dictate jobs from your wrist |

**Where Claude Code wins:** Speed (zero network overhead), MCP ecosystem (deeper integration), permissions (more granular), and it's the official product — it'll always get features first.

**Where CatClaw wins:** You're not at your desk. That's the whole pitch. Also: multi-tool support, agent orchestration, push notifications, and an animated flow graph that makes you feel like you're running a heist.

**Bottom line:**
- At your desk → Claude Code
- Away from your desk → CatClaw controlling Claude Code

They're peanut butter and jelly, not Coke and Pepsi.

---

## 🔒 Security (the boring-but-important part)

An app that holds your SSH keys and AI API credentials is basically a vault. Here's how CatClaw treats it:

- **iOS Keychain** — all credentials stored with hardware-backed encryption (iOS Data Protection). Not in a config file. Not in UserDefaults. Not accessible by other apps.
- **SSH keys generated on-device** — private key never leaves the phone. Ever.
- **Zero analytics SDKs** — no Firebase, no Mixpanel, no crash reporting that uploads your stack traces. The only network traffic is *your* SSH and API calls.
- **Face ID / Touch ID lock** — optional biometric lock on the whole app
- **No cloud servers** — we don't run any infrastructure. Your data goes from your phone to your Mac. That's it.

### How does this compare to OpenClaw?

[OpenClaw](https://github.com/openclaw/openclaw) is the 348K-star open-source AI assistant — and it's a genuinely impressive project. Model-agnostic, extensible, integrates with 20+ messaging platforms. Huge community.

But if you care about credential security, the architectures are *very* different:

**OpenClaw** stores your API keys as **plain text files** in `~/.openclaw/credentials/`. If someone gets access to your machine — malware, compromised SSH, nosy coworker — your keys are right there, readable with `cat`. Its skill marketplace has had **340+ malicious plugins** discovered that could steal credentials or redirect crypto wallets.

**CatClaw** stores credentials in the **iOS Keychain**, protected by hardware encryption and iOS Data Protection. Other apps and processes on the device can't access them — they're sandboxed to CatClaw only. Skills are local JSON files *you* write — no marketplace, no remote code.

| | OpenClaw | CatClaw |
|---|---|---|
| 🔑 Credentials | Plain files on disk | iOS Keychain (hardware-encrypted) |
| 🧩 Plugins/Skills | Public marketplace (340+ malicious found) | Local-only, you write them |
| 📦 Sandboxing | Node.js daemon (full disk access) | iOS app sandbox |
| 🔐 Biometric lock | No | Face ID / Touch ID |
| 📊 Analytics | Configurable | Zero. None. Nada. |

This isn't a knock on OpenClaw — it's a server-side daemon designed for maximum flexibility. CatClaw is a locked-down mobile client designed for maximum paranoia. Different tools, different threat models.

---

## 🏗️ How it's built (for the nerds)

```
┌─────────────────────────────────────┐
│          SwiftUI + TCA              │  ← Presentation
│  iPhone: Slide-out drawer           │
│  iPad: NavigationSplitView sidebar  │
├─────────────────────────────────────┤
│     ClaudeConversationViewModel     │  ← Business logic
│     MyMacSessionManager             │
│     ScheduledJobManager             │
├─────────────────────────────────────┤
│  SSHSessionService (Citadel/NIO)    │  ← Services
│  TmuxChatroomManager               │
│  AnthropicClient                    │
├─────────────────────────────────────┤
│  SwiftTerm │ Citadel │ SwiftNIO     │  ← Infrastructure
│  SwiftData │ Keychain │ BGTask      │
└─────────────────────────────────────┘
```

A few decisions worth mentioning:

- **Citadel/SwiftNIO for SSH** — gives us async/await, but two concurrent exec channels on one connection = NIO deadlock. The entire polling architecture is designed around this constraint.
- **CLI mode by default** — CatClaw runs `claude` (or `codex`, etc.) as a CLI process on your Mac. The AI gets the same filesystem access you do. No sandbox.
- **Tmux for everything** — background jobs run in tmux sessions. They survive SSH disconnects, app crashes, phone restarts. The Mac doesn't care if your phone is alive.
- **Adaptive polling** — 3s when output is flowing → 8s idle → 15s very idle. Saves battery without missing completions.

---

## 🐛 The bugs that nearly broke me

Every iOS developer has war stories. Here are mine:

**The Zombie Connection.** Your iPhone goes to sleep. iOS silently kills the TCP connection. NIO doesn't notice. The SSH client thinks everything is fine. Your next command hangs. Forever. *The fix:* a 12-second timeout on every exec, plus a pre-flight `echo 1` health check before every AI interaction. If the echo doesn't come back, we assume the connection is dead and auto-reconnect.

**The Deadlock.** Two SSH exec channels on one Citadel connection = instant freeze. No error. No timeout. Just... nothing. I spent three days on this one before finding a single sentence in the NIO docs that explained it. *The fix:* never, ever call `tmux has-session` while `tail -f` is running. Re-architect the entire polling system.

**The 7KB Bug.** Claude CLI silently returns empty output when stdin exceeds ~7KB. No error message. Just... silence. *The fix:* write every message to a temp file on the Mac and `cat` it into the command. Never pipe directly.

**The WiFi Race.** iPhone fires `willEnterForeground` before WiFi reconnects (1–3 second gap). App probes SSH → fails → declares connection dead → user sees "Disconnected" banner for no reason. *The fix:* exponential backoff — 5s, 15s, 30s retries. Most of the time, the second probe succeeds.

If you're building anything with NIO + iOS lifecycle + SSH, feel free to DM me. I have scars.

---

## 🚀 What's next

- **Instant notifications** — right now the app checks for completed jobs every ~10 minutes via iOS Background App Refresh. We're building a Mac-side file watcher that pushes completion events through WebSocket for sub-second notification delivery.
- **Shared chatrooms** — host a room, share a 6-digit code, let teammates watch your AI session in real-time. Already built, polishing.
- **watchOS** — dictate a job from your Apple Watch, get the result on your wrist. Because why not.

---

## 🐱 Try it

CatClaw Terminal is free on the App Store in 80+ countries:

### **[Download CatClaw Terminal](https://apps.apple.com/us/app/clawterminal/id6759690902)**

📖 [Documentation](https://github.com/LiqunChen0606/clawterminal-docs)

If you're a developer who's ever needed to fix something from your phone — or if you just want to watch six AI agents work while your cat sits on your lap — give it a try.

Feedback, bugs, feature requests: [open an issue](https://github.com/LiqunChen0606/clawterminal-docs/issues)

---

*Built with Swift, SwiftUI, TCA, SwiftTerm, Citadel, and a mass amount of mass-produced instant coffee. The cat mascot (a Persian Cloud cat, specifically) was not harmed during development, though she did sit on the keyboard several times during critical debugging sessions.*
