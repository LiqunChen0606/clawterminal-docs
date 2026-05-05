# Skill Extraction — Examples

> Practical workflows for turning trajectories into reusable Skills. All examples assume you have one of: an Anthropic API key in Settings, Apple Intelligence enabled on iOS 26 (iPhone 15 Pro and newer, M1+ iPads), or Claude CLI installed on a connected Mac.

---

## Example 1: Save an Architecture Decision as a RULE

**Setup.** You ran `?Should our iOS app store user sessions as JWTs in Keychain or as server-side cookies in CloudKit private database?` and the result sheet is open.

**Steps.**

1. In the result sheet, tap **Select** (toolbar checkmark, secondary action).
2. Tap the winning attempt (gold-bordered, ★) plus the runner-up that lost on a single trade-off — that gives the skill both the "right answer" and the rationale for *why* the other approach was rejected.
3. Tap **Save as Skill (2)**.
4. Confirm sheet appears. Suggested name might be `iOS Session Storage`. Rename to `auth-session-storage`. Keywords might be `session, jwt, keychain, ios, auth` — add `cloudkit` if it's missing.
5. Save.

**Result.** Next time you ask anything about session lifecycle, logout, refresh, or token expiry, the skill loads automatically and tells Claude: "Use JWTs in iOS Keychain over CloudKit cookies because…" — saving you from re-litigating the choice.

---

## Example 2: Save a PR-Creation Recipe from a Background Job

**Setup.** A `/submit Open PR for the auth refactor with reviewers` job finished successfully. The trajectory shows ~12 tool calls including a few false starts.

**Steps.**

1. Open **Jobs** tab → tap the finished job → scroll to the **Trajectory** card.
2. Tap **Select** in the trajectory card header.
3. Select only the steps that did real work, in order:
   - `git checkout -b feat/auth-refactor`
   - `git push -u origin feat/auth-refactor`
   - `gh pr create --title ... --body ...`
   - `gh pr edit --add-reviewer @teammate1 --add-reviewer @teammate2`
   - `gh pr edit --milestone "v1.5"`
4. Skip the diagnostic `git status`, the `git log`, and the failed `gh pr create` that hit a missing label.
5. Tap **Save (5)**.
6. Suggested name: `Open PR with Reviewers`. Rename if you want. Keywords: `pr, github, gh, reviewers, milestone`. Save.

**Result.** Next time you ask "open a PR for this branch with the usual reviewers and milestone", the saved RECIPE injects the exact 5-step sequence as guidance — Claude doesn't have to rediscover the right `gh` flags or remember the convention.

---

## Example 3: Save a Naming Convention from a Search Mode Run

**Setup.** You used `?What's a good name for the new background extractor — needs to be searchable, ≤2 words, and not collide with existing names like NativeResearchEngine?` and got 3 strong candidates with scores.

**Steps.**

1. Open the result sheet → **Select**.
2. Tap the winner (`SkillExtractor`) AND the rationale-rich runner-up that explained the naming convention rule (`SkillSynthesizer`). The runner-up's rationale is what makes the skill reusable for *future* naming questions.
3. Save as Skill. Rename to `Naming Conventions`. Keywords: `name, naming, file, class, struct, swift`. Save.

**Result.** Future naming decisions in this codebase get grounded by your established convention — no drift across sessions.

---

## Example 4: Save a Code-Review Heuristic from a `/team` Run

**Setup.** A `/team Review the auth refactor for security issues` run produced three agent results. The Reviewer agent's output had a particularly sharp critique of session-handling.

**Steps.**

1. Open the Jobs tab → find the Reviewer job in the team group → open it.
2. Trajectory card → **Select** → tap the steps where the Reviewer flagged issues with file/line citations.
3. Save (3-4 steps). The skill comes out as a RECIPE because of the trajectory shape, but the *content* will read like a checklist — "When reviewing auth code, check: (a) token storage location, (b) session expiry handling, (c) refresh-token rotation, (d) logout cleanup."
4. Rename to `Auth Review Checklist`. Keywords: `auth, review, security, session, token`. Save.

**Result.** Every future code review of auth-adjacent code gets the same checklist applied — making team standards portable across sessions.

---

## Example 5: When NOT to Extract

A trajectory is *not* always worth saving as a skill. Some signals that say "don't extract":

- **Score on the winner is below ~0.6.** The conclusion is shaky; saving it as a skill calcifies a weak answer.
- **The selection is one-off.** "How do I rename this specific file" doesn't generalize. Pin it to the chatroom (Trajectory Library) instead, or just rely on memory.
- **The rationale is missing.** If the winning attempt has no rationale and you can't articulate *why* it won, the skill won't be useful — Claude needs the reasoning to apply it next time.
- **The trajectory is debugging churn.** A long sequence of "try this, no it broke, try that" doesn't compress into a skill. Save the *fix* as a /tribal entry instead.

If in doubt, save it anyway and prune later. The cost is low (1 file in `~/.catclaw/skills/`) and you can always disable via Settings → Skills if it never fires usefully.

---

## Example 6: Auditing Saved Skills

After 2-3 weeks of extracting, your Skills list will have grown. Curate it.

1. Open **Settings → Skills**.
2. Sort by "last fired" (or skim the list and ignore skills with `Last used: never`).
3. For each rarely-fired skill: ask whether the keywords are too narrow (skill never matches), or the skill's *content* didn't actually help when it did fire.
4. Disable (un-checkmark) skills that aren't pulling weight. Or delete them.

A small, sharp library of 5-10 well-keyworded skills outperforms a dump of 30 mediocre ones.

---

## Example 7: API Key vs CLI Path Comparison

Same input, two paths:

| Step | API key path (Settings → API Key) | CLI fallback (no API key, Mac connected via SSH) |
| ---- | --------------------------------- | ------------------------------------------------ |
| 1. Tap Save as Skill | Spinner appears | "Extracting skill via Claude CLI — 30-60s" inline status row |
| 2. Wait | 3-5 seconds | 30-60 seconds (background job in tmux) |
| 3. Confirm sheet | Auto-presents | Auto-presents |
| 4. Save | Same | Same |

**Output is identical** — the prompt and JSON parsing are shared between paths. The only differences are speed and the `[Skill Extraction — CLI fallback]`-marked job that appears (briefly) in the Jobs tab while the CLI is doing its thing.

If you're a heavy extractor, the API key path is worth its ~$0.001 per call. If you're a light user without a key, the CLI fallback works fine.

---

## See Also

- [Skill Extraction tutorial](../tutorials/skill-extraction.md) — the step-by-step guide.
- [Memory & Skills](memory-skills.md) — broader picture of the skills system.
- [Trajectory Library](trajectory-library.md) — pinning vs extracting (different uses for the same trajectory).
- [Native Research](native-research.md) and [Search Mode](search-mode.md) — the engines that produce extractable trajectories.
