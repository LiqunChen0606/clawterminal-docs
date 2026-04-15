# /pr -- GitHub PR Workflow

> **Why this matters:** You are on the train. Your teammate's PR needs a review before the morning standup. CatClaw lets you create PRs, review code, check CI, and train the reviewer -- all from your phone.

Full GitHub pull request lifecycle over SSH. Requires `gh` CLI installed and authenticated on your Mac.

```bash
# Install on your Mac (one-time)
brew install gh

# Authenticate (one-time)
gh auth login
```

---

## Create a PR

```
/pr
```

```
/pr create
```

Both commands do the same thing: read `git diff`, ask Claude to generate a PR title and body, then run `gh pr create`. The PR URL appears in your chatroom when done.

---

## AI Code Review

```
/pr review 42
```

Fetches the diff for PR #42 via `gh`, sends it to Claude for analysis. Review items appear color-coded:

| Color | Type | Example |
|-------|------|---------|
| Red | Bug | "This function doesn't handle the null case on line 47" |
| Yellow | Suggestion | "Consider extracting this logic into a helper function" |
| Blue | Question | "Why is this timeout set to 5000ms instead of the default?" |
| Green | Praise | "Nice use of async/await throughout this module" |

Tap **Post Review** to submit the AI review as a GitHub comment via `gh pr review`.

---

## Rate Review Items

Each review item has thumbs-up / thumbs-down buttons:

- **Thumbs up** — reinforce: "flag things like this in future reviews"
- **Thumbs down** — deprioritize: "this isn't relevant to our project"

Ratings are stored per-chatroom. Future reviews learn from your feedback automatically.

---

## Set Review Focus

```
/pr focus security,tests
```

```
/pr focus performance,null-safety,error-handling
```

```
/pr focus
```

Running `/pr focus` with no arguments resets to defaults (all areas equal weight).

---

## List Open PRs

```
/pr list
```

Shows all open pull requests in the current repository with number, title, author, and age.

---

## Check CI Status

```
/pr checks 42
```

Shows the status of all CI checks and workflows for PR #42. Useful for a quick "is it green?" check from your phone before merging.

---

## Real-World Examples

### Fast PR from your phone while commuting

```
# You've committed some changes, now create the PR
/pr create

# While waiting for CI, check an older PR
/pr checks 39

# Review a teammate's PR
/pr review 41
```

### Iterative review learning workflow

```
# Week 1 — fresh start, Claude looks at everything
/pr review 15
# Thumbs down on 3 style nits, thumbs up on null-check and SQL-injection findings

# Week 2 — Claude leads with correctness and security, buries style items
/pr review 22
# Thumbs up on the SQL injection flag, it's the kind of thing that matters here

# Week 3 — Claude's review feels tuned to your project
/pr review 28
```

### Security-focused review

```
/pr focus security,auth,input-validation
/pr review 55
```

### Code quality review for a new contributor

```
/pr focus tests,documentation,error-handling
/pr review 61
```

---

---

## Pro Tips

- **Rate early, rate often.** The first review is always noisy. Spend 2 minutes rating items on your first 2-3 reviews -- the learning pays off for every review after that.
- **Use different chatrooms for different repos.** Review learning is per-chatroom. A strict TypeScript frontend and a relaxed Python backend should have separate chatrooms with separate learned preferences.
- **Focus on bugs first.** If you only have time for one setting, use `/pr focus security,error-handling` and thumb-down everything else. The reviewer becomes a reliable bug-finder, not a style-nagger.
- **Chain with `/preview`.** After `/pr create`, run `/preview --start` to do a final visual check before asking for reviews.

---

## Notes

- `gh` runs on your Mac over SSH, not on your iPhone
- The chatroom's Project Directory must point to the correct git repo
- Works with GitHub only (not GitLab or Bitbucket -- `gh` is GitHub-specific)
- Authentication uses whatever account you logged into with `gh auth login` on your Mac
