# Native Research — Tutorial

> The `/research` command used to require installing SimpleTES (a Python search engine) on your Mac. That was great if you wanted real code-execution evaluators. It was overkill if you just wanted to brainstorm a name, design a prompt, or explore a tradeoff. Native mode runs the same propose-evaluate-refine loop entirely in-app via Claude Haiku — no install, no Mac needed, ~$0.01 per run.

---

## Two Modes, One Command

`/research <task>` now opens a confirmation card with a Mode picker:

- **Native** (default) — runs in the app. Claude proposes, Claude scores, Claude refines. Best for text-heavy tasks. No setup.
- **SimpleTES** — runs Python on your Mac via SSH. Best for code/optimization tasks where a real evaluator can score the result. Requires `/simpletes install` first.

Pick in the card; everything else is identical.

---

## What Each Mode Is Good For

| Task | Best Mode | Why |
|------|-----------|-----|
| Naming a product, app, feature | Native | Pure text; LLM judges fine |
| Designing a system prompt | Native | LLM evaluates LLM steering well |
| "Find a faster sorting algorithm for X" | SimpleTES | Needs to actually run + time the code |
| Drafting marketing copy | Native | Subjective scoring is fine |
| Optimizing a SQL query | SimpleTES | Needs EXPLAIN + benchmarking |
| Strategic options / policy choices | Native | Judging is reasoning, not execution |
| "Find an init pattern that hits 99% test coverage" | SimpleTES | Coverage is a real metric |
| Improving an error message | Native | Quality judgment, no execution |

Rule of thumb: **if a human judge would be subjective, use Native. If success is measurable, use SimpleTES.**

---

## Native Mode Walkthrough

1. Type `/research design a prompt that gets Claude to refuse jailbreak attempts but explain why`
2. The Research Task card opens with the task summary on top
3. **Mode picker:** keep it on **Native** (default)
4. Three knob steppers below:
   - **Parallel chains** (1–6, default 3) — how many independent approaches to explore
   - **Best-of-K** (1–4, default 2) — how many candidates per refinement round
   - **Total evaluator budget** (5–60, default 12) — hard cap on Haiku API calls
5. Tap **Run Research**
6. Live progress sheet appears: chains spawning, scores arriving, refinement rounds
7. After ~15–30 seconds, the result sheet:
   - 🏆 Best result with score (e.g. 0.87/1.00)
   - Stats: API calls + cost (typically $0.005–0.02)
   - **Search trajectory** disclosure: chains × rounds grid
   - All attempts disclosure with the runners-up
   - "Send as Message" button drops the best result into your chat input

---

## Cost Reality Check

A default native run = 3 chains + 2 refinement rounds × 2 candidates = ~9 proposal calls + 3 scoring calls = ~12 Haiku 4.5 calls.

At Haiku pricing, that's roughly **$0.005–0.02 per /research run**. For comparison, a single Opus chat turn is often $0.05–0.20. Native research is cheaper than one regular Opus reply, and you get many candidate solutions instead of one.

You can dial up the knobs for harder tasks:
- 6 chains × depth 3 × K=4 × budget 60 → ~$0.05 per run, much wider exploration

---

## SimpleTES Mode (when you need it)

Switch the mode picker to **SimpleTES** in the card and the wizard:
1. Asks Haiku to translate your natural-language task into `init_program.py` + `evaluator.py`
2. Shows you the generated Python files for review
3. On confirm: writes them via SSH to `~/.simpletes/tasks/<timestamp>/`
4. Dispatches `uv run python main.py …` as a background job
5. Real Python evaluator runs on your Mac — could be timing code, scoring against a benchmark, fitting a curve, etc.

This was the original Research mode (see [research-mode tutorial](research-mode.md)). It still works exactly as before.

---

## Trajectory Graph (both modes)

After the result lands, tap into the result sheet → expand **Search trajectory**.

You see a 2D grid: chains as columns, rounds as rows. Each cell shows that attempt's score, color-coded:
- 🟢 green ≥ 0.7 (strong)
- 🟠 orange 0.4–0.7 (decent)
- 🔴 red < 0.4 (weak)

Best attempt overall is highlighted with a gold border + ★. Tap any cell to see that attempt's full content + the scorer's rationale.

Useful for:
- Sanity-checking the search actually explored breadth (not 3 nearly-identical chains)
- Cherry-picking a runner-up if you prefer it over the judge's pick
- Understanding *why* the winner won

---

## When to use `/research` vs Search Mode

- **`/research <task>`** = one-off explicit search for a specific question. Card → knobs → Run.
- **Search Mode** (`/search on`) = chatroom-wide setting where every message is a search. Faster cadence, no card per send.

Use `/research` for occasional deep dives. Use Search Mode for an ideation chatroom where everything you type benefits from breadth.

---

## Gotchas

- Native mode requires an Anthropic API key configured in Settings. Without one, the card surfaces a "configure API key" error.
- Cost grows roughly linearly with `total_budget`. Don't crank it to 60 unless you really want broader exploration.
- Native mode can't actually run code — if your task needs measurable results (timing, coverage, output correctness), use SimpleTES.
- The judge is also Haiku, which means it has the same biases as the proposer. SimpleTES with a real evaluator avoids this — Native mode's scoring is "another LLM's opinion."
