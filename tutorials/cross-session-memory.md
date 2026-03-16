# Cross-Session AI Memory

ClawTerminal remembers facts, preferences, and project context across chatroom sessions. No more re-explaining your tech stack, preferred tools, or project structure every time you start a new conversation.

---

## Quick Start

Save a fact:

```text
/remember always use pnpm, not npm
```

Claude now includes this preference in every future chatroom session — automatically.

---

## Slash Commands

| Command | What it does |
|---------|-------------|
| `/remember [fact]` | Save a fact to memory (e.g. project conventions, tool preferences) |
| `/forget [keyword]` | Remove memories matching the keyword |
| `/memories` | Open the Memory Library to browse, search, and manage all saved memories |

### Examples

```text
/remember this project uses FastAPI with SQLAlchemy and Alembic migrations
/remember deploy script is at ~/scripts/deploy-staging.sh
/remember always run tests with pytest -x --tb=short
/remember prefer async/await over callbacks
```

Remove memories you no longer need:

```text
/forget pnpm
/forget deploy
```

---

## How Memory Works

### Three Categories

Memories are organized into three categories:

| Category | Icon | What it stores |
|----------|------|---------------|
| **Project Context** | folder | Tech stack, key files, repo structure, deploy commands |
| **User Preferences** | person | Coding style, tool choices, workflow preferences |
| **Session Insights** | lightbulb | Decisions made, bugs fixed, architecture choices |

ClawTerminal auto-categorizes each `/remember` entry based on keywords.

### Project vs Global Memories

- **Project memories** are tied to a specific project directory. They only appear in chatrooms pointed at that project.
- **Global memories** (saved when no project directory is set) appear in all chatrooms.

In the Memory Library, global memories show a blue **Global** badge.

### Relevance Scoring

Not all memories are injected into every message. ClawTerminal scores each memory for relevance:

- **Keyword match** — Does the memory relate to what you're asking about?
- **Recency** — Recently created memories score higher
- **Frequency** — Frequently used memories score higher
- **Global boost** — Global preferences get a small boost so they're always considered

The top memories (up to ~2KB) are injected into the system prompt. This keeps context focused and avoids wasting tokens on irrelevant facts.

---

## Memory Library

Tap the **brain icon** in the chatroom toolbar to open the Memory Library.

### Features

- **Search** — Filter memories by keyword
- **Grouped by category** — Project Context, User Preferences, Session Insights
- **Swipe to delete** — Remove individual memories
- **Clear all** — Trash icon to clear project or global memories (with confirmation)
- **Badge count** — The toolbar button shows how many memories exist for the current project

### Memory Details

Each memory row shows:

- The memory content
- **Global** badge (if not project-specific)
- **Manual** or **Auto** source label
- Relative timestamp (e.g. "2 hours ago")

---

## Tips

- **Be specific**: `/remember deploy to staging: cd ~/myapp && ./scripts/deploy.sh staging` is more useful than `/remember deploy stuff`
- **Use for standing instructions**: `/remember always add type annotations to Python functions` — Claude will follow this in every session
- **Project-specific**: Set a project directory in the chatroom Info panel before using `/remember` to make memories project-scoped
- **Clean up regularly**: Use `/forget` or the Memory Library to remove outdated facts
- **Works in both modes**: Memory injection works in both CLI mode (via preamble) and API mode (via system prompt)

---

## How It's Stored

Memories are saved to `memory_store.json` in the app's Documents directory. They persist across app restarts. Each memory entry tracks:

- Content text
- Auto-extracted keywords (for relevance matching)
- Category and source
- Creation date, last-used date, and use count

---

## FAQ

**Q: How many memories can I save?**
A: There's no hard limit on stored memories. However, only the top ~12 most relevant memories (up to 2KB) are injected per message to keep token costs manageable.

**Q: Do memories sync across devices?**
A: Currently memories are stored locally on each device. iCloud sync is planned for a future update.

**Q: Can Claude automatically learn from conversations?**
A: Auto-extraction (where Claude summarizes conversations into memories) is planned for a future sprint. For now, use `/remember` to explicitly save important facts.

**Q: Do memories work with other CLI tools (Aider, Codex)?**
A: Yes — memories are injected into the preamble regardless of which CLI tool is active in the chatroom.
