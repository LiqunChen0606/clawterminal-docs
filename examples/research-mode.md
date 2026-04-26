# Research Mode — Examples

> Realistic scenarios for `/research` — the slash command that turns a natural-language goal into a SimpleTES propose-evaluate-refine search on your Mac. Pair with `/notifier install` so you find out the moment the search finishes, and `/simpletes install` so the search engine is actually there.

---

## Scenario 1: Finding a Faster Sort for Specific Data

You have a real workload: lists of ~10,000 integers that are usually 90% sorted (recently appended timestamps, mostly-monotonic IDs, that kind of thing). The standard library `sorted()` is fine, but you want to know if a hand-rolled approach could be measurably faster on this distribution.

**Task you'd type:**

```
/research find a Python sort that runs faster than sorted() on lists of 10000 integers where 95% of elements are already in their final position. Score candidates by total wall-clock time across 100 random fixtures. The evaluator should compare against sorted() and report the speedup ratio.
```

**What you'd get back:**

- An `init_program.py` with a baseline implementation (probably plain `sorted()` or insertion sort)
- An `evaluator.py` that generates 100 nearly-sorted fixtures, times the candidate, and returns the speedup ratio over `sorted()`
- The Run sheet with three knobs

**What to tweak:** bump `--total-budget` to 60 if you want SimpleTES to explore more candidates. The default 30 is enough for a first pass to see if the approach works at all.

**Why this works well in Research Mode:** the evaluator is mechanical (you can write it in 20 lines), the search space is large (every variation on Timsort, insertion sort, gallop merge), and "best" is unambiguous (lower wall-clock wins).

---

## Scenario 2: Tuning a Hyperparameter Without a Grid Search

You have a small ML model — a fraud-detection classifier, say — and three hyperparameters that interact non-obviously. Grid search is too expensive. You want a directed search that proposes new hyperparameter triples based on what scored well.

**Task you'd type:**

```
/research find the best (learning_rate, dropout, hidden_dim) tuple for a 3-layer MLP fraud classifier. learning_rate in [1e-5, 1e-1], dropout in [0.0, 0.5], hidden_dim in {64, 128, 256, 512}. Train 5 epochs on the bundled training set and score by F1 on the validation set. Higher is better.
```

**What you'd get back:**

- An `init_program.py` that defines the model class and exposes a `train_and_score(lr, dropout, hidden_dim)` function
- An `evaluator.py` that calls `train_and_score(...)` on a few sensible starting points and returns the F1 score
- SimpleTES will propose new triples based on which combinations scored well

**Knob suggestion:** drop `--k-candidates` to 2 (each one trains a model, so each is expensive) but raise `--total-budget` to 50 so you get more rounds.

**Why this works well in Research Mode:** the evaluator returns a single number you trust, the parameter space is too large for a grid sweep, and you want overnight unattended execution — exactly what `/notifier install` plus a SimpleTES background job is designed for.

---

## Scenario 3: Finding a Better Query Plan

Your slow query is killing dashboard load times. You suspect the query planner is choosing a bad join order or missing an index opportunity. You want to enumerate plausible rewrites and benchmark each one.

**Task you'd type:**

```
/research find a Postgres query rewrite that runs faster than the current `SELECT u.id, COUNT(o.id) FROM users u LEFT JOIN orders o ON o.user_id = u.id WHERE u.created_at > NOW() - INTERVAL '30 days' GROUP BY u.id`. Score candidates by EXPLAIN ANALYZE execution time on a snapshot of the production schema (provided in test_db.sql). Lower is better. Functional equivalence is mandatory — same result rows for any valid users/orders fixture.
```

**What you'd get back:**

- An `init_program.py` that runs the original query and captures the result set + timing
- An `evaluator.py` that loads the schema snapshot, runs the candidate, validates the result rows match the original, and returns negative execution time (so higher is better)
- The Run sheet — you can tweak the budget upward; query rewrites are cheap to evaluate

**Why this works well in Research Mode:** the search space is well-defined (subquery vs. JOIN vs. CTE vs. window function), the evaluator catches incorrect rewrites automatically, and you'd otherwise have to try each rewrite manually.

---

## Scenario 4: Compressing a Bloom Filter Implementation

You have a working bloom filter in Python and want a smaller / cleaner implementation that maintains the same false-positive rate.

**Task you'd type:**

```
/research find a smaller Python bloom filter implementation with the same false-positive rate as the current one (~1% at 10,000 elements). Score candidates by total bytes of the candidate's source code. The evaluator should also assert the false-positive rate is within 0.5% of the baseline across 1000 random membership tests. Lower bytes wins, but reject any candidate that fails the FPR check.
```

**What you'd get back:**

- An `init_program.py` with the existing implementation
- An `evaluator.py` that measures source byte count and validates FPR against a baseline
- SimpleTES will explore minification, bit-packing variants, alternative hash functions

**Tip:** this is a great use case for higher `--num-chains` (4 or 5) — different chains will explore different minification strategies in parallel.

---

## Tips That Apply to Every Task

- **Make the evaluator strict.** "Higher is better" is fine, but add validation. A bloom filter that returns "not in set" for everything has perfect compression but is broken. Reject broken candidates explicitly.
- **Provide concrete numbers.** "10,000 integers" is better than "a list of integers." "~1% false-positive rate" is better than "a similar false-positive rate."
- **State what 'better' means.** "Faster" is ambiguous. "Lower wall-clock time on these 100 fixtures" is unambiguous.
- **Start with default knobs.** If results look meaningful, re-run with double the budget. If results look like noise, fix the task description first; throwing more budget at a bad task wastes money.
- **Read both files.** The card shows you `init_program.py` and `evaluator.py` for a reason. If Haiku misunderstood the goal, those files will reveal it before you spend a single SimpleTES round.

---

## When NOT to Use Research Mode

Research Mode is built for searches with a quantitative evaluator. It's the wrong tool when:

- You want a one-off explanation or fix — just chat with Claude normally
- "Better" is qualitative ("more readable", "more idiomatic") and you can't write an evaluator for it
- The candidate evaluation is so expensive that even a small budget would take days
- You don't actually want to run the candidates — you just want to read about approaches

For any of those, use a normal chatroom (with `/effort high` if you want depth). Save `/research` for when "search" is genuinely the right verb.

---

## Quick Sanity Checks Before Running

1. Did `/simpletes status` confirm the install? If not, `/research` will dispatch a job that fails immediately.
2. Did `/notifier status` show the daemon running? If not, your job will finish silently and you'll wonder.
3. Does the `init_program.py` actually run (in your head, reading it)? If not, fix the task description.
4. Does the `evaluator.py` return a number? If it returns `None` or throws, every candidate gets a meaningless score.
5. Are your `ANTHROPIC_API_KEY` / other LLM keys exported in `.zshrc` / `.bashrc`? SimpleTES inherits them through the `${SHELL:-/bin/bash} -lic '...'` wrapper.

If all five look good, tap **Run SimpleTES** and walk away. The notifier will tell you when it's done.
