# Search Mode — Examples

Real scenarios for the `?prefix` per-message search trigger.

---

## Scenario 1: Naming a product

You're shipping a new mobile dev tool and the working title is "ClawTerminal." Curious if there's something snappier.

**Without search:** "give me a snappier name for a mobile dev terminal app" → one safe answer.

**With `?prefix`:** type `?give me a snappier name for a mobile dev terminal app` and send. Three chains propose something deliberately different ("MobiCode", "Pocket Workbench", "Forge.app"). Haiku scores them on memorability + brand fit. The top-2 get refined ("MobiCode → MobiForge → Forge Mobile") for two more rounds. The best result lands in your chat with a score badge (e.g. 0.85/1.00).

Tap the result message → the trajectory graph shows you why "Forge Mobile" beat the others (concise, memorable, evokes craft) plus the other candidates you can pick from if you disagree with the judge.

---

## Scenario 2: Exploring design tradeoffs

> `?Should I store user sessions as JWTs or server-side cookies for a B2B SaaS app?`

In a normal chatroom: one nuanced answer with both options weighed.

With `?prefix`: 3 chains generate independent positions.
- Chain 1 advocates JWT (stateless, easy to scale).
- Chain 2 advocates server-side cookies (revocability, smaller payload, CSRF-resistant with proper flags).
- Chain 3 proposes a hybrid (short-lived JWTs + a server-side blocklist for revocation).

Refinement rounds force each chain to address the others' best counter-arguments. The hybrid ends up scoring highest because it captures the tradeoff explicitly. You see the full reasoning of each "side" before the judge picks a winner.

Useful when you don't trust a single LLM answer to fairly weigh tradeoffs.

---

## Scenario 3: Drafting a tagline or headline

> `?write a one-line tagline for a mobile dev tool that runs SSH + AI chat from your phone`

The search burns ~12 Haiku calls (under 2 cents) and gives you the top of 9 distinct attempts:

> "Your Mac. In your pocket. Powered by AI." — score 0.83
>
> Other contenders: "Mobile dev, no compromises." (0.72), "SSH + Claude. From anywhere." (0.65), …

Cheaper and faster than a brainstorming session, and you can inspect why one phrase beat the others.

---

## Scenario 4: Brainstorming policy options

> `?what are 3 different ways to handle rate limiting in a public API I'm shipping?`

The `?prefix` forces breadth: the 3 chains each propose a distinct approach (token bucket / leaky bucket, IP-based with allowlist, signed-request quotas). Refinement asks each to address its weakness ("token bucket — what about spike traffic? IP-based — what about NATted users?"). The judge picks the most defensible final answer plus a clear reasoning trail.

Better than asking for "give me 3 options" and getting three vaguely-different versions of the same thing.

---

## Scenario 5: Iterating on a system prompt

> `?design a system prompt for an AI customer-support bot that handles refund requests but never approves them autonomously`

This is a meta-task: you want the LLM to design something for an LLM. The `?` prefix lets the model try multiple prompt structures (rules-first, examples-first, chain-of-thought-first) and lets the judge tell you which one most reliably steers the model. Cheap iteration loop.

---

## Scenario 6: CLI fallback path — no API key configured

You haven't set up an Anthropic API key in Settings, but you still want exploratory search.

> `?what's the best data structure for a leaderboard that needs O(log n) insert and O(1) rank lookup?`

ClawTerminal falls back to the chatroom's active CLI (Claude, Codex, Gemini, or Aider). It sends a search-flavored meta-prompt that instructs the CLI to explore three approaches internally, score them, and report the best. The result still lands in the same rich result sheet with score badges and ranked alternatives — the trajectory graph isn't available, but the ranked list gives you the same decision information.

Cost comes out of the CLI's own API usage, not a separate ClawTerminal charge. Latency is slightly longer (~15–30s) because the CLI does the loop in a single background job rather than parallel API calls.

---

## Scenario 7: When NOT to use the `?` prefix

These should stay as regular messages — the search prefix would burn time and money for no benefit:

- `What's the default Postgres port?` (one-shot lookup)
- `Run git status for me` (tool use, not exploration)
- `Why did this build fail?` (debugging — needs the error context, not breadth)
- `Continue working on the AuthService refactor` (agentic work)

For those, just send normally without the `?`.

---

## Scenario 8: Revisiting a result from the Jobs tab

You ran a `?` search on a naming question an hour ago, dismissed the result sheet, and now you want to share the trajectory with a teammate.

Open the **Jobs panel** → tap the **🔍 Search** tab. Your question appears as a row. Tap it — the full result sheet re-opens with all the scored attempts and the trajectory graph still intact. From there you can tap any alternative and "Send as Message" to paste it into the chat input.

Nothing is lost just because you dismissed the result the first time.
