# Daily Standup Generator — Tutorial

> `/standup` turns yesterday's work into a three-section standup note — Yesterday / Today / Blockers — in about 10 seconds. It reads your chatroom activity and your git log so you do not have to.

---

## What It Does

Every morning, the same question: "What did I work on yesterday?" You scroll through commits, squint at chatrooms, and piece together a fuzzy answer under pressure before your standup call.

`/standup` does that piecing for you:

1. Pulls the last 24 hours of activity from your chatrooms (user + Claude messages, not stream noise)
2. Pulls `git log --since="24 hours ago" --all --format="%h %s"` from your current project via SSH
3. Asks Haiku to summarize into three sections:
   - **Yesterday** — what you actually did
   - **Today** — what is teed up next
   - **Blockers** — anything unresolved
4. Copies the result to your clipboard automatically
5. If no API key is configured, falls back to a pure-git summary with no AI involvement

By the time your coffee is done, your standup is written.

---

## Getting Started

### Run it

```
/standup
```

A card appears in the chatroom with the three sections. Tap any section to expand the full detail. The content is already copied — paste into Slack, Discord, email, whatever.

### Example output

```
Yesterday
  • Finished the Stripe webhook idempotency refactor (#423)
  • Investigated duplicate-charge bug — root cause: missing UNIQUE constraint
  • Reviewed Alice's PR #418 for the auth refactor
  • Shipped the /preview multi-port fix

Today
  • Add ON CONFLICT DO NOTHING for webhook inserts
  • Write migration for the new external_id unique index
  • Code review PR #421 (payment retry logic)
  • Standup → 1:1 with Liqun

Blockers
  • Waiting on Alice's response re: JWT refresh token rotation approach
```

Tap the card to copy. Tap the pencil icon to edit before posting (inline rich-text editor).

---

## How the Summary Is Built

The summary combines two sources:

**Chatroom signal** — Claude reads the last 24 hours of your own messages and looks for:
- Problems you were working on ("Stripe webhook duplicate charges")
- Files you were focused on (auth.ts, webhook.ts)
- Decisions you made ("going with ON CONFLICT")
- Things you left unresolved ("still need to check with Alice")

**Git signal** — Commit subjects grouped by topic ("auth refactor", "webhook idempotency")

Haiku merges the two into a narrative. The same commit shows up once, not twice. Chatroom chatter without a commit still makes it in as "investigated X" or "decided Y".

---

## Offline / No-API Fallback

If you have not configured an Anthropic API key, `/standup` falls back to a purely local summary:

```
Yesterday (from git log)
  2f1c9d3  fix: webhook idempotency
  a8b4e11  refactor: extract PaymentService
  9e2c7ff  test: add duplicate event coverage

Today
  • (planned — check your TODOs)

Blockers
  • (none detected)
```

Still useful, less flowery. No Haiku call, no spend.

---

## Tips

- **Run it right before standup.** Not the night before. The chatroom signal is freshest the moment you open CatClaw in the morning.
- **Edit, then post.** The AI gets 90% right. The last 10% is usually "reorder the Today list to match my actual priorities." The built-in editor makes this a 5-second fix.
- **Use with `/pin`.** If you pin your current sprint plan, the standup generator can match chatroom activity against the plan and flag what is on-track vs off.
- **Run it on multiple projects.** `/standup` uses the current chatroom's project directory. Run it per-project for cleaner summaries.
- **Pair with `/recap`.** `/recap` is for within-session orientation. `/standup` is for cross-session, last-24-hours orientation. Different tools.

The best standup note is the one you did not have to write. Let Haiku do the first draft. You just edit and ship.
