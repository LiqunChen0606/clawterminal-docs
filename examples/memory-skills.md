# Memory and Skills Examples

Memory persists facts about your project and preferences across sessions. Skills provide Claude with domain knowledge. Together they make Claude smarter in every conversation without repeating yourself.

---

## Cross-session memory

### Remember facts

Save anything you'd otherwise have to re-explain every session.

**Project conventions:**
```
/remember always use pnpm, not npm
```
```
/remember this project uses FastAPI with SQLAlchemy and Alembic migrations
```
```
/remember the deploy script is at ~/scripts/deploy-staging.sh — run it with ./deploy-staging.sh <version>
```
```
/remember always run tests with pytest -x --tb=short before committing
```

**Preferences:**
```
/remember prefer async/await over callbacks
```
```
/remember always add type annotations to Python functions
```
```
/remember write commit messages in the imperative mood: "Add feature" not "Added feature"
```

**Architecture decisions:**
```
/remember we decided to use Redis for rate limiting, not in-memory counters, because the app runs on multiple servers
```
```
/remember the authentication service is a separate microservice at http://auth.internal:8080 — never call it directly from the frontend
```

**File locations:**
```
/remember the Stripe webhook handler is in src/billing/webhooks.py
```
```
/remember environment variables are documented in .env.example — never hardcode them
```

After saving, these facts appear in Claude's context on every subsequent message in this chatroom — no re-explaining needed.

---

### Forget memories that are no longer accurate

**Remove by keyword:**
```
/forget pnpm
```

Removes any memory entry that matches the keyword "pnpm". If you've switched to Yarn, save the new fact and forget the old one:

```
/forget pnpm
/remember always use yarn, not npm or pnpm
```

**More examples:**
```
/forget Redis
```
```
/forget deploy-staging
```

---

### Browse and manage the Memory Library

```
/memories
```

Opens the Memory Library sheet with:
- **Search bar** — filter by keyword
- **Grouped by category** — Project Context, User Preferences, Session Insights
- **Swipe to delete** — remove individual entries
- **Global badge** — memories saved without a project directory apply to all chatrooms
- **Manual / Auto badge** — whether you saved it explicitly or it was auto-extracted

The toolbar brain icon shows a badge count for the current project's memories.

---

### Project-scoped vs global memories

**Project-scoped memory** (most common):
Set a Project Directory in the chatroom Info panel before using `/remember`. Memories saved in that chatroom only appear in chatrooms with the same project directory.

```
/remember deploy to staging: cd ~/Projects/myapp && ./scripts/deploy.sh staging
```

This memory appears when working on `myapp`, not in unrelated chatrooms.

**Global memory:**
Open a chatroom with no Project Directory set, then use `/remember`. The entry gets a Global badge and appears in every chatroom.

```
/remember always prefer explicit error messages over generic "something went wrong" text
```

A coding preference like this applies to all projects.

---

## Skills

Skills are prompt snippets that give Claude domain knowledge. Enable them for a session or attach them per-message.

---

### Enable skills for a session

Tap the **Skills button** (wand icon) in the chat toolbar to open the Skills panel.

- Toggle any skill on to inject its system prompt into every subsequent message in this session
- Toggle off to remove it

**Common skills to enable when:**

| When you're doing... | Enable... |
|---------------------|-----------|
| Adding tests | `tdd` |
| Security review | `security` |
| Database work | `backend`, `database` |
| Writing documentation | `technical-writing` |
| Code review | `code-review` |
| Docker/deployment work | `docker`, `devops` |

---

### Use skills for a single message

Add `--skills alias1,alias2` to any message to attach skills to just that one request, without enabling them globally.

**Example: One-off security check**
```
Review src/auth/login.py --skills security
```

Claude gets the security skill's context for this message only.

**Example: Write tests with TDD approach**
```
Add tests for the checkout flow --skills tdd
```

**Example: Multiple skills on one message**
```
Refactor the payment module with proper error handling --skills backend,security,testing
```

**This also works with /submit:**
```
/submit --skills tdd,security Implement the two-factor authentication flow
```

---

### Install skills from the marketplace

1. Tap the Skills button in the chat toolbar
2. Tap **Marketplace** (or browse the installed skills list for a marketplace link)
3. Browse available skill packages
4. Tap **Install** on any skill

Installed skills are available immediately in the Skills toggle panel and in the `--skills` flag autocomplete.

---

### Write a custom skill

1. Tap Skills > **+** to create a new skill
2. Give it a name and alias (e.g. name: "API Design", alias: `api-design`)
3. Write the system prompt — this is injected verbatim into Claude's context when the skill is active

**Example custom skill for a specific project's conventions:**

Name: `myapp-conventions`
Alias: `conventions`
System prompt:
```
This project follows these conventions:
- All API endpoints use snake_case and return JSON with {data, error, meta} wrapper
- Database models use SQLAlchemy 2.x with Mapped[] type annotations
- Tests use pytest with fixtures in conftest.py — never use unittest.TestCase
- Authentication is handled by the auth middleware in src/middleware/auth.py — never roll your own
- Errors are logged with structlog, not print() or logging.getLogger()
```

4. Tap Save. Now `/remember` project-wide decisions and use `--skills conventions` for any task involving this codebase.

---

### Export and share skills

Tap a skill > **Export** to save it as a JSON file. Share it with teammates who can **Import** it directly.

This is useful for standardizing AI behavior across a team — everyone uses the same security or TDD skill definition.

---

### Audit your skill library (Last fired badge + sort)

Saved skills accumulate. Built-in skills + skills you imported + skills you extracted from trajectories can reach 30+ entries fast, and most chatrooms only meaningfully use a handful. The audit view makes it easy to see which ones are pulling their weight.

1. Open the chatroom toolbar > **🧩 Skills** to bring up Manage Skills.
2. Each row in the Installed list now shows a small clock icon and a relative-date badge below the description: **"5d ago"**, **"just now"**, **"2w ago"** for skills that have fired recently, or **"Never"** for skills that haven't matched any message yet.
3. Tap the **arrow.up.arrow.down.circle** icon in the toolbar to switch the sort order:
   - **Name** — alphabetical (the default).
   - **Last used** — most recently fired skills at the top, "Never" skills sink to the bottom.

Use the badge to triage:

- A skill that says "Never" after weeks of use probably has keywords too narrow (it never matches the messages you actually send) — open it in the editor and broaden the keyword list, or disable it.
- A skill that says "3 months ago" might be technically working but not relevant to your current project — toggle it off so it stops eating preamble tokens.
- Skills with recent fired dates ("just now", "5m") are doing their job — keep them on.

The badge updates whenever a skill's full content is injected into the preamble (i.e., when its keywords match your message). Skills that show up in the always-injected index without their full body don't count as "fired" and won't update the badge.

**Note:** older skills saved before this update show "Never" until the next time they fire. The schema is backward-compatible — no migration needed.
