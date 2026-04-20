# What If Simulator — Tutorial

> Before you run `rm -rf node_modules`, `/whatif` asks Claude to predict what will actually happen. Three likely outcomes, color-coded by severity, with a probability on each. Decide before you commit.

---

## What It Does

`/whatif <command>` sends a proposed shell command to Haiku along with your current git status. Haiku returns its three most likely predicted outcomes:

- **Green bar** — low-severity, expected behavior (happy path)
- **Yellow bar** — medium-severity, something will change that you should be aware of
- **Red bar** — high-severity, irreversible or damaging outcome

Each outcome has:
- A one-line description of what happens
- A probability percentage
- A short "why this might happen" tooltip

You see the prediction card, then decide: **Run Anyway** or **Cancel**.

---

## The Core Use Case

You are about to run something and you are not 100% sure what it will do.

```
/whatif git reset --hard HEAD~3
```

Card appears:

```
Outcome Prediction — git reset --hard HEAD~3

🟢  80% — Branch tip moves back 3 commits. Working directory matches.
          Untracked files preserved.
🟡  15% — You have uncommitted changes (git status shows 2 modified files).
          Those will be destroyed.
🔴   5% — If HEAD~3 is before your last push, you will need --force to push
          later, which will break collaborators' local copies.

                    [ Cancel ]  [ Run Anyway ]
```

Three seconds to read. Uncommitted changes spotted. You cancel, commit, then re-run without the 15% risk.

---

## When to Use It

- Any destructive command (`rm`, `git reset --hard`, `DROP TABLE`, `kill -9`)
- Anything with a flag you are unsure about (`--force`, `--no-verify`, `--prune`)
- Long pipelines you wrote from memory (`find ... | xargs ... | sort ...`)
- SQL you are about to paste into a production shell
- Migration scripts on databases that have real data
- Anything involving `sudo` on a server you care about

Basically: any command where "oops" would cost you more than 30 seconds.

---

## How the Prediction Works

`/whatif` runs locally-light + AI-heavy:

1. **Local collection** — CatClaw runs `git status`, `git log -1`, and captures your current working directory's file tree summary via SSH. Takes ~300ms.
2. **Haiku prompt** — the proposed command plus the collected context is sent to Claude Haiku with a specific instruction to return "three most likely outcomes ranked by severity, with probability and one-line description."
3. **Render** — the card renders with color-coded bars. Green / Yellow / Red are picked based on Haiku's severity classification.

Total latency: 2–4 seconds. Cost: ~$0.002 per call.

---

## Real Examples

### Example 1 — The classic

```
/whatif rm -rf ~/Downloads/project-backup
```

```
🟢  70% — Directory and all contents deleted. 2.4 GB freed.
🟡  25% — If you meant ~/Downloads/project-backups (with 's'), you just
          deleted your only backup.
🔴   5% — If this directory is symlinked elsewhere, the symlink target
          will also be gone.
```

### Example 2 — Force push

```
/whatif git push origin main --force
```

```
🟢  40% — Your local branch overwrites remote. Any commits on remote
          that you don't have locally are lost.
🟡  35% — If teammates have pulled the remote, their local branches
          now diverge. They will need to reset or rebase.
🔴  25% — If CI/CD is deploying from main, the force push may cause a
          failed deploy or revert production to an earlier state.
```

This is the kind of card that makes you close your laptop and ask on Slack first.

### Example 3 — Dangerous-looking but actually safe

```
/whatif find . -name "*.tmp" -delete
```

```
🟢  85% — All .tmp files in the current directory tree deleted.
          Expected behavior.
🟡  10% — If you have .tmp files you intentionally kept (e.g. in a
          test fixture directory), they will also be removed.
🔴   5% — If there are .tmp symlinks pointing outside the tree, the
          link is removed but the target is preserved.
```

Read the card, realize all three outcomes are fine, run with confidence.

---

## Integration with Git Status

The git status context matters a lot. Running `/whatif git checkout main` looks harmless when your working directory is clean, but the yellow bar escalates to red if you have uncommitted changes. The AI sees both pieces and weighs appropriately.

This is why `/whatif` is worth running even for commands you "know" are safe — your current state changes the prediction.

---

## Tips

- **Run it for any command you have not typed this month.** Muscle memory is unreliable. Fresh reasoning beats habit.
- **Cancel freely.** The point is to surface the 15% you had not considered. If you cancel 9 out of 10 `/whatif` runs that is *good* — you just avoided a foot-gun.
- **Does not replace dry-run flags.** For commands that support `--dry-run` or `-n`, use those too. `/whatif` is *one* line of defense, not the only one.
- **Works with pipelines.** Paste the whole pipe in quotes: `/whatif "find . -name '*.log' | xargs rm"`.
- **Combine with `/plan`.** `/plan` is for AI-proposed changes. `/whatif` is for shell commands you typed. Different surface area, same spirit.

`/whatif` is a three-second pause that has saved more data than all your backups combined.
