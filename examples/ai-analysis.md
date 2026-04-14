# AI Analysis & Automation Commands

Examples for `/changelog`, `/gentest`, `/security`, `/tribal`, `/spec`, `/graph`, and `/hooks` — commands that help you understand, document, test, and automate your codebase.

---

## /changelog — AI Release Notes

```
/changelog                    — last 20 commits
/changelog v1.0..v1.1         — between tags
/changelog HEAD~5..HEAD        — last 5 commits
/changelog main..feature      — branch diff
```

Generates structured release notes organized into Features, Improvements, Bug Fixes, and Other Changes.

**Before a GitHub release:**

```
/changelog v1.4.0..HEAD
```

CatClaw reads the git log for that range and produces a formatted changelog you can paste directly into your release notes or CHANGELOG.md.

**After a sprint:**

```
/changelog HEAD~30..HEAD
```

Get a summary of the last 30 commits grouped by change type — useful for sprint retrospectives and stakeholder updates.

**Comparing a feature branch to main:**

```
/changelog main..feature/payments
```

See everything that has changed on the feature branch relative to main — useful for pre-PR review or writing a PR description.

---

## /gentest — AI Test Generation

```
/gentest the user authentication flow
/gentest API endpoint /api/users with validation
/gentest the payment processing module edge cases
```

Auto-detects your test framework (Jest, Vitest, pytest, XCTest, Mocha) and generates comprehensive tests.

**Generate tests for a specific module:**

```
/gentest the UserService class including error cases
```

CatClaw scans your project for the relevant files and generates tests covering happy path, error handling, and edge cases.

**Generate integration tests:**

```
/gentest API endpoint POST /api/orders end-to-end including database
```

Claude reads your route handlers, models, and middleware to produce realistic integration tests with appropriate mocks.

**Generate XCTest for iOS:**

```
/gentest the LoginViewModel with invalid credential handling
```

In a Swift/iOS project, CatClaw detects XCTest and generates `XCTestCase` subclasses with proper `setUp`/`tearDown` and async test methods.

**Generate edge case coverage:**

```
/gentest the payment processing module edge cases
```

Focuses on boundary conditions, null inputs, network failures, and race conditions rather than the happy path.

---

## /security — Deep Security Scan

```
/security
```

Runs four checks:
1. **Dependency audit** — `npm audit` / `pip-audit` / `cargo audit`
2. **Pattern scan** — hardcoded passwords, API keys, `eval()`, SQL injection patterns
3. **Secret files** — `.env` files tracked in git
4. **.gitignore gaps** — missing common patterns

Results sent to Claude for prioritized analysis with fix suggestions.

**Before a production deploy:**

```
/security
```

A full scan of your project takes 30–60 seconds depending on the number of dependencies. Claude returns a prioritized list: Critical → High → Medium → Low, with a one-line fix for each issue.

**What the pattern scan catches:**

- Hardcoded API keys and tokens (`sk-`, `ghp_`, `AKIA`, etc.)
- Passwords and secrets in source code
- `eval()` with user input
- SQL queries built with string concatenation
- `shell=True` in Python subprocess calls
- `dangerouslySetInnerHTML` without sanitization

**What the .env check catches:**

```
/security
```

If any `.env`, `.env.local`, or `.env.production` file appears in `git log --all`, CatClaw flags it with the commit where it was added — even if the file has since been deleted from the working tree.

---

## /tribal — Tribal Knowledge Extraction

```
/tribal
/tribal ~/projects/backend
```

Runs as a background job. Scans your project for unwritten knowledge:
- Architecture decisions and their rationale
- Gotchas and pitfalls for new developers
- Hidden dependencies between modules
- Naming conventions that encode meaning
- Performance constraints and bottlenecks
- Historical context (past rewrites, abandoned approaches)
- Testing gaps

**Before onboarding a new developer:**

```
/tribal
```

The job runs in the background (typically 2–5 minutes for a medium-sized project). When complete, the result appears in the Jobs tab. Copy the output into a `CONTRIBUTING.md` or Notion page for your team.

**For a specific subdirectory:**

```
/tribal ~/projects/myapp/backend
```

Scope the extraction to a single service or module when you only need knowledge about that area.

**Example output structure:**

- **Architecture decisions**: "The auth layer uses JWT over sessions because X, decided in 2023 migration"
- **Gotchas**: "Never call `UserService.find()` directly from a route — always go through `AuthContext`"
- **Hidden dependencies**: "The notification module imports from the billing module for plan-tier checks"
- **Naming conventions**: "`handle*` methods are event handlers, `process*` methods do async work"

---

## /spec — Spec-Driven Development

```
/spec Add user authentication with OAuth2 and JWT
/spec Build a caching layer for the API with TTL support
```

Generates a structured spec with:
- Formal requirements in EARS notation ("When X, the system shall Y")
- Files to create/modify
- Ordered implementation tasks
- Acceptance criteria
- Risks and edge cases

After generating, Claude asks if you want to execute the spec with agents.

**For a new feature:**

```
/spec Add rate limiting to all public API endpoints
```

CatClaw reads your existing route structure and generates a spec that fits your codebase — naming your actual files, using your existing middleware patterns, and identifying the right insertion points.

**For a refactor:**

```
/spec Migrate the authentication module from sessions to JWT
```

The spec identifies every file that touches the current auth system, lists them in dependency order for migration, and defines acceptance criteria for backward compatibility.

**EARS notation examples from a spec:**

```
WHEN a user submits a login request with valid credentials,
  THE SYSTEM SHALL return a signed JWT with a 15-minute expiry.

WHEN a user submits a login request with invalid credentials,
  THE SYSTEM SHALL return a 401 response after a 200ms constant delay
  to prevent timing attacks.

WHILE a JWT is expired,
  THE SYSTEM SHALL reject all authenticated endpoints with 401
  and include a Retry-After header.
```

**Execute a spec with agents:**

After `/spec` generates the document, tap **Execute with agents** (or reply "yes") to dispatch a `/batch` run where each agent handles one section of the spec.

---

## /graph — Codebase Knowledge Graph

```
/graph how does the auth flow work end-to-end
/graph what depends on the UserManager class
/graph why does the payment module import notifications
/graph trace the data flow from API request to database
```

Analyzes your project's file tree, import map, and class/function definitions to answer architectural questions with specific file and line references.

**Understand a flow end-to-end:**

```
/graph how does a user password reset work end-to-end
```

CatClaw maps every file involved in the flow from the API endpoint through services, database queries, and email dispatch — listing each step with the file name and function.

**Find all dependents of a class:**

```
/graph what imports or uses the PaymentService class
```

Returns a ranked list of files that depend on `PaymentService`, grouped by direct imports, indirect usage through other services, and test files.

**Explain an unexpected dependency:**

```
/graph why does the auth module import from billing
```

CatClaw traces the import chain and explains what functionality is being used and why — useful for spotting circular dependencies or misplaced logic.

**Trace data flow:**

```
/graph trace where user.email goes after signup
```

Follows the `email` field from the signup handler through storage, email queuing, analytics events, and any downstream consumers.

**Find the right file to edit:**

```
/graph where should I add a new middleware that runs before auth
```

Returns the specific file, the function to modify, and the line number where a new middleware should be inserted — based on analysis of your existing middleware chain.

---

## /hooks — Agent Hooks (File-Change Automation)

```
/hooks add auto-test "*.swift" "run tests for changed files"
/hooks add doc-sync "*.md" "check if docs match code changes"
/hooks add lint "*.py" "run pylint on changed files"
/hooks list
/hooks remove auto-test
/hooks stop
```

Polls for file changes every 10 seconds. When files matching the pattern are modified, automatically dispatches a background job with the specified action. Each hook has a 30-second cooldown to prevent spam.

**Auto-run tests on Swift file changes:**

```
/hooks add auto-test "*.swift" "run tests for changed files"
```

Every time you save a Swift file, a background job fires running the relevant tests. Job results appear in the Jobs tab and trigger a notification when complete.

**Keep documentation in sync:**

```
/hooks add doc-sync "src/**/*.ts" "check if README or docs need updating based on code changes"
```

When any TypeScript source file changes, Claude reviews the change and flags documentation that may be out of date.

**Lint on Python changes:**

```
/hooks add lint "*.py" "run pylint and mypy on changed files, report issues"
```

Any `.py` file change triggers a background lint job. The result is posted back to the chatroom with a summary of issues found.

**Check active hooks:**

```
/hooks list
```

Shows all active hooks, their file patterns, actions, last-triggered timestamp, and job count.

**Remove a specific hook:**

```
/hooks remove lint
```

**Stop all hooks:**

```
/hooks stop
```

Useful before a large refactor where you'd otherwise trigger dozens of jobs from bulk file changes.

**Combining hooks with notifications:**

```
/hooks add auto-test "*.swift" "run tests for changed files"
/notify when tests finish
```

Set both before a long coding session. Tests fire automatically on each save and a push notification arrives when they pass or fail — no manual polling needed.
