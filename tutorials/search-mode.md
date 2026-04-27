# Search Mode — Tutorial

> Most chats with an AI feel like asking a single oracle: you type, it answers, you refine, it answers again. Search Mode flips that on its head. Flip the toggle and every message you send becomes a small parallel search — three chains explore different angles at once, the best ones get refined, and the strongest result lands as your reply. You don't change how you type; you just get answers that have already been pressure-tested before they reach you.

---

## Part 1: What Search Mode Actually Does

When Search Mode is off, sending a message is a single round trip: your text goes to Claude, Claude writes a reply, you see it. Fast and cheap, but it's one shot.

When Search Mode is on, every regular message you send goes through the **Native Research Engine** instead. The engine runs a small propose-evaluate-refine loop:

1. **Propose** — three independent chains generate three different candidate replies in parallel. Same prompt, three angles.
2. **Evaluate** — another Claude call rates all the candidates against the original task and assigns a score.
3. **Refine** — the top-K candidates seed a refinement round; each gets a follow-up pass that tries to improve on it.
4. **Pick** — the highest-scoring final attempt is posted as your assistant reply, complete with a score badge and the dollar cost of the search.

Defaults are tuned for chat frequency: 3 chains × depth 2 × K=2, total budget about 12 Haiku calls. That's roughly $0.005–0.02 per message on Haiku 4.5. Slower than single-shot (a search takes ~5–15 seconds) but the answers are noticeably better for open-ended questions.

The best part: **you don't change how you type.** Just send a message. The chip in the mode banner reminds you the room is doing search instead of single-shot.

---

## Part 2: When to Use It

Search Mode shines for questions where "the right answer" is fuzzy and exploration helps. It's overkill — and slow — for lookups.

**Good fits:**

- Naming things (products, branches, variables, files)
- Drafting a tagline, headline, or commit message
- Exploring design tradeoffs ("Postgres vs DynamoDB for event sourcing — what should I weigh?")
- Brainstorming approaches before you commit to one
- Prompt engineering — iterating on how to ask Claude something
- Strategy questions where you want a few angles considered, not just the first one

**Not great:**

- "What port does Postgres use by default?" (one-shot lookups)
- "Fix this stack trace" (you want a direct answer, not a search)
- Long agentic work that needs tool use (search runs are stateless text-only)
- Anything where speed matters more than depth

A useful rule: if you'd benefit from getting three different answers and picking your favorite, Search Mode does that for you automatically. If the question has one right answer, just chat normally.

---

## Part 3: How to Enable

Two paths.

**At room creation:** tap **+** in the room tab bar. The new-chatroom sheet shows three toggles — Frugal, Super Research, and **Search Mode**. Tick Search Mode. Tap Create. The new room opens with the indigo 🔍 chip already visible.

**At runtime in any room:**

```
/search on
```

A toast confirms, the 🔍 chip appears in the mode banner above the input field, and from the next message onward you're in Search Mode. Flip it off the same way:

```
/search off
```

The toggle is per-chatroom. You can leave one room in search mode and use another for regular chat without any conflict.

---

## Part 4: What You'll See When You Send

Type a message and tap send. Your message lands in the conversation as usual. Just below it, a placeholder appears: **🔍 Searching: <your task>** with a small spinner.

Behind the scenes the engine spawns three chains. Each chain runs depth-2: it generates an initial proposal, scores it, picks the best K=2 to seed a refinement round, generates the refinement, scores again. All in parallel.

Five to fifteen seconds later, the placeholder is replaced with the assistant's actual reply — the highest-scoring final attempt. A small score badge ("score 0.82 · $0.012") sits beneath the reply showing how confident the engine was in this answer and what the search cost.

If you want to see the full search — what the other chains explored, which got refined, why the winner won — tap into the result. The new **Trajectory Graph** opens.

---

## Part 5: The Trajectory Graph

The graph is a small grid:

- **Columns are chains** — three by default, labeled Chain 1, Chain 2, Chain 3.
- **Rows are refinement rounds** — the initial proposal at the top, refinement rounds below it.
- **Each cell is a node** with that attempt's score, color-coded by band:
  - Green: ≥ 0.7 (strong)
  - Orange: 0.4–0.7 (middling)
  - Red: < 0.4 (weak)
- **The winning attempt has a gold border and a ★ marker.** That's the one you got back as your reply.

Tap any node and a detail sheet slides up showing that attempt's full content plus Claude's rationale for the score it got. This is genuinely interesting to read — you'll see ideas that almost won and refinements that hurt instead of helped. It's also useful debugging when a search returns something weird: skim the alternatives and you'll often spot a better candidate that just got out-scored.

The graph is collapsible inside the result sheet, but it opens by default.

---

## Part 6: Cost & Performance

Search Mode is not free. Each send costs about **$0.005–$0.02** on Haiku 4.5 with the default settings. For a chat session of 20 messages, that's roughly $0.10–$0.40 — meaningful, but not crazy.

What you trade for the cost:

- **Quality on open-ended questions.** The reply has been pressure-tested against alternatives.
- **Latency.** A single send takes 5–15 seconds instead of 1–3.
- **Determinism.** Same input twice may give different replies — three chains will explore different angles each time. That's a feature for brainstorming, a bug for "give me the same answer again."

If a particular send isn't worth the search budget, flip Search Mode off (`/search off`), send the message, and flip it back on. Or use a separate non-search chatroom for those messages.

---

## Part 7: Tips

- **Use it for the messy parts.** Open the chatroom in Search Mode for the brainstorm, then either copy the result into a normal room for follow-up or flip Search Mode off when you've narrowed in on the path forward.
- **Read the trajectory when something feels off.** If the winner isn't quite right, the graph often has a near-winner that fits better. Long-press the assistant reply → there's a "Send as Message" affordance from the result sheet that pastes any attempt into the input bar.
- **Don't combine with Frugal Mode.** Frugal strips the preamble to keep replies cheap and direct; Search adds parallel exploration. They pull in opposite directions, and Search will dominate the cost anyway.
- **Watch the score, not just the reply.** A reply with score 0.45 means even the engine wasn't confident. That's a hint to rephrase your task before sending another one.

---

## Quick Reference

| Action | How |
|--------|-----|
| Enable for a new room | Tick the third toggle in NewRoomSheet |
| Enable for an existing room | `/search on` |
| Disable | `/search off` |
| Inspect a search | Tap the assistant reply with the score badge |
| See alternatives | Open the trajectory graph in the result sheet |
| Reuse an alternative | Tap a node in the graph, "Send as Message" from the detail sheet |

**Cost:** ~$0.005–$0.02 per send on Haiku 4.5 with defaults (3 chains × depth 2 × K=2 × 12 calls).

**Latency:** 5–15 seconds per send.

**Best for:** open-ended questions, brainstorming, naming, design tradeoffs, prompt iteration.

**Not for:** one-shot lookups, urgent debugging, agentic tool-using work.
