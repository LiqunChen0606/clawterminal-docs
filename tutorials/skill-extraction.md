# Skill Extraction from Trajectories — Tutorial

> Every research run, every batch of agent work, every `?prefix` exploration produces a *trajectory* — a sequence of attempts, scores, and tool calls. Most of that work disappears into history. Skill Extraction turns the parts that *matter* into reusable Skills that auto-load whenever a similar request comes up.

---

## Part 1: What It Is

A Skill in CatClaw is a small markdown document that gets injected into Claude's preamble when its keywords match what you're working on. Built-in skills cover things like "writing tests with TDD" or "Swift concurrency style"; you can also write your own.

Skill Extraction is a button. You select a few entries from a trajectory — the winning attempts in a research run, the key tool calls in a batch job — and CatClaw asks Haiku to summarize the selection into a clean Skill. You review the draft, edit anything, and save.

The result is a focused, replayable artifact. The next time you ask a question whose keywords overlap, Claude sees the Skill and follows its guidance.

---

## Part 2: Two Trajectory Types, Two Skill Shapes

CatClaw classifies your selection automatically into one of two shapes:

- **RULE** — for selections drawn from `/research` results or `?prefix` Search winners. Reasoning, scoring, multi-option deliberation. The output reads "When *trigger*, prefer *approach* because *reason*. Avoid *pitfall*. Example: *one short concrete case*."
- **RECIPE** — for selections drawn from a job's trajectory (Tool Use → Tool Result → Tool Use…). A numbered list of 3–8 steps with the exact tool / command names.

You don't have to think about which one to pick — the extractor reads the trajectory shape and decides.

---

## Part 3: Extracting from a Research Result

You ran `/research` (or sent a `?prefix` question) and the result sheet is open.

1. Tap **Select** in the toolbar (the checkmark icon, secondary action).
2. The trajectory graph cells become tappable selection chips. Tap 2–4 of the most useful attempts — usually the highest-scored one plus its rationale-rich parents/siblings.
3. Tap **Save as Skill (N)** in the toolbar (purple sparkles icon).
4. Wait for the spinner. With an Anthropic API key the wait is 3–5 seconds; without one (CLI fallback) it's 30–60 seconds and you'll see a status row inline.
5. The confirmation sheet pops up. Read the suggested name + description, scan the keywords (3–6 lowercase tokens — these are what triggers the skill later), and skim the markdown body.
6. Edit anything that's off. The most common tweak: rename the skill to something specific to your domain. The auto-generated name is fine but rarely punchy.
7. Tap **Save**.

A confirmation message lands in your chat: *✨ Saved skill: **<name>** — keywords: …*. The skill is now persistent across all chatrooms in the app.

---

## Part 4: Extracting from a Background Job

Same idea, different entry point.

1. Open the **Jobs** tab. Tap any finished job that has a trajectory (any `/submit`, `/batch`, `/team`, `/orchestrate`, `/race`, or `?prefix` job).
2. Scroll to the **Trajectory** card.
3. Tap **Select** in the card header.
4. Tap 2–N steps that capture the recipe. Usually that means: the tool calls that did the *real* work (file edits, command runs), skipping diagnostic calls and dead-ends.
5. Tap **Save (N)**.
6. Same review-and-edit confirmation sheet. The output will be a numbered RECIPE.

For example, after a successful PR-creation job you might select the steps `git checkout -b`, `gh pr create`, `add reviewers`, and `set milestone`. The extracted skill becomes a 4-step RECIPE that the next agent run can reuse without rediscovering the workflow.

---

## Part 5: With and Without an API Key

Skill Extraction works in two modes:

| Setup | Path | Speed | Notes |
| ----- | ---- | ----- | ----- |
| Anthropic API key in Settings | Direct Haiku call | 3–5 seconds | Fastest. Costs ~$0.001 per extraction. |
| No API key (Claude CLI on Mac via SSH) | Background job through tmux | 30–60 seconds | Same prompt, same JSON, same final skill. Status row keeps you informed while the job runs. |

If you have neither, extraction is unavailable — you'll see an error explaining you need either Settings → API Key or Claude CLI installed on your connected Mac.

---

## Part 6: How Saved Skills Are Triggered

Saved skills use **progressive disclosure** by default — they don't load on every message. They load only when the keywords from your saved skill match keywords in the message you're about to send.

That means:

- Pick keywords that capture the *trigger* — what kind of question would make this skill helpful? "auth", "session", "jwt", "logout" are good keywords for an auth-architecture skill. "good", "improvement" are not.
- 3–6 keywords is the sweet spot. Too few and the skill rarely fires. Too many and it fires for unrelated questions, polluting the preamble.
- You can edit a saved skill's keywords later from **Settings → Skills**.

When a skill fires, you'll see it logged in the **Skills** chip in the chatroom mode banner.

---

## Part 7: Tips

- **Extract while the context is hot.** The best skills come from trajectories you just finished — you remember which attempts mattered. Extract before closing the sheet.
- **One skill per insight.** Don't try to cram three lessons into one skill. Make each skill narrow and the keywords sharp.
- **Edit the auto-generated name.** Haiku's first naming pass is often verbose. Rename to a 1–3 word noun phrase.
- **Don't extract noise.** If the trajectory was a wandering exploration with no clear lesson, there's nothing useful to save. Move on.
- **Audit your skills.** After a few weeks, open **Settings → Skills** and prune any that haven't fired in a while or whose keywords are too broad.

---

## Part 8: Verifying It Worked

After saving, you can verify the new skill is wired up:

1. Open **Settings → Skills**. Your new skill should appear in the list with the green ✓ enabled checkmark.
2. Start a new message that includes one of the skill's keywords. Send it.
3. Check the **Skills** chip in the mode banner. The chip count should include your new skill.
4. (Optional) Send `/think let me check the auth approach` to see Claude's reasoning trace — if your skill loaded, its content shows up in the early thinking.

---

## See Also

- [Trajectory Library](trajectory-library.md) — pin a research winner directly into the chatroom preamble (different from saving a skill).
- [Native Research](native-research.md) — the `/research` engine that produces extractable trajectories.
- [Search Mode](search-mode.md) — the `?prefix` engine that produces extractable trajectories.
- [Background Jobs](../examples/background-jobs.md) — recipes for trajectory-rich job workflows.
