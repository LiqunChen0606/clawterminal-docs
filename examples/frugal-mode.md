# Frugal Mode

> Strip everything — skills, memory, dream profile, pinned files, device context — and tell Claude to just act. Two triggers: per-chatroom `/frugal` or one-off `!` prefix.

## Per-chatroom toggle

```
/frugal
```

Pink "💸 Frugal" chip appears in header. Every subsequent message is lean.

```
/frugal
```

(run again) — turns off. Chip disappears.

## One-off prefix

Send a single frugal message without changing the chatroom setting:

```
!what's 2^16?
!convert 48F to C
!swift keyword for computed properties
```

Next non-`!` message is back to full context.

## What stays, what goes

### Stripped

- Enabled skills
- Cross-session memory entries
- Dream profile (learned preferences)
- Device context (time, battery, etc.)
- Tribal knowledge file
- All pinned files
- Pending job result summaries

### Kept

- `/soul` (persistent identity)
- `/personality` (current persona)
- `/effort` (thinking depth)

## Real comparisons

### Quick factual question

**Normal mode (long):**
> You: What's the default PostgreSQL port?
> Claude: PostgreSQL runs by default on port 5432. This is configurable via postgresql.conf or the -p flag, but 5432 is the standard. You may encounter systems running on other ports (e.g. 5433 for test instances), so always verify...

**Frugal mode:**
> You: !what's the default postgres port?
> Claude: 5432.

### Action-oriented request

**Normal mode:**
> You: Delete all .log files older than 7 days in the current directory.
> Claude: I can help with that. A few clarifying questions — are you on Linux or macOS? Should I include hidden files? Would you like me to show you the command first or just run it?

**Frugal mode:**
> You: Delete all .log files older than 7 days in the current directory.
> Claude: *(runs `find . -name "*.log" -mtime +7 -delete` and reports: "Deleted 14 files.")*

## The header chip

Tap it to see what is being saved:

```
Frugal Mode Active

Skipping:
  • 12 enabled skills
  • 34 memory entries
  • Dream profile
  • 2 pinned files
  • Tribal knowledge
  • Device context

Keeping:
  • /soul (identity)
  • /personality (senior)
  • /effort (low)

Est. savings: ~2,800 tokens/message
```

## Workflow combinations

### Cheapest possible answer

```
/frugal
/personality hacker
/effort low
```

### Quick side lookups without derailing

```
[deep conversation going]
!what's the Stripe test card number?
[continue conversation — main context unchanged]
```

### Spend-sensitive day

```
/frugal
```

Leave it on for the whole session. Watch your `/cost` stay low.

## Tips

- Default to full context. Frugal is a tool, not a habit.
- Use `!` for one-off facts, `/frugal` for sprints of small follow-ups.
- Frugal + `/effort low` + `/personality hacker` is the shortest path from question to answer.
- Do not use Frugal for architecture or security decisions — they need full context.
