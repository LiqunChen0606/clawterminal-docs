# Tutorial: GitHub PR Workflow with `/pr`

ClawTerminal's `/pr` commands give you a full pull request lifecycle from your phone: create PRs with AI-generated titles and bodies, run AI-powered code reviews with severity-coded items, check CI status, and teach the reviewer your team's preferences over time. This tutorial covers setup through advanced use.

---

## Prerequisites: Setting Up `gh` CLI

The `/pr` commands run `gh` (GitHub CLI) over SSH on your Mac. You need to install and authenticate it once.

### Step 1: Install `gh` on your Mac

```bash
brew install gh
```

Verify the installation:

```bash
gh --version
# Expected: gh version 2.x.x (date)
```

### Step 2: Authenticate

```bash
gh auth login
```

Follow the prompts — the browser OAuth flow takes about 30 seconds. Choose **GitHub.com** → **HTTPS** → authenticate in the browser. When you return to the terminal, you should see:

```
✓ Authentication complete.
✓ Configured git protocol
✓ Logged in as your-github-username
```

### Step 3: Verify

```bash
gh pr list
# Should return a list (or "no open pull requests")
```

That's it. You never need to reconfigure this — `gh` stores the token in your Mac's keychain.

---

## Part 1: Creating a Pull Request

### Basic PR creation

```
/pr
```

or

```
/pr create
```

ClawTerminal will:

1. Run `git diff main...HEAD` in the current project directory to get the changes
2. Send the diff to Claude with a prompt like: "Generate a clear PR title and body for these changes, following conventional PR formatting"
3. Run `gh pr create --title "..." --body "..."` on your Mac
4. Post the new PR URL to your chatroom

The title and body Claude generates are intentionally terse and factual — it reads the diff, not your mind. If you want to add context (linked issues, screenshots, special notes), edit the PR on GitHub after creation.

### When to use `/pr` vs. creating PRs on GitHub

`/pr` is fastest when:
- The change is self-explanatory from the diff
- You're on your commute and want to send the PR before a meeting
- You're iterating quickly and creating many small, focused PRs

Create PRs on GitHub directly when:
- The PR needs screenshots, architectural diagrams, or linked issues
- You want to fill in a team PR template with structured fields
- You're opening a draft PR for early feedback before the work is done

---

## Part 2: AI Code Review

### Running a review

```
/pr review 42
```

ClawTerminal fetches the diff for PR #42 via `gh pr diff 42`, then sends it to Claude with detailed instructions to review the code for bugs, style, security, and test coverage. Results appear as color-coded cards:

### Understanding review severity colors

| Color | Severity | When it appears |
|-------|----------|-----------------|
| Red | Bug | Potential functional defect, security vulnerability, or crash |
| Yellow | Suggestion | Code quality improvement, clarity, refactoring opportunity |
| Blue | Question | Clarification needed — the reviewer doesn't have enough context |
| Green | Praise | Pattern worth preserving or highlighting for the team |

A typical review for a medium-sized PR (100–300 lines changed) produces 5–12 items. Large refactors can produce 20+.

### Posting the review to GitHub

Tap **Post Review** at the bottom of the review panel to submit the AI-generated review as a comment on the PR via `gh pr review --comment`. The comment is attributed to your GitHub account.

This is optional — many users prefer to read the review privately first, take notes on what to fix, and only post after they've made corrections.

---

## Part 3: Review Learning

This is where `/pr review` gets more powerful over time.

### How the learning works

Every review item has two small buttons:

- **Thumbs up** — "This is exactly the kind of thing I want flagged. Keep finding these."
- **Thumbs down** — "This isn't useful for our project. De-prioritize in future reviews."

CatClaw stores your ratings per-chatroom. When you run `/pr review` again in the same chatroom, the ratings are injected into the review prompt: "The user has marked X-type findings as high-priority, Y-type findings as low-priority."

Over 5–10 reviews, the reviewer adapts meaningfully to your team's style.

### What to rate

Rate liberally, especially in early reviews:

**Thumbs down examples:**
- "This method could be renamed to be more descriptive" — if your team doesn't care about micro-naming
- "Consider adding a comment here" — if your team prefers self-documenting code over comments
- "This duplicates logic from X" — if X is in a different module that's intentionally separate

**Thumbs up examples:**
- "This SQL query is vulnerable to injection" — always relevant
- "This function has no null check and will crash if the argument is nil" — always relevant
- "This async function is not being awaited" — always relevant for async code

### Setting focus areas

Focus areas tell the reviewer which dimensions to emphasize, independent of learned ratings:

```
/pr focus security,tests
```

Now every review in this chatroom weights security issues and test coverage more heavily than other areas.

Available focus areas (you can combine any of these):

| Area | What it covers |
|------|---------------|
| `security` | SQL injection, XSS, auth bypasses, input validation, secrets in code |
| `tests` | Missing test cases, edge cases not covered, test quality |
| `performance` | N+1 queries, unnecessary allocations, blocking calls, slow algorithms |
| `documentation` | Missing docstrings, unclear variable names, undocumented parameters |
| `error-handling` | Uncaught exceptions, silent failures, incomplete error paths |
| `null-safety` | Null pointer risks, optional unwrapping without checks |
| `accessibility` | Missing aria labels, contrast issues, keyboard navigation |
| `types` | Type safety, missing type annotations, unsafe casts |

You can specify multiple areas:

```
/pr focus security,error-handling,null-safety
```

Reset to defaults (all areas equal weight):

```
/pr focus
```

### A realistic learning workflow

```
# Session 1 — first PR review, very noisy
/pr review 15
# 14 items. Rate: thumbs down on 5 style nits and 2 micro-naming comments.
# Thumbs up on the SQL injection warning and the missing await.

# Session 2 — somewhat better
/pr review 22
# 9 items. The style nits are gone. A new test-coverage item appeared.
# Rate: thumbs up on the test-coverage flag.

# Session 3 — feeling tuned
/pr review 28
# 7 items: 1 bug, 2 error-handling gaps, 2 test coverage items, 2 suggestions.
# This matches your team's actual priorities.
```

After ~5 reviews, most teams find the review quality comparable to a thoughtful senior engineer who knows the codebase.

---

## Part 4: Listing PRs and Checking CI

### List open PRs

```
/pr list
```

Shows all open PRs in the repo with: number, title, author, age, and current draft/ready status. Tap any result to get the PR number for a review or checks command.

### Check CI status

```
/pr checks 42
```

Shows all CI checks and their status:

```
PR #42 CI Status

  ✅  build (ubuntu-latest)         completed  2m 14s
  ✅  test-unit (ubuntu-latest)     completed  4m 38s
  ⏳  test-integration (ubuntu)     running    1m 22s elapsed
  ❌  lint (ubuntu-latest)          failed     45s
  ✅  type-check (ubuntu-latest)    completed  1m 02s
```

Useful for a quick green/red check before merging, especially when you don't want to switch to GitHub in a browser.

---

## Part 5: Full PR Workflow — End to End

Here's a complete workflow from a phone, covering the full lifecycle of a feature PR:

### Morning standup (phone)

```
# See what PRs are waiting
/pr list

# Check if the CI on your own PR is green
/pr checks 38
```

### Commute review

```
# Review a teammate's PR on your way in
/pr review 41

# Rate items:
# - Thumbs up on the missing input validation
# - Thumbs down on the variable naming suggestion (your team uses that convention)
```

### In-office, creating your own PR

```
# After committing your feature work:
/pr create

# PR URL appears in chatroom. Open on GitHub to add any screenshots or linked issues.
```

### Before merging

```
# Check CI one more time
/pr checks 38

# If it's all green, merge via GitHub or:
# (not a slash command — open GitHub for the actual merge)
```

---

## Part 6: Tips and Gotchas

### The review is as good as your project directory

The chatroom's **Info → Project Directory** must point to a directory that is:
- A git repository
- Connected to GitHub (not just a local-only repo)
- On the branch you want to review

If `/pr list` returns "not a git repository" or shows the wrong repo, update the Project Directory in the chatroom's Info panel.

### `gh` auth tokens expire

If you get authentication errors, re-run `gh auth login` on your Mac. GitHub tokens created via the `gh` OAuth flow have long expiry but do eventually expire.

### First review is always noisier

Claude doesn't know your project's conventions, team style, or what categories matter to you before the first review. Invest in rating items on the first 2–3 reviews — the learning pays off quickly.

### Focus on bugs first

If you only have time for one review pass, set `/pr focus security,error-handling` and thumb-down everything else. Over time, the reviewer becomes a reliable bug-finder rather than a style-nagger.

### Use different chatrooms for different repos

Review learning is stored per-chatroom, not per-repo. If you maintain multiple repos with different conventions (e.g., a strict TypeScript frontend and a more relaxed Python backend), use a different chatroom for each. The reviewer will learn independently for each context.

---

## Requirements

- `gh` CLI installed and authenticated on your Mac
- A git repository in the project directory
- The repository must be on GitHub (not GitLab or Bitbucket)
- Active SSH connection to your Mac

See the [examples/pr-workflow.md](../examples/pr-workflow.md) for copy-paste-ready examples.
