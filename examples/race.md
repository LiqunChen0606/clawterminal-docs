# /race -- Multi-Model Comparison

> **Why this matters:** Every AI model has blind spots. Racing multiple models on the same prompt catches mistakes, reveals trade-offs, and saves you from committing to the wrong approach.

`/race` dispatches the same prompt to 2-4 AI models simultaneously and presents the results side-by-side. Pick the winner, accept it into your chatroom, and move on.

---

## Basic Usage: Race Two Models

```
/race Write a function to check if a binary tree is balanced
```

Races `claude` and `codex` by default (both must be installed on your Mac). Results stream in as parallel background jobs. Once both finish, an AI-generated comparison summary appears below — "Claude wrote more defensive code, Codex was more concise."

---

## Race Three or Four Models

```
/race --models claude,codex,gemini Explain the observer pattern with a real code example
```

```
/race --models claude,codex,gemini,aider What is the best way to implement a rate limiter?
```

All selected models are dispatched simultaneously. On iPad, results appear in side-by-side columns. On iPhone, swipe left/right between cards.

---

## Focus on a Specific Model Set

```
/race --models claude,gemini Analyze this JWT implementation for security issues
```

Good for when you specifically want to compare two tools without running all three.

---

## Same-Model Thinking Lenses (`--copies`)

Race the same model with different thinking perspectives:

```
/race --copies 3 How should we implement caching for this API?
```

Randomly picks 3 of 6 thinking lenses. Each copy gets a different system-level perspective injected before your prompt.

---

## Pick Specific Lenses

```
/race --lenses adversarial,pragmatic,optimizer Design a rate limiting strategy
```

```
/race --lenses principled,user-first Should we use REST or GraphQL for this API?
```

```
/race --lenses skeptic,adversarial Review this authentication flow for weaknesses
```

### Available Lenses

| Lens | Focus |
|------|-------|
| `adversarial` | What could break? Edge cases, attack vectors, failure modes |
| `pragmatic` | Simplest path to ship. Maintainability over perfection |
| `principled` | Best practices, design patterns, SOLID principles |
| `user-first` | End user experience, performance, accessibility |
| `skeptic` | Hidden assumptions, unstated requirements, scope creep |
| `optimizer` | Performance, memory, cost efficiency, redundancy |

---

## Combining Models and Lenses

Use `--models` for different tools, `--copies` or `--lenses` for same-tool perspective comparison. These flags are mutually exclusive — pick one approach per race.

```
/race --lenses adversarial,optimizer,user-first Should we use Redis or Memcached for session caching?
```

---

## Accept the Best Response

Each result card has an **Accept** button. Tap it to insert that model's response as the canonical answer in your chatroom. The other cards are dismissed.

---

## Real-World Examples

### Algorithm selection

```
/race --models claude,codex,gemini Implement Levenshtein distance — optimize for readability first, then note performance trade-offs
```

### Documentation wording

```
/race --lenses pragmatic,user-first,principled Write the README introduction for a Go library that parses YAML configuration files
```

### Code review before PR

```
/race --lenses adversarial,skeptic Review this database migration script and tell me what could go wrong
```

### Architecture decision

```
/race --models claude,gemini Should we use a monorepo or separate repos for a React frontend + Node.js API? Consider team size of 3.
```

### Security audit

```
/race --lenses adversarial,principled,skeptic Audit this Express.js middleware for security issues
```

---

---

## Pro Tips

- **Be specific.** "Database options" produces vague races. "Compare PostgreSQL vs ClickHouse for 50M time-series events/day with 2-year retention" gives each model something concrete to reason about.
- **Use Adversarial before proposing designs.** Run `/race --lenses adversarial your-plan-here` before your next architecture review. Better to hear the objections from AI than from your team lead.
- **Accept and continue.** After accepting a race response, you can keep chatting -- Claude builds on the accepted answer naturally.
- **Race documentation, not just code.** `/race --lenses pragmatic,user-first Write the README intro for our CLI tool` finds the clearest explanation before you write it yourself.

---

## Requirements

- Each model in `--models` must be installed and authenticated on your Mac
- `claude` -- requires [Claude Code](https://docs.anthropic.com/en/docs/claude-code): `npm install -g @anthropic-ai/claude-code && claude /login`
- `codex` -- requires the OpenAI Codex CLI: `npm install -g @openai/codex`
- `gemini` -- requires the Gemini CLI: `npm install -g @google/generative-ai-cli`
- `aider` -- requires Aider: `pip install aider-chat`

See the [Multi-CLI Tool Support tutorial](../tutorials/multi-cli-tools.md) for full setup instructions.
