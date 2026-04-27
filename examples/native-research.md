# Native Research — Examples

When to reach for `/research` Native mode (the in-app, no-install variant).

---

## Scenario 1: Designing a system prompt

> *"design a system prompt for an AI customer-support agent that handles refund requests, never approves them autonomously, and always escalates to a human for amounts over $50"*

Native mode spawns 3 chains exploring different prompt structures (rules-first, examples-first, role-play with explicit guardrails). Each gets refined twice. Judge picks the variant most likely to actually steer the model. You see the runners-up in the trajectory graph — useful for hybridizing.

Cost: ~$0.01. Time: ~20 seconds.

---

## Scenario 2: Naming a feature

> *"come up with a name for a chatroom mode that automatically routes every message through a propose-evaluate-refine loop"*

You used this exact prompt to land on "Search Mode." The trajectory graph shows the candidates that lost: "Explore Mode" (too generic), "Branch Mode" (collides with conversation branching), "Hunt Mode" (cute but unclear). The judge's rationale tells you why Search Mode won (clear, action-oriented, not collision-prone).

---

## Scenario 3: Drafting App Store copy

> *"write the 4-line What's New blurb for an iOS app update that adds Search Mode and a Trajectory Graph"*

Native mode is great here because copy quality is subjective. 3 chains try different angles (feature-led, benefit-led, intrigue-led). Refinement makes each tighter. You skim 9 attempts in the trajectory graph and pick whichever fits your voice — the gold-bordered winner isn't always the best for *your* brand.

---

## Scenario 4: Strategic options

> *"give me 3 distinct strategies for monetizing a developer tool app — recurring subscription, one-time purchase, freemium — and tell me which fits a niche power-user audience best"*

Search Mode would also work, but `/research` lets you crank up the budget for harder strategic questions. Set chains=4, depth=3, budget=30. The chains explore each strategy in depth before the judge weighs them. Cost: ~$0.04, but you get a defensible answer with explicit reasoning.

---

## Scenario 5: Iterating on a tagline

> *"a one-line tagline for ClawTerminal — punchy, mobile-first, mentions AI"*

Native mode is the right tool. Cost: under 2 cents. Returns 9 candidates in 15 seconds. You pick or remix. Compare to spending 10 minutes brainstorming yourself or paying a copywriter.

---

## Scenario 6: When NOT to use Native mode

These are SimpleTES territory:

- *"find a faster sort for nearly-sorted lists of 1000 ints"* (needs to actually time the code)
- *"pack 20 unit circles into the smallest enclosing rectangle"* (geometric optimization with measurable score)
- *"find a hyperparameter combo that hits 95% accuracy on this dataset"* (needs to train + evaluate)
- *"discover a SQL query plan that's 2x faster than the current one"* (needs EXPLAIN + benchmark)

For those, switch the mode picker to **SimpleTES (Mac)** and let the Mac do the real work.

---

## Scenario 7: Fast iteration during product design

You're spec'ing a new feature and have 5 open questions. In a Native-mode-flavored chatroom:

- *"3 alternatives for the empty state copy when the user has no chatrooms yet"*
- *"3 button labels for 'send and dismiss the keyboard' that aren't 'Send'"*
- *"3 SF Symbol candidates for a 'Search Mode' chip"*
- *"3 ways to phrase the privacy disclosure for the Telegram bot path"*
- *"3 candidate names for the in-app docs viewer that aren't 'Docs Viewer'"*

Each `/research` run is ~$0.01. Total cost: ~$0.05 to explore breadth across 5 design questions. Same money as one Opus reply. Way more decision-ready output.
