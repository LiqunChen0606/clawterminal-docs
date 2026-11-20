# Faster Replies — Tutorial

> Waiting on a reply is easier when you can see what's happening, and shorter when Claude is already running. ClawTerminal now shows each reply's real progress, can keep Claude Code running between messages so replies start in about 2 seconds, and can show you exactly where the time went. `/netdiag` checks the whole path to your Mac when something feels slow.

---

## Part 1: Real Progress While You Wait

While a reply is on its way, the status line under the conversation shows what is actually happening on your Mac, based on what Claude Code reports. It doesn't guess:

| Stage | Meaning |
|-------|---------|
| **Connecting** | Reaching your Mac |
| **Sent** | Your message reached the Mac (with how long that took) |
| **Claude starting** | Claude Code is launching |
| **Ready** | Claude Code is up, with the model and version it's using |
| **Thinking** | The model is working on your message |
| **Running `<tool>`** | Claude is using a tool, for example `Running Bash npm test` |
| **Writing** | The answer is streaming in |

**"Slower than usual."** If a reply is stuck *before* Claude Code has started (usually a network or connection problem), a small note appears with **Retry on fresh connection**. It resends exactly that message on a new connection and keeps your draft as it is. The note doesn't appear once Claude has started, because by then the connection is working and a retry wouldn't help.

---

## Part 2: Keep Claude Running Between Messages (Opt-In)

Normally every message starts a fresh Claude Code process on your Mac, which takes a few seconds before the model even sees your message. With **Keep Claude running between messages**, each chatroom keeps one Claude Code process running, so your next message goes straight to it.

- **Speed:** replies start in about 2 seconds instead of 5–9
- **Turn it on:** **Settings → AI Intelligence → Keep Claude running between messages**
- **Requirements:** Claude Code 2.1.300 or newer on your Mac, and tmux chatrooms
- **Never twice:** every message carries its own ID, so a message is never run twice, even if the connection drops while it is being sent
- **Cleans up after itself:** the process shuts down after 15 minutes without messages. It also stops when you hand the session off, continue a Mac session, delete the chatroom, or turn the setting off
- **Safe fallback:** if anything looks unusual, the message goes through the normal launch instead

The trade-off: a running Claude Code process uses some memory and battery on your Mac while it waits.

---

## Part 3: Show Reply Timings (Opt-In)

Turn on **Settings → AI Intelligence → Show Reply Timings**, or long-press any Claude reply and choose **Show Reply Timings**. Each finished reply then gets a small caption like:

```
Mac 0.3s · start 3.9s · model 2.1s · total 6.6s
```

| Part | What it measures |
|------|------------------|
| **Mac** | Getting your message to the Mac |
| **start** | Starting Claude Code (near zero with Keep Claude running) |
| **model** | The model itself, until the reply finished |
| **total** | From tapping Send to the finished reply |

A **prep** part appears only when preparing the message took noticeable time, for example when you attached files or a web page. `/dashboard` shows typical (p50) and slow (p95) reply times across your recent turns.

---

## Part 4: `/netdiag` — Check the Path to Your Mac

Type `/netdiag` in any chatroom for a full connection check. It runs only when you ask, so it never slows down normal messages.

- **Route race:** every address ClawTerminal knows for your Mac (home network, Tailscale name, Tailscale IP), each with its round-trip time
- **SSH ping:** whether the current connection is actually alive
- **Tailscale on this iPhone:** whether it's on
- **Claude Code version** on your Mac
- **Recent replies:** a timing breakdown of your last few turns
- **Cold-start test (optional):** starts a fresh `claude -p` on your Mac with Haiku and reports where the time went. It sends one tiny prompt to Anthropic (about 1¢)

`/doctor` remains the quick setup check. Use `/netdiag` when you want numbers.

---

## Troubleshooting

| Symptom | Try |
|---------|-----|
| Replies still take 5+ seconds with Keep Claude running on | Check your Claude Code version (`/netdiag` shows it). Older versions fall back to the normal launch. The first message after 15 idle minutes starts a new process |
| "Slower than usual" appears often | Run `/netdiag` and look at the route race. If the home-network address fails away from home, see [Connect From Anywhere](connect-anywhere.md) |
| The "start" time is large | Try **Settings → AI Intelligence → Lean Launch**, or turn on Keep Claude running |
| The "model" time is large | That is the model thinking. Try `/effort low` for quick questions, or a faster model with `/model` |
| No timing caption | The caption only appears on Claude Code replies that finished. Check that Show Reply Timings is on |
