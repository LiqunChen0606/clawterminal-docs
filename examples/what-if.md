# What If Simulator

> `/whatif <command>` predicts the three most likely outcomes of a shell command before you run it. Color-coded bars, probability %, one-line descriptions. Then you decide: Run Anyway or Cancel.

## Basic usage

```
/whatif rm -rf node_modules
```

Card appears:

```
Outcome Prediction — rm -rf node_modules

🟢  85% — node_modules directory and all contents deleted.
          Disk space freed (~400 MB typical).
🟡  10% — If you have symlinked packages, the symlinks are removed
          but the targets are preserved.
🔴   5% — If this is a monorepo root, you may delete more than
          intended. Check package.json workspaces.

                    [ Cancel ]  [ Run Anyway ]
```

---

## Dangerous examples

### Force push

```
/whatif git push origin main --force
```

```
🟢  40% — Local branch overwrites remote. Clean for you.
🟡  35% — Teammates who pulled main now have divergent branches
          and will need to reset or rebase.
🔴  25% — If CI/CD deploys from main, the force push may cause a
          failed deploy or revert production to an earlier state.
```

### Hard reset

```
/whatif git reset --hard HEAD~3
```

```
🟢  80% — Branch tip moves back 3 commits. Working tree matches.
🟡  15% — You have 2 modified files in working directory that will
          be destroyed.
🔴   5% — If HEAD~3 is before your last push, force-push required
          later which breaks teammates.
```

### Database operation

```
/whatif DROP TABLE users
```

```
🟢   5% — Success only if you're in a scratch database.
🟡  20% — Drops the table and all rows; any app connected will
          error on the next query.
🔴  75% — If this is production or staging, data is gone. Restoring
          from backup is the only option.
```

---

## Safe-looking command that surfaces a hidden risk

```
/whatif git checkout feature/auth-refactor
```

```
🟢  60% — Clean switch to the feature branch.
🟡  35% — Your current branch has 3 uncommitted files that conflict
          with the target branch. Git will refuse and show an error.
🔴   5% — If the target branch has destructive changes to your
          working files, you may lose local edits.
```

Without `/whatif` you would have just run `checkout`, gotten the error, wasted time figuring out what was wrong. With it, the yellow bar told you up front.

---

## How the prediction is built

1. CatClaw runs `git status`, `git log -1`, and captures working directory state via SSH (~300ms)
2. Sends the command + context to Haiku with a "predict 3 outcomes" prompt
3. Haiku returns severity + probability + description for each
4. Card renders with color-coded bars

Total latency: 2–4 seconds. Cost: ~$0.002 per call.

---

## Pipelines

Wrap in quotes:

```
/whatif "find . -name '*.log' | xargs rm"
```

Works for any complex pipeline — the simulator reasons about the whole chain.

---

## When to use it

- Any destructive command (`rm`, `git reset --hard`, `DROP`, `kill -9`)
- Anything with `--force`, `--no-verify`, or `--prune`
- Long pipelines written from memory
- SQL pasted into production shells
- Migration scripts against real data
- Any `sudo` on a server you care about

---

## Tips

- **Cancel freely.** The goal is surfacing the 15% you had not considered. Cancelling 9/10 `/whatif` runs is a good outcome.
- **Combine with `--dry-run`.** Use native dry-run where supported; `/whatif` adds a second line of defense.
- **Pair with `/plan` for AI-proposed changes.** `/whatif` is for shell commands you typed. `/plan` is for what Claude wants to do. Different surface areas.
- **Context matters.** Uncommitted changes change the prediction. Run `/whatif` even for commands you "know" are safe.
