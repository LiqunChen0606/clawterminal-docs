# Search Mode — Examples

Real scenarios for `/search on` plus `🔍 Search Mode` chatrooms.

---

## Scenario 1: Naming a product

You're shipping a new mobile dev tool and the working title is "ClawTerminal." Curious if there's something snappier.

**Without Search Mode:** "give me a snappier name for a mobile dev terminal app" → one safe answer.

**With Search Mode on:** the same prompt fans out into 3 chains. Each chain proposes something deliberately different ("MobiCode", "Pocket Workbench", "Forge.app"). Haiku scores them on memorability + brand fit. The top-2 get refined ("MobiCode → MobiForge → Forge Mobile") for two more rounds. The best result lands in your chat with a 🏆 badge and a 0.85/1.00 score.

Tap the result message → the trajectory graph shows you why "Forge Mobile" beat the others (concise, memorable, evokes craft) plus the 9 other names you can pick from if you disagree with the judge.

---

## Scenario 2: Exploring design tradeoffs

> *"Should I store user sessions as JWTs or server-side cookies for a B2B SaaS app?"*

In a normal chatroom: one nuanced answer with both options weighed.

In Search Mode: 3 chains generate independent positions.
- Chain 1 advocates JWT (stateless, easy to scale).
- Chain 2 advocates server-side cookies (revocability, smaller payload, CSRF-resistant with proper flags).
- Chain 3 proposes a hybrid (short-lived JWTs + a server-side blocklist for revocation).

Refinement rounds force each chain to address the others' best counter-arguments. The hybrid ends up scoring highest because it captures the tradeoff explicitly. You see the full reasoning of each "side" before the judge picks a winner.

Useful when you don't trust a single LLM answer to fairly weigh tradeoffs.

---

## Scenario 3: Drafting a tagline or headline

> *"write a one-line tagline for a mobile dev tool that runs SSH + AI chat from your phone"*

Search Mode burns ~12 Haiku calls (under 2 cents) and gives you the top of 9 distinct attempts:

> 🏆 *"Your Mac. In your pocket. Powered by AI."* — score 0.83
>
> Other contenders: "Mobile dev, no compromises." (0.72), "SSH + Claude. From anywhere." (0.65), …

Cheaper and faster than spinning up a brainstorming session, and you get to inspect why one phrase beat the others.

---

## Scenario 4: Brainstorming policy options

> *"what are 3 different ways to handle rate limiting in a public API I'm shipping?"*

Search Mode forces breadth: the 3 chains each propose a *distinct* approach (token bucket / leaky bucket, IP-based with allowlist, signed-request quotas). Refinement asks each to address its weakness ("token bucket — what about spike traffic? IP-based — what about NATted users?"). The judge picks the most defensible final answer plus a clear reasoning trail.

Better than asking for "give me 3 options" and getting three vaguely-different versions of the same thing.

---

## Scenario 5: Iterating on a system prompt

> *"design a system prompt for an AI customer-support bot that handles refund requests but never approves them autonomously"*

This is a meta-task: you want the LLM to design something for an LLM. Search Mode lets the model try multiple prompt structures (rules-first, examples-first, chain-of-thought-first) and lets the judge tell you which one most reliably steers the model. Cheap iteration loop.

---

## Scenario 6: When NOT to use Search Mode

These should stay in normal chat — Search Mode would burn money for no benefit:

- *"What's the default Postgres port?"* (one-shot lookup)
- *"Run `git status` for me"* (tool use, not exploration)
- *"Why did this build fail?"* (debugging — needs the error context, not breadth)
- *"Continue working on the AuthService refactor"* (agentic work)

Toggle off via `/search off` or just create a separate non-search chatroom for these.

---

## Scenario 7: Reading the trajectory graph

After Search Mode lands a result, tap the 🏆 message → result sheet → expand "Search trajectory."

```
Round    Chain 1   Chain 2   Chain 3
  0      [0.45]    [0.55]    [0.30]
  1      [0.65]    [0.55]    [0.50]
  2      [0.85★]   [0.70]    [0.50]
```

- Score color tells you at a glance which chain found gold (green ≥0.7 / orange 0.4–0.7 / red <0.4).
- Gold-bordered ★ cell is the overall winner.
- Tap any cell → see that attempt's full content + the scorer's rationale.

It's the same data SimpleTES exposes, minus the Python execution: useful for sanity-checking the search picked the right answer, or for cherry-picking a runner-up if you disagree with the judge.
