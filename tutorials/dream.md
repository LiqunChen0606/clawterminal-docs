# Dream Mode — Tutorial

> CatClaw gets smarter the more you use it. Dream Mode analyzes your conversation history while you sleep and builds a living profile of how you work — your skill levels, preferred style, active focus areas, and key project files. That profile is injected automatically into every future session, so Claude meets you where you are without you having to explain yourself again.

---

## Part 1: What Dream Mode Actually Does

Most AI tools start from zero every session. You describe your stack, explain your preferences, and re-establish context — every time. Dream Mode eliminates that loop.

CatClaw observes:

- **What you ask about** — infers skill level per domain (Swift: advanced, testing: intermediate, devops: beginner)
- **How you phrase things** — short, direct messages → you want concise answers; long paragraphs → you want thorough explanations
- **What you correct** — "no, simpler" or "don't do it that way" gets saved as a preference automatically
- **Files you mention repeatedly** — after a file appears in 30%+ of messages, it gets flagged as a key project file
- **Commands you use** — `/health` before every PR → code quality matters to you; `/race` frequently → you like comparing approaches
- **Topics that recur** — the auth module appearing in 8 conversations in a row signals it's your current focus

The resulting profile looks like this:

```
Project: iOS SSH terminal app using SwiftUI + TCA
Skills: swift: advanced, testing: intermediate, devops: beginner
Style: prefers concise answers with code examples
Communication: moderate detail
Focus: authentication, SSH reliability, background jobs
Key files: ClaudeConversationViewModel.swift, SSHSessionService.swift
Avoid: don't suggest singleton pattern; skip boilerplate explanations

Insights:
- Prefers seeing working code before explanations
- Values security and error handling highly
- Tends to ask "why" before accepting suggestions
```

This is injected as a `<dream_profile>` block in the preamble of every message — automatically, without you doing anything after the initial setup.

---

## Part 2: Two Analysis Paths

Dream Mode has two ways to update your profile.

### Active Mode — Every 20 Messages

While you are chatting with an SSH connection active, CatClaw waits until 20 new messages have accumulated. Then it runs a Haiku SSH call (approximately $0.01 and 3 seconds) that reads your recent conversation and produces an updated structured profile. You will see a brief "Updating dream profile..." status chip and then the profile is live.

This is the high-quality path. Haiku reads semantic meaning — it picks up on things like "the user keeps asking about error handling but never about happy paths, which suggests defensive coding is a priority for them."

### Background Mode — 2 AM Daily

When you are not using the app, iOS wakes it briefly at approximately 2 AM using `BGProcessingTask`. CatClaw runs a local heuristic analysis with no network required:

- Counts file mention frequency across all persisted messages
- Counts command usage distribution
- Detects correction phrases ("no, use X instead", "stop doing Y", "I prefer Z")
- Measures average message length (proxy for preferred response length)
- Extracts the top 5 most-discussed topics

This produces a lighter profile update — good enough to refresh focus areas and file priorities overnight. For the active app to hand off to the background task, it saves intermediate signal data to `chatrooms.json` before backgrounding.

---

## Part 3: Your First Dream Cycle — Step by Step

### Step 1: Enable Dream Mode

Go to **Settings → AI Intelligence → Dream Mode** and toggle it on. Nothing else is required — Dream Mode runs automatically from this point.

### Step 2: Use CatClaw Normally

Have a real conversation. Ask Claude about your project. Make some corrections. Run a few slash commands. The more authentic the usage, the better the profile.

If you want to jump-start the learning process, you can send a self-description:

```
For context: I'm a senior iOS developer working on a SwiftUI + TCA app. I prefer concise code examples over long explanations. I'm currently focused on refactoring the auth module and improving test coverage.
```

Dream Mode will pick up the explicit signals here while also observing your actual behavior over time.

### Step 3: Trigger Your First Cycle

After 10–15 messages, run:

```
/dream now
```

You will see:

```
Running dream cycle...
Analyzed 23 messages across 2 sessions.

Profile updated:
  Project: iOS app (SwiftUI + TCA)
  Skills: swift: advanced, testing: intermediate
  Style: prefers concise answers with code examples
  Focus: authentication, test coverage
  Key files: AuthViewModel.swift, LoginView.swift

Done. Profile will be injected automatically from now on.
```

### Step 4: Check the Profile

```
/dream show
```

Read through the profile. If anything is wrong — you do not actually prefer singleton pattern avoidance, or the wrong files are listed — you can run `/dream reset` and let it re-learn. Or just correct Claude in conversation and the correction gets captured automatically.

### Step 5: Watch It Improve

Come back the next day. After the 2 AM background cycle, run `/dream show` again. The profile will have updated focus areas and possibly added new key files based on overnight analysis.

---

## Part 4: How the Profile Evolves

### Week 1

The profile is sparse. CatClaw knows your stack, your rough skill distribution, and maybe your preferred response length. Claude still sometimes gives you answers that are slightly too basic or slightly too detailed.

```
Project: iOS app
Skills: swift: intermediate-advanced
Style: prefers code examples
Focus: (none identified yet)
```

### Week 2

The profile fills in. File patterns emerge. Recurring topics lock in as focus areas. Corrections accumulate. Claude starts to feel noticeably personalized.

```
Project: iOS SSH terminal app using SwiftUI + TCA
Skills: swift: advanced, testing: intermediate, ssh: beginner
Style: prefers concise answers with code examples
Focus: authentication, SSH reliability
Key files: ClaudeConversationViewModel.swift, SSHSessionService.swift
Avoid: don't suggest singleton pattern
```

### Week 4

The profile has converged. It captures not just what you work on but how you think. Claude now pre-empts your "why" questions, skips explanations of things you clearly know, and suggests error handling before you ask for it.

```
Project: iOS SSH terminal app using SwiftUI + TCA
Goals: refactor auth module, improve test coverage
Focus: authentication, SSH, terminal, SwiftUI
Skills: swift: advanced, testing: intermediate, devops: beginner
Style: prefers concise answers with code examples; sees working code first
Communication: moderate detail; use UIKit analogies for TCA concepts
Avoid: singleton pattern; boilerplate explanations; excessive caveats
Key files: ClaudeConversationViewModel.swift, SSHSessionService.swift, ClaudeView.swift, AppSettings.swift

Insights:
- Switches frequently between architecture and implementation tasks
- Values security and error handling; always asks about edge cases
- Prefers backend-first, then frontend
- Tends to ask "why" before accepting a suggestion
- Active auth refactor is the current sprint priority
```

---

## Part 5: Combining Dream Mode with Other Commands

Dream Mode is a foundation layer. Other commands layer on top of it.

### Dream + `/personality`

Dream Mode captures who you are — your actual skills, style, and focus. `/personality` sets a role for how Claude responds.

```
/personality architect
```

Now Claude knows you are an advanced Swift developer focused on the auth module (from Dream Mode) AND is responding in architect persona — systems thinking, scalability concerns, interface design, trade-off analysis. Two layers of personalization with a single command.

Other useful combinations:

| Scenario | Dream provides | `/personality` adds |
|----------|---------------|---------------------|
| Design session | Your project context + skill level | `architect` — systems thinking, scalability |
| Code review | Your quality standards + focus files | `reviewer` — meticulous, correctness-first |
| Prototyping | Your stack + preferred patterns | `hacker` — fastest path, iterate later |
| Learning TCA | Your Swift level + existing UIKit patterns | `mentor` — analogies, patient explanations |

### Dream + `/effort`

Dream Mode already knows your preferred level of detail from observed message patterns. `/effort` lets you override it for a specific conversation.

```
/effort high
```

With Dream Mode knowing you prefer concise answers, even high-effort mode will be thorough but not verbose — more depth without bloat.

```
/effort low
```

Combined with Dream Mode knowing you are an advanced developer, low-effort mode gives you single-line answers to factual questions — no hedging, no caveats.

### Dream + `/soul`

If your project has a `/soul` persona defined, Dream Mode and the soul work in parallel layers:

- **Soul:** defines what kind of assistant this chatroom should be (e.g. "You are a senior engineer at a fintech startup. Security is never optional.")
- **Dream Mode:** defines who you are as the user (e.g. "This developer is an advanced Swift engineer focused on the auth module who prefers concise answers")

Together, they create a fully personalized, role-appropriate assistant for each chatroom.

### Dream + `/pin`

You pick the files you know matter. Dream Mode surfaces the files it observed you mentioning frequently. The result: your pinned context plus learned context, with no overlap and no gaps.

```
/pin src/ClaudeConversationViewModel.swift
/dream show
# → Key files: SSHSessionService.swift (learned)
```

Now every message includes both your explicitly pinned file and the frequently-mentioned file Dream Mode identified — without you having to remember to pin the second one.

---

## Part 6: The 2 AM Background Analysis — How It Works

When iOS triggers the `BGProcessingTask` at approximately 2 AM, CatClaw runs the following steps locally, with no network connection:

1. **Load message history** from `chatrooms.json` (Documents folder, local only)
2. **Count file mentions** — any path-like string appearing in messages; rank by frequency
3. **Count command usage** — `/health`, `/race`, `/submit`, etc.; frequency distribution reveals workflow preferences
4. **Scan correction phrases** — detects "no, use X instead", "stop doing Y", "I prefer Z", "don't do that" and records the implied preference
5. **Measure message length distribution** — median length of user messages is a reliable proxy for preferred response detail level
6. **Extract top topics** — term frequency across the last 7 days of messages identifies active focus areas
7. **Update `dreamProfile`** in `chatrooms.json` — writes the updated profile back; the change is live the next time the app opens

The background task takes 3–8 seconds and uses no API calls. It respects iOS power budget constraints and will defer if the device is too warm or on battery below 20%.

---

## Part 7: When to Reset vs. Let It Learn

### Let it learn when:

- The profile is mostly right but has a few outdated entries — corrections accumulate automatically over time
- You switch between different areas of the same project — the focus section updates with each cycle
- Claude is slightly off but trending in the right direction — wait for another cycle

### Reset when:

- You switch to a completely different project in the same chatroom
- The profile has locked onto a wrong assumption that keeps surfacing
- You want to start a new sprint with a clean context slate

```
/dream reset
```

After resetting, run `/dream now` immediately if you have 10+ messages that represent your current work. Otherwise, the next 2 AM cycle will rebuild from scratch.

---

## Part 8: Privacy in Depth

Dream Mode is designed around local-first principles.

| What happens | Where |
|-------------|-------|
| Message history storage | `chatrooms.json` in your iOS Documents folder — local, not iCloud |
| Active cycle analysis (Haiku) | Messages sent over your SSH connection to your Mac → Haiku API → result returns | 
| Background cycle analysis | 100% local, zero network, zero API calls |
| Dream profile storage | `chatrooms.json` — local, not synced to any server |
| What gets sent to Claude API | Your messages + the dream profile preamble, same as normal usage |

The dream profile itself is stored in plain JSON inside `chatrooms.json`. You can read it, edit it, or delete it at any time — no opaque data formats, no hidden uploads.

Dream Mode is **per-chatroom**. A chatroom dedicated to your iOS project builds a different profile than a general-purpose chatroom. This is intentional — different contexts deserve different learned preferences.

---

## Part 9: What's Next

Dream Mode in this release establishes the foundation: passive observation, overnight analysis, and profile injection. Future enhancements planned:

- **Explicit feedback integration** — thumbs-up/down on individual profile entries to refine learning faster
- **Cross-chatroom profile merge** — opt-in consolidation of profiles across multiple project rooms
- **Profile export/import** — share your learned profile with a new device or a new room
- **Agent awareness** — Dream profile injected into agent sub-tasks as well as direct messages

---

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/dream` or `/dream show` | View your full learned profile |
| `/dream now` | Run a Haiku analysis cycle immediately |
| `/dream reset` | Clear the profile and start fresh |
| Settings → AI Intelligence → Dream Mode | Enable or disable Dream Mode globally |

**Minimum messages for first cycle:** 10–15 messages in a chatroom.

**Active cycle trigger:** Every 20 new messages (SSH required).

**Background cycle timing:** Approximately 2 AM daily via `BGProcessingTask`.

**Profile storage:** Local `chatrooms.json` — never sent to any external server.
