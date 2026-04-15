# Tutorial: Multi-Model Comparison with `/race`

> You are about to commit to a new caching strategy. You have a gut feeling about Redis, but you are not sure if Memcached or a simple in-memory LRU would be better for your use case. Instead of asking one AI and trusting its first answer, you race three models on the same question and get three perspectives in 30 seconds. That is what `/race` is for.

This tutorial walks you through everything from your first race to advanced thinking lenses that stress-test your designs from multiple angles.

---

## When to Use `/race`

### Use different-model racing (`--models`) when:

- You want to benchmark how Claude, Codex, and Gemini each approach a problem
- You need a second (or third) opinion before committing to an implementation
- You are choosing between architectural approaches and want diverse perspectives
- The stakes are high enough that "trust, but verify" applies

### Use same-model thinking lenses (`--copies` / `--lenses`) when:

- You already know which model you want to use but want to stress-test an idea
- You want an adversarial critique of your plan without switching to a different tool
- You need to evaluate trade-offs systematically (performance vs. maintainability vs. user experience)
- You're doing architecture or design work where multiple viewpoints matter

---

## Part 1: Your First `/race` Run

### Step 1: Make sure the models are installed

Before your first race, confirm the models you want to use are installed on your Mac. Open the Terminal tab and run:

```bash
which claude    # Should print a path like /usr/local/bin/claude
which codex     # For OpenAI Codex CLI
which gemini    # For Google Gemini CLI
```

If any are missing, see [Multi-CLI Tool Support](multi-cli-tools.md) for setup.

### Step 2: Run a basic race

In your chatroom, type:

```
/race Explain the difference between a mutex and a semaphore with a code example
```

ClawTerminal dispatches this to `claude` and `codex` (the default two). You will see two status indicators appear in the Jobs panel — one for each model. As they stream in, results populate their respective cards.

### Step 3: Read the comparison summary

Once both models finish, an AI-generated comparison appears below the result cards:

> "Claude provided a more thorough theoretical explanation with a real-world analogy. Codex went straight to code — two focused examples in Go and Python. If you need to explain this to a junior engineer, Claude's response is better. If you need working code right now, use Codex's."

### Step 4: Accept the response you prefer

Tap the **Accept** button on the card you find most useful. That response is inserted into the chatroom as the answer and the race is closed.

---

## Part 2: Racing Three or Four Models

Adding Gemini to a race gives you a long-context perspective that sometimes catches things Claude and Codex miss:

```
/race --models claude,codex,gemini What are the security implications of storing JWTs in localStorage vs. httpOnly cookies?
```

On **iPad**: three columns appear side-by-side.
On **iPhone**: three swipeable cards — swipe left to advance, swipe right to go back.

The comparison summary at the end names each model and its standout contribution.

---

## Part 3: Thinking Lenses

Thinking lenses let you race a single model against itself with different system-level perspectives. This is useful when you trust Claude (or whichever model you use) but want to make sure you're not missing blind spots.

### How lenses work

Each lens injects a different system-level framing before your prompt. The model sees the same question but approaches it from a different angle:

- **Adversarial** — The model imagines it's a red-team engineer looking for failure modes, edge cases, and attack vectors. It will push back on your assumptions.
- **Pragmatic** — The model acts as an experienced senior engineer focused on getting the job done without over-engineering. Prefers boring, maintainable solutions.
- **Principled** — The model applies SOLID principles, design patterns, and best-practice frameworks. Prioritizes correctness and testability.
- **User-First** — The model thinks as a product engineer whose primary concern is the end-user experience: latency, error messages, accessibility, progressive enhancement.
- **Skeptic** — The model probes the premise. It identifies hidden assumptions, unstated requirements, and scope creep in the question itself before answering.
- **Optimizer** — The model focuses on performance, memory usage, cost efficiency, and reducing redundancy. Will suggest profiling before optimizing and prefer data-driven approaches.

### Example: Algorithm selection with lenses

```
/race --lenses pragmatic,principled,optimizer Implement a rate limiter for an API endpoint
```

You will get three responses from the same model:

- **Pragmatic** might suggest a simple in-memory sliding window with Redis, acknowledging it won't work distributed but that's OK for now
- **Principled** might implement a token bucket algorithm with a clean interface and dependency injection for the storage backend
- **Optimizer** might analyze the tradeoffs between token bucket, leaky bucket, and sliding window log, then recommend token bucket with a note about Redis atomic operations

This is far more useful than asking the question once and accepting the first answer.

### Example: Architecture decision

```
/race --lenses adversarial,user-first,skeptic Should we add real-time notifications to the app using WebSockets?
```

- **Adversarial** will list what can go wrong: connection drops, reconnect storms, server fan-out at scale, browser tab limits
- **User-First** will focus on perceived responsiveness, fallback polling, notification preferences, and battery drain on mobile
- **Skeptic** might ask: Do you actually need real-time? Have you measured the latency of the current polling approach? What's the actual user complaint?

Together, these three perspectives often produce a more nuanced decision than a single-lens answer.

### Example: Random lens selection

If you don't want to pick lenses manually, let the app choose:

```
/race --copies 4 How should we structure the error handling in this service?
```

Four copies of the model run with four randomly selected lenses. You'll see which lens each copy received in the card header.

---

## Part 4: Tips for Getting the Most Out of `/race`

### Be specific in your prompt

Vague prompts produce vague races. Instead of:

```
/race database options
```

Try:

```
/race --lenses pragmatic,optimizer,principled We need to store 50M time-series events per day, support aggregation queries by hour/day/week, and keep 2 years of history. Compare PostgreSQL with TimescaleDB, InfluxDB, and ClickHouse for our use case.
```

Specificity gives each model something concrete to reason about. The comparison summary will be much more useful.

### Use the Adversarial lens on your own plans

Before you propose a design in a PR review or team meeting, run it through the Adversarial lens:

```
/race --lenses adversarial Our plan is to use event sourcing for the orders domain. We'll use Kafka as the event bus and Postgres as the event store. The read model is maintained by a projection service.
```

The Adversarial copy will identify things like: "What happens when the projection service falls behind? How do you handle schema evolution for events from 3 years ago? What's your strategy for replaying events when a bug in a projection is discovered?"

Better to hear this before you build than after.

### Use `/race` before a big architectural commit

If you're about to make a foundational decision that's expensive to undo — choosing a database, a framework, an API design pattern — run a race first:

```
/race --models claude,gemini We're choosing between REST, GraphQL, and gRPC for our internal microservices API. The services are in Go, clients include a React web app, iOS app, and 3 backend services. Expected traffic: 10K RPM at peak. What are the trade-offs and what would you choose?
```

Gemini's strength is long-context reasoning — if you paste in your current API surface area or your team's capabilities, it will incorporate that context more thoroughly.

### Compare documentation quality

Use `/race` to find the clearest explanation before you write documentation yourself:

```
/race --lenses pragmatic,user-first,principled Write a brief explanation of OAuth 2.0 for a developer who knows web development but has never implemented authentication before
```

The User-First copy will naturally produce the most approachable explanation. You can use it as a starting point.

### Accept, edit, and continue

After accepting a race response, you can continue the conversation normally. The accepted response is just a message in your chatroom — Claude can build on it, revise it, or extend it in subsequent turns.

---

## Part 5: Understanding the Comparison Summary

After all models finish, ClawTerminal sends a lightweight follow-up request to generate the comparison summary. This summary:

- Identifies each model's strongest contribution
- Flags where models agreed and where they diverged
- Notes correctness differences if any (e.g., "Codex's implementation had an off-by-one error in the boundary condition")
- Recommends which response to use based on your likely use case

The summary is generated by the chatroom's currently selected model (shown in the banner). If you want a more thorough comparison, switch to a more capable model with `/model claude-opus-4-6` before running the race.

---

## Part 6: `/race` vs. Other Commands

| Scenario | Best command |
|----------|-------------|
| Compare three models on the same problem | `/race --models claude,codex,gemini <prompt>` |
| Stress-test your design from multiple perspectives | `/race --lenses adversarial,skeptic,optimizer <prompt>` |
| Get a diverse implementation before choosing one | `/race --copies 3 <prompt>` |
| Long, multi-step task in parallel | `/batch --agents 3 <goal>` |
| Research → Implement → Review with context propagation | `/team <goal>` |
| Single focused background task | `/submit <task>` |

`/race` is best for **decision support and comparison**. When you already know what you want and just need it done, `/submit` or `/batch` are faster.

---

## Requirements

- Each model you race must be installed and authenticated on your Mac
- SSH connection to your Mac with tmux installed
- `gh` CLI is NOT required for `/race` (that's only needed for `/pr`)

See the [examples/race.md](../examples/race.md) for more copy-paste-ready examples.

---

## What's Next?

- **[GitHub PR Workflow](pr-workflow.md)** -- After racing models to pick the best approach, create a PR and get an AI code review from your phone.
- **[Session Handoff](handoff.md)** -- Accepted a race winner and want to continue with a full keyboard? Hand off to your Mac in one command.
- **[Agent Teams](agent-teams.md)** -- When you need more than comparison -- full Research --> Implement --> Review orchestration with multiple agents.
