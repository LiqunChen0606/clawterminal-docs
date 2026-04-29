# Search Mode — Tutorial

> Most questions have one right answer. Some questions are better served by exploring three different angles at once, scoring them, and giving you the best one. Search Mode is how you ask for that. Type `?your question` and ClawTerminal routes it through a parallel propose-evaluate-refine loop — instead of a single-shot reply, you get the strongest candidate from a small field of alternatives.

---

## Part 1: What Search Mode Actually Does

When you send a regular message, it's a single round trip: your text goes to the AI, the AI replies, done. Fast, cheap, and usually fine.

When you prefix a message with `?`, that message gets routed through the **Search engine** instead. The engine runs a small propose-evaluate-refine loop:

1. **Propose** — three independent chains generate three different candidate replies in parallel. Same task, three angles.
2. **Evaluate** — a scoring call rates all the candidates against the original task and assigns a score from 0 to 1.
3. **Refine** — the top-K candidates seed a refinement round; each gets a follow-up pass that tries to improve on it.
4. **Pick** — the highest-scoring final attempt is posted as the assistant reply, complete with a score badge and cost.

The `?` prefix is the only trigger. There is no chatroom-wide toggle — every `?` message is a deliberate choice to trade a few seconds and a small amount of money for a pressure-tested answer.

---

## Part 2: When to Use It

Search Mode shines for questions where "the right answer" is fuzzy and exploration helps. It's overkill — and slower — for lookups.

**Good fits:**

- Naming things (products, branches, variables, features)
- Drafting a tagline, headline, or commit message
- Exploring design tradeoffs ("Postgres vs DynamoDB for event sourcing — what should I weigh?")
- Brainstorming approaches before you commit to one
- Prompt engineering — iterating on how to phrase something for Claude
- Strategy questions where you want a few angles considered, not just the first

**Not great:**

- "What port does Postgres use by default?" (one-shot lookup)
- "Fix this stack trace" (you want a direct answer, not a search)
- Long agentic work that needs tool use (search runs are stateless text-only)
- Anything where speed matters more than depth

A useful rule: if you'd benefit from getting three different answers and picking your favorite, the `?` prefix does that automatically. If the question has one right answer, just send normally.

---

## Part 3: How to Trigger It

**Inline, per message:** start your message with `?`.

```
?what's the best name for this Swift protocol that validates SSHconnection health?
```

That single message becomes a search. The next message you send normally goes back to single-shot — no mode to flip off.

**Long-press send:** tap and hold the send button → choose **Send with Search** from the context menu. This sends the current message through the search engine once without any prefix typing. Handy if you've already typed your message and just decided it deserves search treatment.

**`??` carve-out:** if you genuinely need a leading `?` as text (say, for a regexp or a markdown table), prefix the message with `??`:

```
??what regex matches optional trailing punctuation
```

The double `??` is stripped to a single `?` and the message is sent normally with no search routing.

---

## Part 4: Two Backends — API Key vs CLI Fallback

Search Mode has two backends depending on whether you've configured an Anthropic API key in Settings.

**With an API key (Settings → API Key):**

The in-app **Native Research Engine** handles the full loop — parallel Haiku 4.5 calls for proposals, scoring, and refinement. You get a live progress indicator as chains spawn, real scores, a full trajectory tree, and a structured result sheet.

- Time: ~10–15 seconds per send
- Cost: ~$0.005–0.02 per search (Haiku 4.5, default 3 chains × depth 2 × K=2)
- Full trajectory graph in the result sheet

**Without an API key (CLI fallback):**

The search is delegated to whatever AI CLI is configured in the chatroom (Claude, Codex, Gemini, or Aider). ClawTerminal sends a search-flavored meta-prompt that instructs the CLI to internally explore three approaches, score them, and report the best. The result is parsed into the same rich result sheet — score badges, ranking — so the UX matches the API-key path.

- Time: ~15–30 seconds (depends on the CLI's own latency)
- Cost: billed against the CLI's own API usage, not ClawTerminal
- Same result sheet, same score display; no trajectory graph

Both paths produce the same result sheet. The main difference is the trajectory graph and tighter per-attempt score data — those only exist in the API-key path.

---

## Part 5: What You'll See When You Send

Type a message starting with `?` and tap send. The message lands in the conversation. Just below it, a placeholder appears: **Searching: \<your task\>** with a spinner.

Behind the scenes the engine spawns three chains. Each chain runs depth-2: it generates an initial proposal, scores it, picks the best K=2 to seed a refinement round, generates the refinement, scores again. All in parallel.

Ten to fifteen seconds later, the placeholder is replaced with the assistant's actual reply — the highest-scoring final attempt. A small score badge ("score 0.82 · $0.012") sits beneath the reply showing confidence and cost.

If you want to see the full search — what the other chains explored, which got refined, why the winner won — tap into the result to open the **Trajectory Graph**.

---

## Part 6: The Trajectory Graph

The graph is a small grid:

- **Columns are chains** — three by default, labeled Chain 1, Chain 2, Chain 3.
- **Rows are refinement rounds** — the initial proposal at the top, refinement rounds below it.
- **Each cell is a node** with that attempt's score, color-coded by band:
  - Green: ≥ 0.7 (strong)
  - Orange: 0.4–0.7 (middling)
  - Red: < 0.4 (weak)
- **The winning attempt has a gold border and a ★ marker.** That's the one you got back as your reply.

Tap any node and a detail sheet slides up showing that attempt's full content plus the scorer's rationale. This is worth reading — you'll see ideas that almost won and refinements that hurt instead of helped.

The graph is available in the API-key path. CLI-fallback results show ranked alternatives in the result sheet but not the full grid.

---

## Part 7: The 🔍 Search Tab in the Jobs Panel

Every `?prefix` message that runs a search creates a job entry in the Background Jobs panel, under the new **🔍 Search** tab.

Each row in the tab shows:

- The actual question you asked (extracted from the task, not the meta-prompt)
- Status: Running / Done / Failed
- Score and cost for completed jobs

Tap a Done row to re-present the full result sheet — so you can revisit a search result after dismissing it, even if you've sent many more messages since.

---

## Part 8: Cost & Performance

A `?prefix` search is not free. With the API-key backend, each search costs about **$0.005–$0.02** on Haiku 4.5 with default settings (3 chains × depth 2 × K=2). For a session where you send 10 `?` messages, that's roughly $0.05–$0.20.

With the CLI fallback, cost is absorbed into the CLI's own billing — you don't see a separate ClawTerminal line item.

What you trade for the cost:

- **Quality on open-ended questions.** The reply has been pressure-tested against alternatives.
- **Latency.** A search takes 10–30 seconds instead of 1–3.
- **Determinism.** Same input twice may give different replies — three chains will explore different angles each time. That's a feature for brainstorming, a bug for "give me the same answer again."

If a message isn't worth the search budget, just send it without the `?`.

---

## Part 9: Tips

- **Use `?` for the messy parts.** Draft normally, then send the hard naming/design questions with `?`.
- **Read the trajectory when something feels off.** If the winner isn't quite right, the graph often has a near-winner that fits better. From the result sheet, any attempt has a "Send as Message" affordance that pastes it into the input bar.
- **Don't combine with Frugal Mode `!` overrides.** Frugal keeps things direct and cheap; search adds exploration overhead. They pull in opposite directions.
- **Watch the score, not just the reply.** A reply with score 0.45 means even the engine wasn't confident. Rephrase the task and search again.
- **`/research <task>`** is still the right tool for long-form, one-off research questions where you want to set chains/budget explicitly. The `?` prefix is for lightweight per-message exploration in your normal chat flow.

---

## Quick Reference

| Action | How |
|--------|-----|
| Run a search on one message | Start the message with `?` |
| Force search from long-press | Hold send button → **Send with Search** |
| Use a literal leading `?` | Start with `??` (doubled prefix) |
| Inspect a search result | Tap the message with the score badge |
| Revisit an old search | Open Jobs panel → 🔍 Search tab → tap the row |
| See alternatives / trajectory | Open the trajectory graph in the result sheet |
| Reuse an alternative | Tap a node in the graph → "Send as Message" |

**API-key backend cost:** ~$0.005–$0.02 per search on Haiku 4.5 with defaults.

**CLI-fallback:** no extra cost to ClawTerminal; billed to the CLI's API account.

**Latency:** 10–15s (API key) or 15–30s (CLI fallback).

**Best for:** naming, brainstorming, design tradeoffs, prompt iteration, drafting.

**Not for:** one-shot lookups, urgent debugging, agentic tool-using work.
