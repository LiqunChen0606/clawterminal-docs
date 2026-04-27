# Trajectory Library — Examples

When to reach for the Pin button after a `/research` or Search Mode result.

---

## Scenario 1: Auth architecture for a SaaS app

You're early in building a B2B SaaS and need to decide on session handling. You run:

> `/research JWTs vs server-side sessions for a B2B SaaS — which fits better for revocability and scale?`

Native mode explores all three approaches: stateless JWTs, server-side cookies, and a hybrid. The hybrid wins — short-lived JWTs with a server-side blocklist for revocation, score 0.84. The trajectory graph shows the judge's rationale: it captures statelessness at scale while preserving the ability to force-logout any session.

You agree. Tap **Pin**.

For the next two weeks, every implementation question in this chatroom — "how should the logout endpoint work?", "what do I do when a user resets their password?", "how do I handle token refresh on mobile?" — gets answered within that auth model without you explaining it again. Claude already knows the chosen architecture.

Cost of the original search: ~$0.01. Time saved re-explaining context: indefinite.

---

## Scenario 2: Naming a product

Working title is "DevPocket." You open a Search Mode chatroom and send:

> *"come up with a name for a mobile dev terminal app that feels punchy and mobile-first"*

Three chains explore different angles. The judge picks "Forge Mobile" at 0.82, beating "MobiTerm" (too generic) and "ShipKit" (product-adjacent but doesn't signal mobile-first). You read the scorer's rationale — "concise, evokes craft, mobile-native feel" — and agree.

Pin the name.

Any time you ask for marketing copy, App Store descriptions, onboarding text, or UI labels in this chatroom, Claude writes against "Forge Mobile" — not the old working title, not a hedged "whatever you decide to call it." The pin removes ambiguity from every downstream creative decision.

---

## Scenario 3: A principle for a code review chatroom

You have a dedicated chatroom where you paste files and ask Claude to review them. You run:

> `/research what's the most important error-handling principle for a Go web service?`

The winner (score 0.79): *"Surface errors at boundary crossings — HTTP handlers, repository calls, external API clients. Inside those layers, propagate errors up cleanly without swallowing them. Avoid catching errors deep and logging silently — by the time the log entry fires, the call stack context is gone."*

Pin it.

Now every file you paste into this chatroom gets reviewed against that principle automatically. You don't have to say "also check for swallowed errors" every time — it's in the preamble.

---

## Scenario 4: Using the chatroom as an ADR

You're building a data pipeline and debating event sourcing vs. regular CRUD. Long session, multiple stakeholders looking at the screen. You use Search Mode to explore:

> *"event sourcing vs CRUD for a pipeline that needs audit history and replay — what should we choose?"*

Event sourcing wins at score 0.81, with the rationale covering audit log, replay, and the tradeoff of added complexity. You pin the result.

The chatroom now functions as a lightweight architectural decision record. When someone asks "wait, why are we doing event sourcing?" two weeks later, you can open the 📌 management sheet and show them the scoring trail and the judge's reasoning. The decision is documented without needing a separate Confluence page.

---

## Scenario 5: Design system iteration

You're designing the empty state for a new feature. You run three quick `/research` calls to lock in decisions:

1. *"tagline for the empty chatroom state when no SSH connection is configured"* → pins the copy winner
2. *"SF Symbol to use for a 'no connection' state that feels approachable, not alarming"* → pins the symbol choice with rationale
3. *"background treatment: gradient, flat, or subtle pattern for the empty state?"* → pins the visual direction

Three pins, three decisions locked. You then ask Claude to write the SwiftUI for the empty state view and it builds within all three decisions, no re-specification needed.

This works because pinning lets you use `/research` as a cheap design-decision tool, then carry multiple decisions forward into a single implementation prompt.

---

## Scenario 6: When NOT to pin

These are cases where pinning would bloat the preamble for no benefit:

**One-shot lookups.** *"What's the default Postgres port?"* — the answer is 5432, Claude already knows it. Pinning this wastes a slot.

**Debugging sessions.** You found the bug. The root cause is specific to that moment. A pin saying "the crash was caused by a nil force-unwrap in ProfileViewController line 42" is irrelevant to every future message in the chatroom.

**Decisions you might revisit.** If there's a real chance the conclusion changes, a pin commits you to it. Just keep the result message in the chat history and reference it manually. If the decision solidifies later, pin it then.

**Agentic multi-tool work.** When Claude is running background jobs, writing files, and using tools, the text preamble matters less than the task instructions. Pins add tokens without directing behavior in meaningful ways during heavy agentic runs.

**Low-confidence winners.** If the search returned a winner with score 0.45, the engine wasn't confident in the result. Don't pin a coin-flip. Rephrase the research task and run again, or just keep the result in message history and reference it explicitly.

---

## Reading the score before you pin

Before tapping **Pin**, glance at the score in the result sheet header. A rough guide:

| Score | What it means | Should you pin? |
|-------|--------------|-----------------|
| ≥ 0.75 | Strong, well-differentiated winner | Good candidate to pin |
| 0.55–0.74 | Decent winner, runner-up was competitive | Pin if you agree after reading the rationale |
| 0.40–0.54 | Low confidence — chains were similar or weak | Consider rerunning with different framing |
| < 0.40 | Engine struggled | Don't pin; reframe the task |

The trajectory graph helps here too. Even a 0.72 winner can be the right call if the runner-up in the graph was genuinely different (not just a synonym) and the rationale is coherent. Read before pinning.
