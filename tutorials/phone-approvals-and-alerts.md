# Phone Approvals & Alerts — Tutorial

> You started Claude Code in Terminal at your desk and walked away. Now your iPhone tells you when that session finishes or needs you, and if Claude wants to run something risky, like `rm -rf` or a force push, you can Allow or Deny it right from your Lock Screen. It works for every Claude Code session on your Mac, not just the ones ClawTerminal started. Notifications are private by default and stay quiet while you're at the Mac.

---

## Part 1: What You Get

| Feature | What happens |
|---------|--------------|
| **Alerts for all Mac sessions** | A push when any Claude Code session on your Mac finishes (✅) or is waiting for you (⏳), including sessions you started yourself in Terminal. Tap it to open **Mac Sessions** on that session |
| **Lock-Screen approvals** | When a session wants to run a risky shell command, it waits for your **Allow** or **Deny** (Face ID required). Everything else runs with no delay |

Both use Claude Code's own hooks feature on your Mac. Nothing is installed until you say so.

---

## Part 2: Before You Start — A Push Channel

The alerts reach your iPhone through the push channel you already use for job notifications. Set one up first if you haven't:

- **`/notifier ntfy`**: no account needed (see [Smart Notifications](smart-notifications.md))
- **`/notifier telegram`**: your own Telegram bot (see [Telegram Notifications](telegram-notifications.md))
- **`/notifier setup`**: Apple Push, which needs an Apple Developer account

With Apple Push, the **Allow** and **Deny** buttons appear right on the notification. With Telegram or ntfy, tapping the notification opens ClawTerminal on the waiting request, where you tap Allow or Deny.

---

## Part 3: Installing

1. Connect to your Mac and type **`/notifier hooks install`** in any chatroom (or open **Settings → Mac Session Alerts → Install Hooks…**)
2. Read the preview. It lists exactly what changes on your Mac before anything happens
3. Tap **Install Hooks**

What the install does:

- Backs up `~/.claude/settings.json`, then adds ClawTerminal's hook entries. Each entry is tagged, so your own hooks and settings stay exactly as they are
- Writes a small hook script to `~/.clawterminal/`, along with a default list of risky command patterns. Add your own patterns to `~/.clawterminal/risky_patterns.txt`, which is never overwritten

Claude Code sessions pick up the hooks when they start, so restart any session that was already running.

Other commands:

```
/notifier hooks          # status and options
/notifier hooks status   # hooks, push channels, and pending approvals
/notifier hooks uninstall
```

Uninstalling also makes a backup first and removes only ClawTerminal's entries. Your own `risky_patterns.txt` is kept.

---

## Part 4: How Approvals Behave

- **Only risky commands wait.** Shell commands that match a risky pattern wait for your phone: `rm -rf`, `git push --force`, `git reset --hard`, `git branch -D`, `sudo`, `curl | sh`, `chmod -R`, `rsync --delete`, `kubectl delete`, `terraform destroy`, `DROP TABLE` and similar. Everything else runs immediately
- **No answer in time.** The default timeout is 60 seconds (adjustable from 30 seconds to 5 minutes). If you don't answer, a Terminal session falls back to its normal on-screen prompt, and an unattended session (one running with permission prompts bypassed) is denied
- **Deny always holds.** A Deny from your phone stops the command even in a session that bypasses permission prompts
- **At the Mac? No buzz.** While you're using the Mac (keyboard or mouse in the last minute), sessions just show their normal prompt and finished/needs-you pushes are skipped
- **Plan mode is never gated**
- **Chatroom turns** you run from ClawTerminal with **Autonomy = Full** aren't gated

If a request was already answered at your Mac or timed out, the app tells you so when you open it.

---

## Part 5: Privacy

- **Private by default.** Notifications say only which session finished or wants approval, for example "🛡️ my-project wants to run a risky command". They don't include the command or message text
- **Opt in to details.** Turn on **Settings → Mac Session Alerts → Include command/message text in notifications** to see the command itself in the push
- **Secrets are always masked.** Tokens, passwords, and Authorization headers are hidden even when text is included
- The event log on your Mac (`~/.clawterminal/events.jsonl`) is readable only by your user account

---

## Part 6: Settings

**Settings → Mac Session Alerts** (connect to your Mac first). These settings are stored on the Mac and take effect immediately:

| Setting | What it does |
|---------|--------------|
| **Notify Me for All Mac Sessions** | Finished / needs-you pushes for every session |
| **Approve Risky Actions from My Phone** | Lock-Screen Allow / Deny for risky commands |
| **Include command/message text in notifications** | Off by default. Adds the command or message to the push |
| **Approval Timeout** | 30 seconds to 5 minutes; default 60 seconds |

Pushes are also skipped while you're viewing ClawTerminal itself (**Quiet While App Is Open**).

---

## Troubleshooting

| Symptom | Try |
|---------|-----|
| No pushes at all | Run `/notifier hooks status` and check that a push channel is set up. Without one, approvals fall back to the normal prompt right away |
| A session I was already running doesn't send alerts | Sessions load hooks when they start. Restart that session |
| No pushes while I'm at the Mac | That's on purpose. Pushes are skipped while the Mac is in use |
| Allow / Deny didn't reach the Mac | Reconnect to your Mac, then tap the request again in the approvals list |
| A command I consider risky ran without asking | Add a pattern for it to `~/.clawterminal/risky_patterns.txt` |
