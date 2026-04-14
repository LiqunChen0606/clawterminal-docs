# Tutorial: UX & Intelligence Features

This tutorial covers CatClaw's UX and intelligence features introduced in v1.7.0. Each section explains when and why to use the feature, how to set it up, and how to combine features for maximum productivity.

---

## Smart Follow-Up Suggestions

### What it is

After every Claude response, 3 tappable chips appear above the input bar. Each chip is a pre-formed question you can send with one tap — no typing. The chips change based on what Claude said.

### When to use it

Smart suggestions are most useful when:
- You want to dig deeper but aren't sure what to ask
- You're on a small screen and typing is inconvenient
- You're in a fast debugging loop and don't want to re-state context

### How it works

CatClaw classifies the response into one of four categories:

**Error context** — response contains error output, stack traces, or failure messages
- "How do I fix this?"
- "Show me the relevant code"
- "What caused this error?"

**Code response** — response contains a code block
- "Explain this code"
- "Are there any bugs?"
- "How can this be improved?"

**Test context** — response is about testing, test files, or test results
- "Run the tests"
- "Add more edge cases"
- "Show test coverage"

**General** — anything else
- "Tell me more"
- "Show me an example"
- "What should I do next?"

### Step-by-step walkthrough

**Debugging with suggestions:**

1. Paste an error message and ask "What does this mean?"
2. Claude explains the error
3. The chip "How do I fix this?" appears — tap it
4. Claude provides a fix
5. The chip "Show me the relevant code" appears — tap it to see where the fix applies

**Code review loop:**

1. Ask Claude to write a function
2. Claude returns the implementation
3. Tap "Are there any bugs?" — Claude checks for edge cases
4. Tap "How can this be improved?" — Claude suggests optimizations

### Productivity tip

Combine smart suggestions with conversation branching: after Claude responds, branch from the message, then use the suggestion chips to explore two different follow-up paths without re-typing the setup.

---

## Gesture Shortcuts

### Long-Press Send

**What it is:** Hold the Send button for 0.5 seconds to submit as a background job instead of a chatroom turn.

**Visual cue:** The Send button darkens slightly after 0.3s to indicate the long-press is registering. Release at 0.5s to dispatch.

**When to use long-press vs regular tap:**

| Use regular tap when... | Use long-press when... |
|------------------------|------------------------|
| You want to watch the response stream in | The task will take more than 2-3 minutes |
| You need to ask a follow-up immediately after | You want to do something else while it runs |
| The task is short (explanation, snippet) | The task touches many files |
| You're in a debugging loop | You're running a refactor, test suite, or build |

**Step-by-step:**

1. Type your task in the input bar (e.g. "Add JSDoc comments to all exported functions in src/")
2. Press and hold the Send button
3. Release at 0.5 seconds — the input bar clears and a "Job dispatched" confirmation appears
4. Open the Jobs tab to monitor progress
5. The job result is posted back to the chatroom when complete

**Tip:** If you accidentally long-press and don't want a background job, release before 0.5 seconds. The button returns to normal.

### Swipe Down to Compact

**What it is:** Swipe down on the input bar to trigger `/compact`, which summarizes the middle of the conversation and frees up context tokens.

**When to use it:**

- After a long debugging session that's been resolved — compact before starting the next task
- When responses start getting shorter or Claude seems to be losing track of earlier context
- Before switching to a completely different topic in the same chatroom
- Before a `/submit` job that references earlier work — compact first so the job's preamble stays lean

**What compact does:**

CatClaw summarizes the middle messages (preserving the first 2 and last 6) into a single compact marker. The conversation looks shorter, Claude's working context is freed, and the session ID is preserved — Claude still knows the project and can resume where it left off.

**Step-by-step:**

1. Notice the conversation is long (50+ messages)
2. Swipe down on the input bar — the gesture recognizer activates
3. A "Compacting..." indicator appears briefly
4. The conversation collapses to a summary + recent messages
5. Continue working in the same session

---

## Split-Screen Mini Terminal

### What it is

A collapsible SSH terminal panel that slides up from the bottom of the chatroom. You can type shell commands directly without switching to the Terminal tab.

### When to use it

The split-screen terminal is ideal when you need a fast feedback loop between asking Claude something and checking the result on your server:

- Ask Claude to create a file → immediately `cat` the file in the mini terminal to verify
- Ask Claude to refactor a function → run the tests in the mini terminal to confirm nothing broke
- Ask Claude to explain git history → run `git log --oneline -10` alongside to follow along
- Ask Claude to write a deploy script → run it directly in the mini terminal

### Opening the split-screen

1. Connect to SSH (My Mac tab or any connection profile)
2. Open any chatroom
3. Tap the **split-rectangle icon** in the chatroom banner bar (top right area)
4. The mini terminal slides up from the bottom — a new SSH session opens automatically

> The split-rectangle icon only appears when SSH is connected. If you don't see it, check the connection status in the banner.

### Resizing

Drag the horizontal **handle bar** (the thick gray line at the top of the mini terminal):
- Drag **up** to make the terminal taller (up to 400pt)
- Drag **down** to make it shorter (minimum 100pt)
- Drag all the way down to collapse it

### Closing

Tap the split-rectangle icon again to close. The terminal session stays alive in the background and reconnects the next time you open the panel.

### Step-by-step walkthrough — verify-as-you-go coding

1. Open a chatroom for your project
2. Tap the split-rectangle icon to open the mini terminal
3. Ask Claude: "Create a utility function `formatDate(date: Date)` in src/utils/date.ts"
4. While Claude streams the response, type in the mini terminal: `ls src/utils/`
5. The file appears as Claude finishes
6. Type: `cat src/utils/date.ts` to confirm the content
7. Tap the "Are there any bugs?" chip above the input bar
8. Claude reviews its own code
9. If there's a fix, type: `npm run build` in the mini terminal to verify

### Productivity tip — split-screen + branching

Open the split-screen, then branch the conversation. You can:
- Branch 1: one approach → check results in the mini terminal
- Switch to Branch 2: different approach → check results in the same mini terminal
- Compare the terminal outputs to pick the winner

---

## Conversation Branching

### What it is

Fork any conversation at any message. Each branch is an independent continuation from that point — like a save state in a game. You can switch between branches to compare two different approaches without starting a new chatroom.

### When to use it

Branching is most valuable when:
- You want to try "approach A vs approach B" without losing either
- A refactor could go in two directions and you want to explore both
- You asked Claude something and want to take the response in two different directions
- You're prototyping and want a safe fallback if the current direction goes wrong

### Creating a branch

1. Long-press any message in the conversation
2. Tap **"Branch from here"** in the context menu
3. A purple **branch picker** appears in the banner bar showing "Branch 1"
4. Continue the conversation — this is Branch 1

### Switching branches

Tap the purple branch picker in the banner. A sheet appears showing all branches:
- **Main** — the original conversation
- **Branch 1**, **Branch 2**, etc.

Tap any branch to switch. The conversation view updates to show that branch's messages.

### Clearing branches

Tap the purple branch picker → **"Clear branches"**. All forks are deleted and the conversation returns to the main thread.

### Step-by-step walkthrough — API design comparison

**Goal:** Compare REST vs GraphQL for a new API

1. Ask Claude: "Design an authentication API for our mobile app"
2. Read the response
3. Long-press the response → "Branch from here"
4. You're now on Branch 1
5. Type: "Implement this using REST with JWT cookies. Show me the full endpoint structure."
6. Read the REST implementation
7. Tap the branch picker → select **Main**
8. Long-press the same original response → "Branch from here"
9. You're now on Branch 2
10. Type: "Implement this using GraphQL with session tokens. Show me the schema and resolvers."
11. Read the GraphQL implementation
12. Tap the branch picker to switch between Branch 1 and Branch 2
13. Ask Claude on each branch: "What are the trade-offs of this approach?"
14. Pick the winner and continue coding on that branch

### Step-by-step walkthrough — safe refactoring

**Goal:** Refactor a service without risking the working code

1. Ask Claude to explain the current `UserService` implementation
2. Long-press the explanation → "Branch from here"
3. Branch 1: "Refactor UserService to use the repository pattern"
4. If the refactor produces broken code, tap the branch picker → Main
5. Long-press the original explanation → "Branch from here" (creates Branch 2)
6. Branch 2: "Refactor UserService to use dependency injection instead"
7. Compare both — keep the one that compiles and passes tests

### Productivity tip — branching + smart suggestions

On Branch 1, use the "How can this be improved?" chip to push the implementation further.
On Branch 2, use the "Are there any bugs?" chip to stress-test it.
Compare results before committing to one branch.

---

## JSON Table Cards

### What it is

When Claude returns a JSON array in a code block, CatClaw automatically renders it as a formatted table instead of raw JSON text.

### When it triggers

All three conditions must be true:
- The code block is tagged ` ```json `
- The top-level value is an array `[...]`
- Each element is an object `{...}` with consistent keys

### Table features

- **Column headers** — JSON keys, camelCase split into readable words
- **Scrollable rows** — swipe horizontally for wide tables
- **Alternating row shading** — easier to scan
- **Row limit** — 20 rows shown by default; a footer shows "N more rows"
- **Above-table text** — any markdown above the code block still renders normally

### When to use it

Ask Claude to return structured data as a JSON array when you need to compare:
- API endpoint options
- Model specifications and pricing
- Package dependencies and versions
- Server configurations
- Country/region data
- Database schema fields

### Step-by-step walkthrough

**Compare AI model pricing:**

```
Give me a JSON array comparing Claude Sonnet, GPT-4o, and Gemini 2.5 Pro.
Include fields: modelName, inputPricePerMillion, outputPricePerMillion,
contextWindow, and bestFor. Format as a JSON code block.
```

The response renders as a 3-row table with 5 columns — much easier to read than raw JSON.

**Audit npm dependencies:**

```
List the top-level dependencies from our package.json as a JSON array.
For each include: name, version, license, weeklyDownloads (estimate), and isDevDependency.
Format as a JSON code block.
```

Scroll horizontally to see all columns.

**Compare refactoring options:**

```
Give me a JSON array comparing these 3 approaches to rate limiting:
token bucket, leaky bucket, and sliding window.
Include fields: approach, complexity, burstHandling, memoryUsage, and recommendation.
```

A 3-row comparison table renders immediately.

---

## Auto-Context from Terminal

### What it is

When a command fails in the Terminal tab, a floating pill appears at the bottom of the terminal. Tapping it switches to the chatroom with the terminal output already injected — Claude sees what went wrong before you type a single word.

### How the error detection works

The terminal maintains a 200-line ring buffer of recent output. When new output arrives, it's scanned against ~30 error patterns. If a match is found, the "Ask Claude" pill appears and stays for 10 seconds.

Common patterns detected:
- `command not found` — missing tool or typo
- `Permission denied` — file system or SSH permissions
- `SyntaxError:`, `TypeError:`, `ReferenceError:` — runtime exceptions
- `npm ERR!`, `pip install failed` — package manager failures
- `ENOENT`, `EACCES`, `ECONNREFUSED` — Node.js system errors
- `segmentation fault`, `core dumped` — C/C++/Rust crashes
- `error:`, `fatal:`, `ld: error:` — build system errors

### When to use it

Use auto-context instead of copy-pasting whenever:
- A build fails and you need a quick diagnosis
- A deployment script errors out mid-run
- A test runner reports failures with a long trace
- You run an unfamiliar command and get an unexpected error
- A server crashes and spits errors to the terminal

### Step-by-step walkthrough — npm build failure

1. Open the Terminal tab
2. Run: `npm run build`
3. Build fails with several error lines
4. The "Ask Claude" pill appears at the bottom of the terminal
5. Tap it — CatClaw switches to the chatroom
6. The last 30 lines of terminal output are already in context
7. Type: "What's wrong and how do I fix it?"
8. Claude reads the exact error and provides a specific fix

### Step-by-step walkthrough — permission error

1. Run: `sudo systemctl restart nginx`
2. Get: `Permission denied: /etc/nginx/nginx.conf`
3. Tap "Ask Claude"
4. Type: "Why is this failing even with sudo?"
5. Claude sees the exact command and error — diagnoses the issue immediately

### Productivity tip — split-screen + auto-context

Open the split-screen mini terminal in the chatroom. When a command fails in the mini terminal, use the "Ask Claude" chip (or just type your question directly) — the terminal output is already adjacent to the chatroom. You never need to switch tabs at all.

---

## Implicit Learning from Corrections

### What it is

CatClaw detects when you correct Claude and automatically saves the correction to memory — without any `/remember` command. Over time, your corrections build a preference profile that Claude applies automatically in every session.

### What gets detected

CatClaw watches for correction phrases at the start of your message:

- "No, I meant..."
- "That's wrong, actually..."
- "Instead, do..."
- "Actually, I prefer..."
- "Not X, use Y instead"
- "We don't do X here, we..."

When a correction phrase is detected, CatClaw extracts the preference from what follows and saves it as a `userPreference` memory entry with the current project directory as scope.

### How it builds up over time

After 3–5 sessions, your memory store contains a profile of your preferences:
- Naming conventions ("we use camelCase, not snake_case")
- Error handling patterns ("we return Result types, we don't throw")
- File organization ("always put types in /types/, not in the same file")
- Testing preferences ("we use Jest, not Mocha")
- Framework choices ("we use Zustand for state, not Redux")

These preferences are injected into every chatroom session automatically — Claude applies them without being reminded.

### Step-by-step walkthrough

**Session 1:**
```
Claude: Here's the implementation with snake_case variables...

You: No, I meant camelCase — our whole codebase uses camelCase
```
CatClaw saves: "Project uses camelCase variable naming."

**Session 2 (new session, same project):**
```
Claude: (automatically uses camelCase without being asked)
```

**Session 3:**
```
Claude: I've used a try/catch block for error handling...

You: That's wrong, actually — we use Result<T, Error> everywhere in this project
```
CatClaw saves: "Project uses Result types for error handling, not try/catch."

**Session 4:**
```
Claude: (automatically uses Result types for error handling)
```

### Reviewing your saved corrections

1. Tap the **Memories** button (brain icon) in the chatroom toolbar
2. Filter to **This Session** to see what was saved today
3. Filter to **Global** to see preferences that apply across all projects
4. Tap any memory entry to edit or delete it

### When implicit learning saves the most time

- **Onboarding a new project**: After a few sessions of corrections, Claude knows the project's conventions
- **Team preferences**: If everyone on the team uses the same CatClaw setup, the shared preferences build quickly
- **Personal style**: Code formatting, comment style, and variable naming all get learned automatically

---

## Cat Mascot

### What it is

The pawprint icon in the chatroom banner bar is a live state indicator for CatClaw's current activity.

### States at a glance

| What you see | What it means |
|--------------|---------------|
| Gray, static | Idle — Claude is not doing anything |
| Orange, pulsing | Streaming — Claude is generating a response |
| Cyan | Background job running |
| Red | Background job failed |
| Bounces once | Background job just completed |

### Why it helps

The mascot is visible even when you're scrolled to the top of the conversation, or when you've swiped to a different message. You don't need to look at the streaming indicator in the message list to know whether Claude is still working.

If the icon is cyan and you tap the Jobs tab, you'll see which job is running. If it turns red, check the Jobs tab to see why.

---

## Haptic Feedback

### What it is

CatClaw uses distinct vibration patterns for different events — even when the app is in the foreground and you're actively using it.

### Haptic patterns

| Event | Pattern | What it means |
|-------|---------|---------------|
| Job completed | Single firm click | A background job finished successfully |
| Job failed | Double impact | A background job failed |
| SSH disconnected | Soft + medium pulse | The SSH connection dropped |
| Message sent | Soft tap | Your message was sent |

### Why distinct patterns matter

When you have multiple jobs running and your phone is in your pocket or on a desk, you can feel which event occurred without picking it up. A single click means look at the job result. A double impact means something needs attention.

No notification permission is required — haptics work independently of the system notification center.

### Haptics + cat mascot

The haptic pattern and the mascot animation fire together. If you feel a double impact, the mascot icon will be red. If you feel a single click, the mascot bounces once. Both signals confirm the same event from different senses.

---

## Combining Features for Maximum Productivity

### The "verify as you go" workflow

**Features:** Split-screen terminal + smart suggestions + implicit learning

1. Open the mini terminal (split-screen icon)
2. Ask Claude to implement a feature
3. Tap smart suggestion chips to review and improve the code
4. Verify in the mini terminal after each iteration
5. Any corrections you make ("no, we use X here") get auto-saved to memory

After a few sessions, Claude naturally writes code that matches your conventions — less reviewing needed.

---

### The "compare and decide" workflow

**Features:** Conversation branching + split-screen terminal + JSON table cards

1. Ask Claude to propose 3 different approaches to a problem
2. Ask Claude to summarize them as a JSON array — renders as a table
3. Pick 2 approaches to explore
4. Branch from the original response
5. Implement Approach A on Branch 1, Approach B on Branch 2
6. Use the mini terminal on each branch to run tests and verify
7. Tap the branch picker to compare results
8. Continue on the winning branch

---

### The "terminal to fix" workflow

**Features:** Auto-context from terminal + smart suggestions + long-press Send

1. Run a failing script in the Terminal tab
2. Tap the "Ask Claude" pill when the error appears
3. Ask for a diagnosis — Claude sees the full error
4. Tap "How do I fix this?" chip to get the fix
5. If the fix requires a long multi-file change: long-press Send to dispatch as a background job
6. Monitor the job in the Jobs tab
7. Haptic feedback notifies you when done

---

### The "build memory over time" workflow

**Features:** Implicit learning + smart suggestions + split-screen terminal

**Week 1:** Work normally. Make corrections when Claude gets conventions wrong.
**Week 2:** Claude starts getting naming conventions right. Fewer corrections needed.
**Week 3:** Memory is dense enough that Claude produces project-idiomatic code on the first try.

Use the Memories toolbar button at any point to review what CatClaw has learned and edit any entries that are outdated or wrong.
