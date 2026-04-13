# /handoff — Session Handoff Examples

Seamlessly move a Claude conversation between your phone and your Mac — or from your Mac back to your phone — without losing context, project directory, or conversation history.

---

## Hand Off from Phone to Mac

```
/handoff mac
```

Exports the current phone chatroom context (project directory, Claude session ID, last messages) and opens a new tmux window on your Mac. The Claude session uses `--resume` so the full conversation history is intact. Open Terminal.app, attach the tmux session, and continue with a full keyboard.

---

## Pick Up a Mac Session on Your Phone

```
/handoff
```

ClawTerminal SSHs to your Mac and lists all active tmux sessions running Claude:

```
Active sessions on your Mac:

  1.  myapp — ~/Projects/myapp
      Running for 2h 14m
      Last output: "I've updated the auth module, now working on tests..."

  2.  docs-rewrite — ~/Projects/docs
      Running for 47m
      Last output: "Generated 12 of 20 API reference pages..."

  3.  quick-fix — ~/Projects/myapp
      Running for 8m
      Last output: "Found the null pointer issue in UserService.kt line 94"
```

Tap any session to attach it to the current chatroom on your phone.

---

## Commute-to-Desk Handoff

Start a job on your phone before leaving the office, continue at your desk with full terminal:

```
# On your phone, leaving the office:
/submit --ckpt Refactor the auth module to use async/await, update all call sites

# On the train — monitor in Jobs tab, no action needed

# Arriving at your desk:
/handoff mac
# → A tmux window opens on your Mac with the full context
# → Continue in your editor with full keyboard and screen space
```

---

## Desk-to-Commute Handoff

Start work at your desk, continue on your phone:

```
# You're deep in a session on your Mac, time to leave:
# (From your phone — already connected to your Mac via SSH)

/handoff
# → Lists all active Mac sessions
# → Tap your current work session
# → Continue on your phone during your commute
```

---

## Real-World Examples

### Code review on the go

```
# Started a detailed review session on Mac:
/pr review 42
# Now leaving for lunch, want to keep reading:

/handoff
# Tap the "pr-review" session — continue reading Claude's analysis on your phone
```

### Multiple Mac sessions in flight

```
# You have 3 background jobs running on your Mac from earlier:
/handoff
# ClawTerminal shows all active sessions with last-output preview
# Tap the one you want — instantly attached
```

### Hand off after long background job completes

```
# Running a big refactor overnight:
/submit --ckpt Full auth module refactor — async/await, error handling, tests

# Next morning on your commute, check the Jobs tab — job is done
# Back at desk: pick up the context in a full terminal
/handoff mac
```

---

## Notes

- Both directions use Claude's `--resume <sessionID>` flag — the session state lives on your Mac, ClawTerminal just points at it from either device
- No additional setup needed beyond an active SSH connection
- The phone session's project directory, model selection, and enabled skills all carry over
- The Mac tmux session name follows the pattern `claw_handoff_<roomID>` and is visible in `tmux ls`
