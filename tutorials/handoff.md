# Tutorial: Session Handoff with `/handoff`

> It is 5:45 PM. You have been deep in a refactoring session at your desk for two hours. Time to leave. You do not want to lose context -- the conversation history, the project directory, the running job. You pick up your phone, type `/handoff`, tap your active session, and walk out the door. On the train, you continue the conversation exactly where you left off. That is handoff.

---

## How It Works Under the Hood

ClawTerminal uses Claude's `--resume <sessionID>` flag for session continuity. The session state lives on your Mac's filesystem (in `~/.claude/`), not on your phone. ClawTerminal is always "pointing" at a session ID — when you hand off, you're just redirecting that pointer from one device to another.

This means:
- No data is transferred between devices
- Conversation history is always intact
- The active project directory is preserved
- `/handoff` works even if you were in the middle of a message

---

## Prerequisites

`/handoff` requires nothing beyond an active SSH connection to your Mac. No additional apps, accounts, or setup needed.

---

## Part 1: Phone to Mac — `/handoff mac`

### When to use this

- You started a task on your phone and want to continue with a full keyboard
- A job is running and you want to monitor it in a proper terminal
- You want to attach your editor to the same project Claude is working on

### Step-by-step walkthrough

**1. Start a session on your phone**

Open a chatroom on your Mac's Claude tab and work normally. Maybe you've been in a conversation for an hour:

```
You: Let's refactor the authentication module. Start with UserService.
Claude: [Analyzes UserService.swift...]
You: Good. Now update the call sites.
Claude: [Updates 12 files...]
You: Now write unit tests for the new async methods.
Claude: [Working on tests...]
```

**2. Run the handoff**

```
/handoff mac
```

ClawTerminal:
- Reads the current chatroom's session ID and project directory
- Opens a new tmux window on your Mac named `claw_handoff_<roomID>`
- Pre-populates it with the correct `cd` and `claude --resume <sessionID>` command
- The terminal is ready to attach immediately

**3. Open Terminal on your Mac**

Run:

```bash
tmux attach -t claw_handoff_<roomID>
```

Or just:

```bash
tmux ls
# Shows all sessions including your handoff session
tmux attach -t <session-name>
```

The Claude CLI resumes with full conversation history. You pick up exactly where you left off.

**4. Continue on Mac**

Now you have:
- Full keyboard
- Editor open alongside
- Multiple terminal panes
- The complete conversation history in Claude's session

---

## Part 2: Mac to Phone — `/handoff`

### When to use this

- You were working at your desk and need to leave
- You want to check on a running session from your phone
- Multiple Claude sessions are running and you want to pick one up

### Step-by-step walkthrough

**1. Run the handoff picker from your phone**

In a chatroom on your phone:

```
/handoff
```

ClawTerminal SSHs to your Mac and discovers all active tmux sessions running Claude. A picker appears with each session listed:

```
Active sessions on your Mac:

  1.  myapp-auth — ~/Projects/myapp
      Running for 2h 14m
      Last output: "I've updated UserService.swift, now writing tests..."

  2.  docs-rewrite — ~/Projects/website
      Running for 47m
      Last output: "Generated the API reference for endpoints 1-12 of 20..."

  3.  quick-investigation — ~/Projects/myapp
      Running for 8m
      Last output: "Found the issue — it's in the retry backoff logic at line 94"
```

**2. Tap the session you want**

Tap any session to attach it. The chatroom on your phone inherits that session's Claude session ID, and subsequent messages continue the conversation seamlessly.

**3. Continue on your phone**

You can now message Claude as if you'd been in this conversation the whole time. The history is in Claude's session database — ClawTerminal just needs the session ID to access it.

---

## Part 3: Real-World Scenarios

### Commute-to-desk handoff

A classic use case — kick off a long task before your commute, continue at your desk:

```
# Leaving the office — on your phone:
/submit --ckpt Refactor the payment module to use async/await, update all call sites, add tests

# On the train:
# Monitor in the Jobs tab — no action needed
# (The job runs in the background regardless of whether the app is active)

# Arriving at your desk, job is done:
/handoff mac
# A tmux window opens with the session pre-configured
# Attach it: tmux attach -t claw_handoff_<id>
# Continue reviewing Claude's changes in your editor
```

### Desk-to-commute handoff

Going the other way — you're deep in something at your desk and need to leave:

```
# At your desk, you've been working for 2 hours:
# [lots of back-and-forth in the chatroom]

# Time to leave — pick up on your phone:
# (From your phone's chatroom)
/handoff
# Tap the session you were working on
# Continue the conversation during your commute
```

### Picking up a background job in progress

Background jobs (`/submit`) run independently of the chatroom. When a job completes, you can hand off to explore the results:

```
# Job completed notification arrives on your phone
# Tap "View" in the notification

# The Jobs tab shows the result summary
# To get a full interactive session:
/handoff
# The active Claude session for that chatroom is listed
# Tap it to pick up context and ask follow-up questions
```

### Multiple parallel sessions

If you've been running multiple sessions — maybe a `/team` orchestration with several waves:

```
/handoff
# Shows:
#   1. team-session-wave-1 — finished, last output: "Research complete..."
#   2. team-session-wave-2 — finished, last output: "Implementation done..."
#   3. team-session-synthesizer — running, last output: "Merging findings..."
# Tap the synthesizer to watch it live
```

---

## Part 4: Tips and Edge Cases

### After a phone-to-Mac handoff, does the phone session still work?

Yes. Tapping the phone chatroom and sending another message will re-attach the same Claude session. Both devices point at the same session on disk. The last one to send a message "wins" — they're not truly synchronized, so avoid sending from both at the same time.

### What if my Mac has no active Claude sessions?

`/handoff` (Mac-to-phone direction) will show an empty picker: "No active Claude sessions found on your Mac." This means no tmux sessions named `claw_*` are currently running. You can start a new session normally from your chatroom.

### What if the session ID on the phone is stale?

If the Mac-side session database has been cleared (e.g., you ran `claude --dangerously-clear-sessions`), the `--resume` will fail gracefully — Claude starts a new session and tells you the previous session wasn't found. The project directory is preserved, so you can continue normally.

### Can I hand off between different iPhones?

Technically yes — as long as both phones are connected to the same Mac via SSH. They both point at the session ID stored on the Mac. This is the same mechanism that makes the phone-to-phone handoff work in the shared chatroom feature (different feature — that uses a relay server, not `/handoff`).

### The tmux window didn't appear on my Mac

- Make sure your Mac's Terminal.app is running (not just the Mac itself)
- Check `tmux ls` in Terminal — the `claw_handoff_*` session may be there but Terminal didn't auto-focus it
- If tmux isn't installed: `brew install tmux`

---

## Part 5: `/handoff` vs. Shared Chatroom

These are two different features that serve different purposes:

| Feature | `/handoff` | Shared Chatroom |
|---------|-----------|-----------------|
| Purpose | Move between your own devices | Share a session with a teammate |
| Participants | Just you | You + one or more guests |
| Guest can type? | N/A | No — guests are read-only |
| Requires relay server? | No | Yes (optional relay setup) |
| Works offline? | Yes (LAN only) | Yes (LAN only, no relay) |
| Session state | Claude's `--resume` flag | Relay server broadcast |

Use `/handoff` to switch between your own devices. Use **Host / Join** in the toolbar for team collaboration.

---

## Summary

`/handoff mac` — phone to Mac in one command. Great for continuing mobile work at your desk.

`/handoff` — Mac to phone, or picking up any active session. Great for on-the-go continuation.

Both directions work seamlessly because the session state lives on your Mac, not on your phone. ClawTerminal just points at it from wherever you are.

See [examples/handoff.md](../examples/handoff.md) for copy-paste-ready examples.

---

## What's Next?

- **[Live Web Preview](preview.md)** -- After handing off to your phone, preview your web app's UI changes directly on the device you are holding.
- **[GitHub PR Workflow](pr-workflow.md)** -- Picked up a session on your phone? Create a PR from the train before you forget.
- **[Background Jobs](../examples/background-jobs.md)** -- Kick off long tasks with `/submit` before a handoff -- they run independently and you can pick up the results from either device.
