# Frugal Mode — Tutorial

> Strip every bell and whistle. No skills, no memory, no dream profile, no pinned files. Just your message, Claude, and "do it." Frugal Mode is for when you want the cheapest, fastest, most direct answer possible.

---

## What It Does

Your normal message to Claude carries a *lot* of context:

- All enabled skills (code-review, tdd, security, etc.)
- Cross-session memory facts
- Your dream profile (learned preferences)
- Device context (time, battery, calendar, motion)
- Tribal knowledge file
- Every pinned file
- Background job result summaries waiting to be read

That context is usually a feature. For simple questions it is pure noise and token cost. Frugal Mode strips all of it.

What survives the strip:

- Your message
- `/soul` (persistent chatroom identity)
- `/personality` (current persona)
- `/effort` (thinking depth dial)

That is it. Five or six tokens of framing, then your actual prompt. Claude is also instructed to act directly without asking approval questions — it just does the thing.

---

## Two Ways to Go Frugal

### Mode 1 — Per-chatroom toggle: `/frugal`

```
/frugal
```

Turns Frugal Mode on for this chatroom. A "💸 Frugal" chip appears in the header. Every message until you run `/frugal` again is lean.

```
/frugal
```

(run it again) — turns it off. Chip disappears. You are back to full context.

Good for: "I am doing a batch of small tasks and none of them need the full brain."

### Mode 2 — One-off prefix: `!`

Prefix any single message with `!` to send that message frugal — without changing the chatroom setting:

```
!what time is it in Tokyo?
!convert 48°F to Celsius
!what's the Swift keyword for a computed property?
```

Three one-off frugal sends. The next message without `!` is back to full context.

Good for: "I am deep in a long conversation and I just need one quick lookup without tainting the thread."

---

## Why "Frugal"?

Two reasons.

**1. Token cost.** A full preamble with skills and memory can easily hit 3,000–5,000 tokens before your message even starts. Frugal drops that to maybe 100. On a heavy day that is a real dollar amount saved.

**2. Response style.** Remove the scaffolding and Claude naturally becomes terser. It stops hedging, stops asking clarifying questions, stops suggesting "you might want to consider". It just answers.

---

## Real Examples

### Example 1 — Quick math

**Normal mode:**
> User: What's 2^16?
> Claude: 2^16 is 65,536. This is a commonly referenced number in computing because it's the total number of 16-bit unsigned integers, ranging from 0 to 65535. Let me know if you'd like more context about binary powers or related calculations.

**Frugal (`!`):**
> User: !what's 2^16?
> Claude: 65,536.

### Example 2 — API reminder

**Normal mode:**
> User: What's the NSURLSession method for a GET?
> Claude: *Gives 3 paragraphs about URLSession, dataTask vs downloadTask, completion handlers, async/await options...*

**Frugal (`/frugal` on):**
> User: What's the NSURLSession method for a GET?
> Claude: `URLSession.shared.data(from: url)` or `dataTask(with: url)`.

### Example 3 — Running a command

**Normal mode:**
> User: Check if any files changed in the last hour.
> Claude: I can help with that. Would you like me to search the current project directory? Should I include hidden files? Also, do you want this formatted as a list or a count?

**Frugal mode:**
> User: Check if any files changed in the last hour.
> Claude: *(runs `find . -mmin -60 -type f` without asking and returns the list)*

That last one is the killer feature. Frugal Mode tells Claude to act, not ask.

---

## The Header Chip

When Frugal Mode is on, the chatroom header shows a pink "💸 Frugal" chip. Tap it to see exactly what is being stripped:

```
Frugal Mode Active

Skipping:
  • 12 enabled skills
  • 34 memory entries
  • Dream profile
  • 2 pinned files
  • Tribal knowledge
  • Device context
  • 1 pending job summary

Keeping:
  • /soul (identity)
  • /personality (senior)
  • /effort (low)

Est. savings: ~2,800 tokens/message
```

Tap "Turn off" in the popover or run `/frugal` again to exit.

---

## Tips

- **Default to full context.** Frugal is a *tool*, not a habit. Most of your messages benefit from the full preamble.
- **`!` for lookups, `/frugal` for sprints.** Use the prefix for one-off facts, the toggle when you are doing 20 small follow-ups in a row.
- **Frugal + `/effort low` + `/personality hacker`** is the absolute shortest path from question to answer. For prototyping, nothing beats this combo.
- **Do not use Frugal for big decisions.** Architecture, security, complex refactors — those need the full context. Dream profile and memory exist for a reason.
- **Check your spend weekly.** If `/cost` looks bloated, try going frugal for a day and see if the difference is meaningful.

Frugal Mode is for developers who know what they are doing and just need Claude to keep up.
