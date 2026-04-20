# Session Intelligence Tutorial

> Six commands that give you fine-grained control over how Claude thinks, responds, and stays oriented within a session. Use them together to build a conversation rhythm that fits how you actually work.

---

## The Problem These Commands Solve

You open a chatroom at 9 AM and start a big refactor. It's now 2 PM — you've sent 40 messages, run 3 background jobs, and pinned two files. You step away for lunch, come back, and... where were you? What did Claude already figure out? Is the context window getting full? Would a quicker answer style help right now, or do you need Claude to really think this through?

Session Intelligence is a set of commands built for exactly this moment.

---

## The Commands at a Glance

| Command | What it does | When to use it |
|---------|-------------|----------------|
| `/recap` | Session summary: messages, jobs, topics, cost | Returning to a session after a break |
| `/context` | Context window usage bar with suggestions | Before pasting a large file or starting a long prompt |
| `/effort low\|medium\|high` | Tune thinking depth | Switching between quick lookups and deep design work |
| `/btw [question]` | One-sentence side question | Checking a fact without losing your flow |
| `/retry` | Remove last exchange and re-send | When Claude took the wrong angle |
| `/personality [name]` | Switch agent persona | Adapting Claude's style to the task at hand |

---

## Part 1: Returning to a Long Session

### Scenario

You've been working on a payment integration for the past 3 hours. You stepped away to take a call, and now you're back. The chatroom shows 38 messages and 2 completed jobs. Where did you leave off?

### Step 1: Get oriented with /recap

```
/recap
```

CatClaw generates a summary of the current session:

```
Session Recap
─────────────────────────────────
Messages: 38 (19 yours · 19 Claude)
Background jobs: 2 complete · 0 running
Recent topics: Stripe webhook validation, idempotency keys, error retry logic
Session cost: $0.61 (Claude Sonnet 4.6)
─────────────────────────────────
```

In under a second you know: you were working on webhook validation and retry logic. You're at $0.61 in spend. Two jobs finished while you were gone. No active jobs.

### Step 2: Check your context headroom

```
/context
```

```
Context Window
████████████████░░░░  74%
~74,000 / 100,000 tokens used

Suggestions:
• Consider /compact to summarize older messages and free ~35%
• /unpin src/legacy_payments.ts (pinned but not referenced in last 15 messages)
```

You're at 74%. Before you paste that large Stripe API response you were planning to share, it's worth running `/compact` or unpinning that stale file.

### Step 3: Compact and continue

```
/compact
```

Claude summarizes the first 30 messages into a compact context block. You're now back at ~40% usage. Plenty of headroom.

---

## Part 2: Controlling Thinking Depth

### When to use /effort low

Quick factual lookups. You don't need Claude to explain why — just the answer.

```
/effort low
What HTTP status code should a webhook return to acknowledge receipt?
```

Claude: "Return `200 OK`. Any 2xx response tells Stripe the webhook was received successfully."

One sentence. Done. You move on.

### When to use /effort medium (default)

Most day-to-day coding questions. Claude explains the approach without overwhelming you.

```
/effort medium
How should I handle failed webhook deliveries?
```

Claude explains the pattern — idempotency keys, exponential backoff, dead letter queues — with enough context to implement it. Not a lecture, not a one-liner.

### When to use /effort high

Architecture decisions, security reviews, complex debugging. You want Claude to really think before answering.

```
/effort high
We're seeing duplicate charges in production when Stripe retries webhooks
and our handler isn't idempotent. Walk me through how to fix this properly.
```

Claude works through the full picture: idempotency keys at the database level, the race condition during concurrent retries, the recommended Stripe patterns, and what to test. It's a longer response — but for a production bug that's causing duplicate charges, you want thoroughness.

### Switching mid-session

You can change effort level at any point. The setting applies to the next message and stays until you change it again.

```
/effort high
[architectural question — Claude thinks deeply]

/effort low
What's the Stripe webhook signing secret environment variable name?

/effort high
[back to architecture]
```

---

## Part 3: Asking Side Questions Without Losing Your Flow

### The /btw command

You're in the middle of explaining a complex refactor to Claude, and you just need to check one thing — but you don't want to break the conversation thread.

```
We're refactoring the PaymentService to use async/await throughout.
The core issue is that fetchTransaction() wraps a callback-based library.
I want to promisify it and bubble errors properly.

/btw does Node.js util.promisify work with libraries that use error-first callbacks?
```

Claude answers the `/btw` in one sentence — "Yes, `util.promisify` wraps error-first callbacks (err, result) into Promises; if the library follows that convention, it works directly" — then in the next turn continues with the refactor as if the side question never happened.

### /btw vs. a regular question

| | Regular question | /btw |
|-|-----------------|------|
| **Answer length** | As long as needed | 1–2 sentences |
| **Conversational continuity** | Claude may shift focus | Claude stays on the main thread |
| **Good for** | "Explain X" | "Is X true?" / "What's the value of Y?" |

### Examples

```
/btw is JSON.stringify safe for circular references?
/btw how many characters is a UUID?
/btw does SwiftData support relationships across containers?
/btw what's the Stripe test card for a declined charge?
```

---

## Part 4: Recovering When Claude Misses the Mark

### The /retry command

Sometimes Claude understands the words but takes the wrong angle — overly generic, too detailed, or focused on the wrong part of the problem.

```
You: How should I structure the webhook handler?
Claude: [gives a generic REST API design answer — not what you wanted]

/retry
Claude: [tries again — this time focuses on webhook-specific concerns:
         signature verification, idempotency, async processing, response timing]
```

### When /retry helps

- Claude interpreted the question too broadly
- The answer was technically correct but not useful for your situation
- You want a different approach (more concrete, more abstract, different trade-offs)
- The response was too long or too short

### When to rephrase instead

If you realize your question was ambiguous or missing context, edit it and resend rather than retrying. `/retry` re-sends the exact same message — it's for getting a different answer to the same question, not a different question.

---

## Part 5: Switching Agent Personas

### The /personality command

Different tasks call for different modes of thinking. CatClaw lets you switch Claude's persona mid-session without starting a new chatroom.

```
/personality           — list all personas
/personality default   — back to standard Claude
```

### Persona Reference

#### senior — The Staff Engineer

```
/personality senior
I'm thinking of using a global singleton for the database connection.
```

Claude pushes back. It explains the failure modes (thread safety, test isolation, connection leaks), gives the idiomatic alternative, and doesn't soften the critique. Use this when you want honest engineering feedback, not validation.

#### mentor — The Patient Teacher

```
/personality mentor
I don't fully understand why we need a connection pool.
```

Claude starts from first principles, uses an analogy (like a hotel concierge desk), builds up to the technical explanation, and ends with a question to check your understanding. Use this when you're learning something new or want to deeply understand a concept.

#### reviewer — The Meticulous Reviewer

```
/personality reviewer
Here's the auth module diff. What's wrong with it?
[paste diff]
```

Claude focuses on what could go wrong: missing error handling, edge cases, security issues, race conditions. It doesn't comment on formatting or style unless there's a correctness implication. Use this before a PR or when you suspect there are bugs you haven't seen yet.

#### architect — The Systems Thinker

```
/personality architect
We're seeing latency spikes when traffic doubles. Where do I start?
```

Claude steps back and looks at the whole system — database query patterns, connection pool sizing, caching layers, CDN configuration, service dependencies. It maps the problem space before suggesting a solution. Use this for performance issues, scaling decisions, and infrastructure design.

#### hacker — Ship It

```
/personality hacker
I need OAuth working by end of day.
```

Claude gives you the shortest path to a working implementation. No over-engineering. No "you might want to consider". Just the thing that ships today. Use this for prototypes, MVPs, and time-constrained tasks.

---

## Part 6: Real-World Workflows

### Morning Standup Workflow

You're starting the day and want to orient quickly before your standup call.

```
/recap
```

Check what you got done yesterday, what jobs ran overnight, and your session cost for context.

```
/context
```

If context is over 50%, start a new chatroom for today's work. Clean slate, fresh context.

```
/effort medium
What are the most important things to finish in PaymentService today?
```

Claude draws on yesterday's conversation context (if still in window) to give you a prioritized list.

### Deep Work Session

You're about to tackle a hard architectural problem. You want Claude to really engage.

```
/effort high
/personality architect
```

Now ask your question. Claude will think through the full system — trade-offs, failure modes, scalability implications — before giving a recommendation.

```
/pin src/PaymentService.swift
/pin docs/architecture.md
```

Keep the relevant files in context throughout.

```
/context
```

Check your headroom every 20-30 messages. Run `/compact` if you're over 70%.

When you have a proposal and want a critical review:

```
/personality reviewer
Here's the design I'm planning. What are the risks?
```

### Quick Lookup Workflow

You're in the middle of implementing something and just need facts.

```
/effort low
/btw what's the max size for a Stripe webhook payload?
/btw does URLSession cache GET requests by default on iOS?
/btw what's the Swift KeyPath syntax for nested properties?
```

Three side questions answered in three sentences. Your main conversation is unaffected.

### Prototype to Production

Start fast, then audit.

```
/personality hacker
/effort low
Build a working OAuth flow for GitHub — just needs to work, polish later.
```

Claude gives you the minimal working implementation.

When it's working and you're ready to harden it:

```
/personality reviewer
/effort high
Review the OAuth implementation for security issues and production readiness.
```

Claude switches to critical mode — looks for token storage issues, CSRF protection, state parameter validation, error handling gaps.

---

## What's Next

- [UX & Intelligence Features](ux-features.md) — smart suggestions, gesture shortcuts, and more
- [AI Analysis & Automation](ai-analysis.md) — `/security`, `/spec`, `/gentest` for production-readiness checks
- [Cross-Session AI Memory](cross-session-memory.md) — save preferences so you don't have to set `/effort` or `/personality` every session
- [Session Intelligence Examples](../examples/session-intelligence.md) — copy-paste recipes for every command in this tutorial
