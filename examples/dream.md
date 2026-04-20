# /dream — Autonomous Preference Learning

> The more you use CatClaw, the smarter it gets. Dream Mode silently analyzes your conversation patterns and builds a living profile of how you work — so Claude adapts to you, not the other way around.

## Quick Start

1. Enable: **Settings → AI Intelligence → Dream Mode**
2. Chat naturally for a while
3. Run `/dream now` to trigger your first dream cycle
4. View your profile: `/dream show`

---

## Commands

```
/dream              — view your learned profile
/dream show         — same as above
/dream now          — run a dream cycle immediately
/dream reset        — clear all learned preferences
```

---

## What Gets Learned

| Signal | What CatClaw Learns | Example |
|--------|---------------------|---------|
| Your questions | Skill levels per domain | "Asks basic Swift questions but gives advanced architecture guidance" |
| Your corrections | What to avoid | "User said 'no, simpler' → prefers concise answers" |
| Files you mention | Key project files | "ViewModel.swift mentioned in 60% of messages → auto-prioritize" |
| Commands you use | Workflow patterns | "Uses /health before every PR → cares about code quality" |
| Message length | Communication style | "Short messages → prefers direct, concise responses" |
| Topics that repeat | Current focus areas | "Auth module discussed 8 times → it's the active focus" |

---

## How It Works

### Active Mode (SSH connected)

Every 20 messages, CatClaw runs a quick Haiku analysis (~$0.01, ~3s) of your recent conversation. The analysis produces a structured profile that gets injected into every future message as a `<dream_profile>` block.

### Background Mode (app closed)

At approximately 2 AM daily, iOS wakes the app briefly via `BGProcessingTask` to run a local heuristic analysis — no SSH or internet connection needed. Analyzes your persisted message history for patterns like file mention frequency, command usage, and correction phrases.

### The Profile Compounds

- **Day 1:** "User is working on an iOS app"
- **Day 5:** "User prefers functional patterns, hates singletons, always wants error handling"
- **Day 14:** "User is an advanced Swift developer but new to TCA. Explain TCA with UIKit analogies."
- **Day 30:** Claude feels like a teammate who has been on the project for a month

---

## Example: Enabling Dream Mode and Running First Cycle

```
Settings → AI Intelligence → Dream Mode → ON
```

After a few conversations:

```
/dream now
```

```
Running dream cycle...
Analyzed 47 messages across 3 sessions.

Profile updated:
  Project: iOS SSH terminal app using SwiftUI + TCA
  Skills: swift: advanced, testing: intermediate, devops: beginner
  Style: prefers concise answers with code examples
  Focus: authentication, SSH reliability, background jobs
  Key files: ClaudeConversationViewModel.swift, SSHSessionService.swift

Done. Profile will be injected automatically from now on.
```

---

## Example: Viewing Your Profile

```
/dream show
```

```
Dream Profile (learned over 12 cycles)

Project: iOS SSH terminal app using SwiftUI + TCA
Goals: refactor auth module, improve test coverage
Focus: authentication, SSH, terminal, SwiftUI
Skills: swift: advanced, testing: intermediate, devops: beginner
Style: prefers concise answers with code examples
Communication: moderate detail
Avoid: don't suggest singleton pattern; skip boilerplate explanations
Key files: ClaudeConversationViewModel.swift, SSHSessionService.swift, ClaudeView.swift

Insights:
- User frequently switches between architecture and implementation tasks
- Prefers seeing working code before explanations
- Values security and error handling highly
- Usually works on backend before frontend
- Tends to ask "why" before accepting suggestions

Last dream: 2 hours ago
Next scheduled: tonight at 2 AM
```

---

## Example: Resetting the Profile

Switched projects? Profile drifted off target?

```
/dream reset
```

```
Dream profile cleared. CatClaw will start learning from scratch.
Run /dream now anytime to rebuild from your current message history.
```

---

## Combining Dream Mode with Other Commands

### Dream + `/personality`

Dream Mode sets the baseline — your actual skill level, coding style, and active focus. `/personality` layers a role on top. Together they create a highly personalized assistant.

```
/personality architect
```

With Dream Mode active, Claude now knows you are an advanced Swift developer focused on auth + SSH and responding in the architect persona — systems thinking, scalability concerns, and trade-off analysis baked in.

### Dream + `/effort`

```
/effort high
```

Dream Mode already knows you prefer concise answers, so even in high-effort mode Claude will be thorough but not verbose — the right balance for your style.

### Dream + `/pin`

Pin your key project files and let Dream Mode identify the rest automatically:

```
/pin src/ClaudeConversationViewModel.swift
```

Dream Mode will surface additional frequently-mentioned files in the profile, extending your pinned context with learned context.

---

## Privacy

- All analysis runs locally on your device or your Mac (via SSH)
- Dream profile stored in `chatrooms.json` (local, not synced to any server)
- No data sent beyond normal Claude API usage (Haiku calls for active mode analysis)
- Full control: view, edit, reset, or disable at any time
- Dream Mode is **per-chatroom** — different rooms build independent profiles

---

## Tips

- Run `/dream now` after your most productive sessions to capture fresh insights
- Use `/dream show` before starting a new conversation branch to review what Claude already knows about you
- Dream Mode works per-chatroom — a dedicated project room builds a more focused profile than a general-purpose room
- If `/dream now` returns "not enough messages yet," send at least 10–15 messages first
- The background 2 AM cycle requires the app to have been opened at least once that day
