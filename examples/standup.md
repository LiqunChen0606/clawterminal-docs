# Daily Standup Generator

> `/standup` writes your standup note in 10 seconds from yesterday's chatroom activity + git log. Three sections: Yesterday / Today / Blockers. Auto-copied to clipboard.

## Run it

```
/standup
```

Card appears with three sections. Clipboard is already loaded — paste into Slack.

## Example output

```
Yesterday
  • Finished Stripe webhook idempotency refactor (#423)
  • Investigated duplicate-charge bug — root cause: missing UNIQUE constraint
  • Reviewed Alice's PR #418 for auth refactor
  • Shipped /preview multi-port fix

Today
  • Add ON CONFLICT DO NOTHING for webhook inserts
  • Write migration for external_id unique index
  • Code review PR #421 (payment retry logic)
  • 1:1 with Liqun at 2 PM

Blockers
  • Waiting on Alice re: JWT refresh token rotation approach
```

## How it works

1. **Chatroom signal** — last 24h of your messages, Claude-side decisions
2. **Git signal** — `git log --since="24 hours ago" --all --format="%h %s"` via SSH
3. **Haiku** merges the two into the three-section narrative
4. **Clipboard** gets the plain-text version; the card shows the rendered version

Latency: 5–10 seconds depending on how much activity needs summarizing.

Cost: ~$0.01 per call (Haiku).

## No-API fallback

If no Anthropic API key is configured, `/standup` falls back to local git-only:

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

Still useful. Zero cost, zero network.

## When to run it

- Right before your standup call (not the night before — freshest signal in the morning)
- After long-weekend gaps to recap Friday's work
- After a sprint wrap-up to summarize the final day

## Multi-project workflow

`/standup` uses the current chatroom's project directory. For multi-repo days:

```
[switch to iOS chatroom]
/standup

[switch to backend chatroom]
/standup
```

Copy each output, combine in your standup note.

## Tips

- **Edit before posting.** The AI gets 90% right. Use the inline editor to reorder Today items by priority.
- **Pair with `/pin`.** If your sprint plan is pinned, the standup will match activity against it.
- **Standup is not recap.** Use `/recap` for within-session orientation. Use `/standup` for last-24h summary.
- **Run it solo too.** Standup notes are useful even when you do not have a standup — they are a daily journal in three bullets.
