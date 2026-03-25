# Agent Teams Examples

`/team` is wave-based orchestration: a Commander decomposes your goal into sequential waves (Research → Implementation → Review). Agents within each wave run in parallel. When a wave completes, its discoveries are extracted and injected into the next wave — so each phase builds on what the previous one found.

Use `/team` when phases have dependencies. Use `/batch` when tasks are independent.

---

## How waves work

```
/team Write a Python calculator with add, subtract, multiply, divide, input validation, and a REPL loop

  Wave 1 — Research (runs first)
    Agent A: Scan project for existing math utilities, conventions, Python version
    Agent B: Identify edge cases and REPL design patterns

  Discoveries extracted and injected into Wave 2:
    • Python 3.11+ is available
    • No existing math utilities — build from scratch
    • Input validation should handle non-numeric strings and ZeroDivisionError
    • REPL should support q/quit/exit

  Wave 2 — Implementation (starts after Wave 1 completes)
    Agent A: Write the operation functions (add, subtract, multiply, divide)
    Agent B: Write the REPL loop with input validation

  Discoveries extracted and injected into Wave 3:
    • calculator.py created at ~/Projects/calculator/calculator.py
    • All four operations implemented as pure functions
    • REPL handles ValueError and ZeroDivisionError

  Wave 3 — Review (starts after Wave 2 completes)
    Agent A: Test all operations, check edge cases
    Agent B: Review code quality, documentation, REPL UX

  Final summary posted to chatroom
```

---

## 1. Basic: single goal, three waves

```
/team Write a Python calculator with add, subtract, multiply, divide, input validation, and a REPL loop
```

ClawTerminal spawns the Commander, then executes Research → Implementation → Review waves. The Visual Command Center shows the animated flow graph as agents run.

**Expected output:**
- A working `calculator.py` on your Mac with all four operations
- A REPL that handles invalid input gracefully
- Wave 3 review findings (any edge cases that need attention)

---

## 2. Complex refactor that needs research before implementation

```
/team Refactor the authentication system to use JWT tokens with refresh token rotation
```

**Why `/team` over `/batch`:**
The implementation wave needs to know what the existing auth system looks like — its models, middleware, session management, and API surface — before writing a single line of new code. With `/team`, Research agents map that out first, and Implementation agents receive their findings as context.

**What each wave produces:**

Wave 1 — Research:
- "Authentication currently uses session cookies with flask-login"
- "User model has: id, email, password_hash, created_at"
- "3 files use `current_user` from flask-login"
- "No existing JWT library installed"

Wave 2 — Implementation (knows all of the above):
- Adds `PyJWT` to requirements.txt
- Creates `src/auth/jwt_service.py` with token generation and validation
- Creates `src/auth/refresh_tokens.py` with rotation logic
- Updates `src/auth/routes.py` to issue tokens on login
- Updates the 3 files that previously used `current_user`

Wave 3 — Review (knows what was changed):
- "Refresh token rotation looks correct"
- "Token expiry is hardcoded — recommend moving to config"
- "Missing unit tests for token revocation"

---

## 3. Multi-tool with --multi flag

```
/team --multi Build a REST API with database models, endpoints, tests, and documentation
```

With `--multi`, the Commander assigns different tools to agents based on their strengths:

**Wave 1 — Research:**
- Gemini agent: read all existing source files at once (large context)
- Claude agent: analyze architecture and identify conventions

**Wave 2 — Implementation:**
- Aider agent: create database models with git-tracked commits
- Claude agent: write endpoint handlers
- Claude agent: write tests

**Wave 3 — Review:**
- Claude agent: check API design and consistency
- Claude agent: verify test coverage

Tools detected are those installed on your Mac (`claude`, `aider`, `codex`, `gemini`). If a tool is not installed, that subtask falls back to Claude.

---

## 4. Feature development from scratch

```
/team Add a user notification system: email notifications for account events (signup, password reset, order confirmation) with templates and an unsubscribe flow
```

**Wave 1 — Research finds:**
- Existing email config uses SendGrid
- User model has `email`, `email_verified`, `notifications_enabled`
- No existing template engine — Jinja2 is already in requirements
- Account events currently log to console only

**Wave 2 — Implementation uses those findings:**
- Creates `src/notifications/` directory with `email_service.py`, `templates/`, and `unsubscribe.py`
- Adds Jinja2 templates for each event type
- Wires account events (signup, password reset, order) to trigger emails
- Adds `GET /unsubscribe?token=...` endpoint

**Wave 3 — Review:**
- "Unsubscribe token is not time-limited — recommend 7-day expiry"
- "Templates tested manually but no unit tests for email_service.py"
- "Missing rate limiting on the unsubscribe endpoint"

---

## 5. Open the Visual Command Center

While a team is running (and any time after), tap the **Team** button in the chat toolbar.

**What you see:**

- **Animated flow graph** — circular nodes for each agent, pulsing when running, checkmarks when complete, data-flow lines between waves as discoveries propagate
- **Live discovery feed** — real-time stream of discoveries extracted from completed agents, with wave attribution and timestamps
- **Per-agent status cards** — progress ring, current status (Queued / Running / Complete / Failed), assigned tool, and a preview of the most recent output

Even after the team finishes, the Team button reopens the flow graph so you can replay the discovery timeline and understand exactly what happened.

---

## Choosing between /batch and /team

| Scenario | Use |
|----------|-----|
| Tasks are independent (audit 30 endpoints, add tests to 20 files) | `/batch` |
| Implementation needs research findings to be correct | `/team` |
| You want parallel execution with no phase dependencies | `/batch` |
| You want Research → Implement → Review with knowledge propagation | `/team` |

**Rule of thumb:** If you could assign the subtasks to independent contractors with no communication between them, use `/batch`. If the contractors need to read each other's notes before starting, use `/team`.
