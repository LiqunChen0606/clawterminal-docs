# Session Intelligence

> Master your conversation flow with commands that give you control over how Claude thinks, responds, and remembers.

## /recap — Where Were We?

```
/recap
```

Shows: message count, job stats, recent discussion topics, session cost. Perfect after stepping away from a long session.

**Example output:**

```
Session Recap
─────────────────────────────────
Messages: 34 (18 yours · 16 Claude)
Background jobs: 3 complete · 1 running
Recent topics: auth refactor, JWT expiry handling, unit tests
Session cost: $0.42 (Claude Sonnet 4.6)
─────────────────────────────────
```

---

## /context — How Full Is My Context?

```
/context
```

Visual progress bar showing context window usage. Includes actionable suggestions:

- Over 80%? → `/compact` to free space
- Over 50%? → `/unpin` unused files
- Over 30 messages? → Start a `/new` room

**Example output:**

```
Context Window
████████████████░░░░  78%
~78,000 / 100,000 tokens used

Suggestions:
• Run /compact to summarize and free ~40% of context
• /unpin src/legacy.ts (pinned but unused in last 10 messages)
```

---

## /effort — Think Harder (or Faster)

```
/effort low      — "Just give me the answer"
/effort medium   — Default balanced mode
/effort high     — "Think deeply, consider everything"
```

Great for switching between quick lookups and deep architectural questions.

### /effort low — Quick Answers

```
/effort low
What's the default port for PostgreSQL?
```

Claude gives a direct 1-2 sentence answer. No preamble, no elaboration unless asked.

### /effort medium — Balanced (Default)

```
/effort medium
How should I handle token refresh in this auth flow?
```

Claude explains the approach with enough context to understand the reasoning.

### /effort high — Deep Thinking

```
/effort high
Should we use a monorepo or separate repos for this project?
```

Claude considers trade-offs, edge cases, team size implications, tooling overhead, and long-term maintenance. Expects a longer, structured response.

---

## /btw — Quick Side Question

```
/btw what port is the dev server on?
/btw how many lines is this file?
/btw what's the Swift equivalent of map in Python?
```

Claude sees your full context but keeps the answer to 1-2 sentences. Doesn't derail your main conversation.

### Example: Checking a fact mid-refactor

```
We're refactoring the UserService to use async/await. The callback-based
fetchUser() needs to be replaced. Let's start with the login flow.

/btw what version of Node.js supports top-level await natively?
```

Claude answers the `/btw` in one sentence, then continues with the refactor in the next turn — without losing context.

---

## /retry — Try Again

```
/retry
```

Removes the last exchange and re-sends your message. Useful when Claude misunderstood or you want a different approach.

### Example: Getting a different angle

```
You: How should I design the caching layer?
Claude: [gives a Redis-centric answer]

/retry
Claude: [gives a different answer — perhaps in-memory, CDN, or database-level]
```

No need to rephrase or copy your original message. `/retry` handles it.

---

## /personality — Switch Personas

```
/personality senior     — Opinionated staff engineer, pushes back on bad ideas
/personality mentor     — Patient teacher, uses analogies, asks guiding questions
/personality reviewer   — Meticulous code reviewer, focuses on correctness
/personality architect  — Systems thinker, considers scalability and trade-offs
/personality hacker     — Ship fast, iterate later, simplest solution wins
/personality default    — Standard Claude
/personality            — List all available personas
```

### senior — The Skeptical Staff Engineer

```
/personality senior
I'm thinking of using a global singleton for the database connection pool.
```

Claude pushes back on the tradeoffs, explains the failure modes, and suggests a better pattern — not just "here's how to do it".

### mentor — The Patient Teacher

```
/personality mentor
I don't understand why we need a connection pool at all.
```

Claude explains with an analogy, builds up from first principles, and asks a follow-up question to check understanding.

### reviewer — The Meticulous Reviewer

```
/personality reviewer
Here's the diff for the auth module: [paste diff]
```

Claude focuses on correctness, edge cases, security issues, and missing error handling — not style nits.

### architect — The Systems Thinker

```
/personality architect
We're seeing latency spikes under load. Walk me through the options.
```

Claude considers the full system: database, caching, network, CDN, load balancing, and service dependencies — before suggesting a fix.

### hacker — Ship Fast

```
/personality hacker
I need a working OAuth flow by end of day.
```

Claude gives the shortest path to working code. No over-engineering, no "you should consider", just the thing that ships today.

---

## Pro Tips

- Start deep work with `/effort high` + `/personality architect`
- Use `/btw` to check facts without breaking your flow
- Run `/recap` when returning to a session after a break
- Check `/context` before long prompts to avoid hitting the limit
- Try `/personality hacker` for rapid prototyping, then `/personality reviewer` to audit
- Combine `/effort low` + `/btw` for the fastest possible side question
- `/retry` works best when you want a different angle, not when you want to change the question — edit and resend if you want to rephrase
