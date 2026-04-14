# UX & Intelligence Features

## Smart Follow-Up Suggestions

After Claude responds, 3 tappable chips appear above the input bar. Tap any chip to send instantly — no typing needed.

Chips adapt based on the content of the response:

**Error context**
```
"How do I fix this?"
"Show me the relevant code"
"What caused this error?"
```

**Code response**
```
"Explain this code"
"Are there any bugs?"
"How can this be improved?"
```

**Test context**
```
"Run the tests"
"Add more edge cases"
"Show test coverage"
```

**General**
```
"Tell me more"
"Show me an example"
"What should I do next?"
```

**Trying it out:**

Ask Claude to explain a function, then tap "Are there any bugs?" — the follow-up sends instantly with the full context already loaded.

Ask Claude to help with a failing test, then tap "Add more edge cases" to extend coverage without re-explaining the setup.

---

## Gesture Shortcuts

### Long-Press Send — Submit as Background Job

Hold the Send button for 0.5 seconds instead of tapping it:

```
Tap Send       → sends as a chatroom turn (streaming response in chat)
Long-press Send → sends as a background job (result appears in Jobs tab)
```

**When to use long-press:**
- Long refactors you don't need to watch in real time
- Batch operations on many files
- Anything that will take more than 2-3 minutes

**Example:**
```
Type: "Refactor all database queries to use prepared statements"
Long-press Send → job dispatched, conversation stays clean
```

Check the Jobs tab for the result when you're ready.

### Swipe Down — Compact the Conversation

Swipe down on the input bar to trigger `/compact`:

```
Swipe down on input bar → conversation summarized, context tokens freed
```

This is identical to typing `/compact` but faster. Use it when:
- A long session starts producing slower or shorter responses (context getting full)
- You are about to switch to a completely different topic
- You want to keep the session alive but reset the working context

---

## Split-Screen Mini Terminal

**Opening the split-screen:**

Tap the split-rectangle icon in the chatroom banner bar (only visible when SSH is connected).

A mini SSH terminal slides up from the bottom of the screen.

**Resizing:**

Drag the horizontal handle bar up or down:
- Minimum height: 100pt
- Maximum height: 400pt

**Closing:**

Tap the split-rectangle icon again, or drag the handle all the way down.

**Example workflows:**

Check git status without leaving the chat:
```bash
git status
git diff --stat
```

Run tests and watch the output while keeping Claude's last response visible:
```bash
npm test -- --watch
```

Verify a file Claude just created:
```bash
ls -la src/components/
cat src/components/NewButton.tsx
```

Ask Claude to implement a feature, then immediately check the result in the mini terminal — without switching tabs once.

---

## Conversation Branching

**Creating a branch:**

Long-press any message in the conversation → tap **"Branch from here"**.

The conversation forks at that point. A purple branch picker appears in the banner showing:
- **Main** (the original thread)
- **Branch 1**, **Branch 2**, etc.

**Switching between branches:**

Tap the purple branch picker in the banner and select a branch. The conversation view updates immediately.

**Clearing branches:**

Tap the purple branch picker → **"Clear branches"**. This removes all forks and returns to the main thread.

**Example workflow — comparing two API designs:**

1. Ask Claude: "Design an authentication API for our app"
2. Read the response
3. Long-press the response → "Branch from here"
4. In Branch 1: "Implement this using REST with JWT cookies"
5. Switch back to Main → "Branch from here" again
6. In Branch 2: "Implement this using GraphQL with session tokens"
7. Switch between Branch 1 and Branch 2 to compare both implementations
8. Pick the winner and continue on that branch

**Example workflow — safe refactoring:**

1. Ask Claude to explain the current architecture
2. Long-press the response → "Branch from here"
3. Branch 1: "Refactor this to use dependency injection"
4. If the refactor goes sideways, switch back to Main and branch again with a different approach
5. No need to start a new chatroom — just branch

---

## JSON Table Cards

When Claude returns a JSON array inside a code block, CatClaw auto-renders it as a formatted table.

**What triggers the table:**
- A code block tagged ` ```json `
- The top-level value is a JSON array `[...]`
- Each element is an object `{...}` with the same keys

**Table layout:**
- Column headers from JSON keys (camelCase split into words)
- Scrollable rows with alternating row shading
- Up to 20 rows shown; a "N more rows" footer appears for larger arrays
- Regular markdown text above the code block still renders normally

**Trying it:**

```
Give me a JSON array of the 5 most popular programming languages
with fields: name, paradigm, typingSystem, and yearCreated
```

```
Return a JSON array comparing REST vs GraphQL vs gRPC with fields:
protocol, latency, schemaRequired, streamingSupport, bestFor
```

```
List the top 10 npm packages by weekly downloads as a JSON array
with fields: name, weeklyDownloads, license, and description
```

---

## Auto-Context from Terminal

When a command fails in the Terminal tab, a floating "Ask Claude" pill appears at the bottom of the terminal screen.

Tapping "Ask Claude":
1. Switches to the chatroom (My Mac tab)
2. Pre-injects the last 30 lines of terminal output (ANSI stripped)
3. Focuses the input bar so you can type your question

**What triggers the pill:**
The pill appears when terminal output matches ~30 error patterns including:
- `command not found`
- `Permission denied`
- `SyntaxError:`, `TypeError:`, `ReferenceError:`
- `npm ERR!`, `pip install failed`
- `ENOENT`, `EACCES`, `ECONNREFUSED`
- `segmentation fault`, `core dumped`
- Build errors: `error:`, `fatal:`, `ld: error:`

**The pill auto-dismisses after 10 seconds** if you don't tap it.

**Example flow:**

1. Run `npm run build` in the Terminal tab
2. Build fails with `SyntaxError: Unexpected token` in a compiled file
3. The "Ask Claude" pill appears
4. Tap it → chatroom opens with the error output already injected
5. Type: "What's causing this and how do I fix it?"
6. Claude already sees the full error — no copy-paste needed

---

## Implicit Learning from Corrections

CatClaw detects correction phrases in your messages and auto-saves the underlying fact to memory — exactly like `/remember` but without any command.

**Phrases that trigger auto-save:**
- "No, I meant..."
- "That's wrong, actually..."
- "Instead, do..."
- "Actually, I prefer..."
- "Not X, use Y instead"

**What gets saved:**

When you type "No, I meant use camelCase for all variable names", CatClaw extracts the preference and saves it as a memory entry with category `userPreference`. On the next session, this preference is automatically injected into context.

**Example:**

```
Claude: Here's the function using snake_case variables...

You: No, I meant camelCase — we use camelCase for everything in this project
```

CatClaw saves: "User prefers camelCase variable naming in this project." Claude remembers it next session.

```
Claude: I've added error handling that throws an exception...

You: That's wrong, actually — we never throw here, we return Result types
```

CatClaw saves: "Project uses Result types for error handling, not exceptions." No `/remember` needed.

**Reviewing saved memories:**

Tap the Memories button (brain icon) in the chatroom toolbar to see all auto-saved corrections alongside manually saved memories.

---

## Cat Mascot

The pawprint icon in the chatroom banner bar reflects the current app state:

| State | Appearance |
|-------|-----------|
| Idle | Gray, muted |
| Streaming response | Orange, pulsing |
| Background job running | Cyan |
| Background job failed | Red |
| Background job completed | Bounces once |

The mascot gives you at-a-glance state without looking at the messages list or Jobs tab.

---

## Haptic Patterns

CatClaw uses distinct vibration patterns for different events — works even when the app is in the foreground:

| Event | Haptic |
|-------|--------|
| Job completed | Success tap (single firm click) |
| Job failed | Error buzz (double impact) |
| SSH disconnected | Warning pulse (soft + medium) |
| Message sent | Soft tap |

Haptics work independently of notification permissions — no setup required.
