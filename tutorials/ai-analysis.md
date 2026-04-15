# AI Analysis & Automation -- Tutorial

> You are about to ship v2.0. You need release notes for the GitHub release, a security audit before production, tests for the new payment module, and documentation of the gotchas your team never wrote down. Without CatClaw, that is a full day of work. With these seven commands, it is 15 minutes and a handful of background jobs.

Each command below addresses a different gap in the typical development workflow. They work independently and chain together beautifully.

---

## Table of Contents

1. [/changelog — AI Release Notes](#changelog--ai-release-notes)
2. [/gentest — Test Generation](#gentest--test-generation)
3. [/security — Deep Security Scan](#security--deep-security-scan)
4. [/tribal — Tribal Knowledge Extraction](#tribal--tribal-knowledge-extraction)
5. [/spec — Spec-Driven Development](#spec--spec-driven-development)
6. [/graph — Codebase Knowledge Graph](#graph--codebase-knowledge-graph)
7. [/hooks — Agent Hooks](#hooks--agent-hooks)
8. [Combining Commands: Real-World Pipelines](#combining-commands-real-world-pipelines)

---

## /changelog — AI Release Notes

### When to use it

Use `/changelog` when you need release notes, a PR description, or a sprint summary and don't want to manually sort through commits. It's fastest when your commit messages are reasonably descriptive — even `fix: payment timeout` is enough for Claude to categorize correctly.

### How it works

`/changelog` reads the git log for the specified range, groups commits by inferred category (Features, Improvements, Bug Fixes, Other Changes), and writes structured release notes. It strips noise (merge commits, automated version bumps) and expands abbreviated commit messages using context from the diff when needed.

### Step-by-step walkthrough

**Step 1 — Connect to your Mac and open a chatroom pointed at your project.**

Make sure the chatroom's Project Directory is set to your repo root. CatClaw needs git access to read the log.

**Step 2 — Run `/changelog` with a range.**

For a release:
```
/changelog v1.4.0..HEAD
```

For the last two weeks of commits:
```
/changelog HEAD~40..HEAD
```

For a feature branch:
```
/changelog main..feature/auth-refactor
```

**Step 3 — Review and copy the output.**

The result appears in the chatroom as a structured Markdown block. Tap **Copy** (from the clipboard icon in the message toolbar) to copy it, then paste into GitHub, Notion, or your CHANGELOG.md.

### Tips and best practices

- **Conventional commits pay off here**: If your team uses `feat:`, `fix:`, `chore:`, `/changelog` categorizes with near-perfect accuracy.
- **Use it for PR descriptions**: Run `/changelog main..HEAD` just before `/pr create` and paste the output into the PR body — your PR description is done.
- **Combine with `/health`**: Run `/health` and `/changelog HEAD~20..HEAD` together before a release to get both a diff summary and a codebase snapshot.
- **Limit to meaningful ranges**: Very large ranges (500+ commits) produce long output. Use tag-to-tag ranges for clean, scoped notes.

---

## /gentest — Test Generation

### When to use it

Use `/gentest` when you have a feature that needs test coverage but writing tests from scratch would take 20–30 minutes. It's also useful when you want edge-case coverage you might not think of yourself — CatClaw is particularly good at identifying failure modes like null inputs, network timeouts, and concurrent access.

### How it works

CatClaw scans your project to detect the test framework (checking `package.json`, `requirements.txt`, `Podfile`, `Package.swift`, etc.), locates the relevant source files based on your description, reads them, and generates tests that match your project's existing style. The generated tests are shown in the chatroom — you review them, then tap **Run** or paste them into your editor.

### Step-by-step walkthrough

**Step 1 — Open a chatroom with the project directory set.**

CatClaw needs to read your source files and detect the framework. Make sure the Project Directory in the chatroom Info tab points to your repo root.

**Step 2 — Describe what you want tested.**

Be specific about the feature, not the file:
```
/gentest the user registration flow including validation and duplicate email handling
```

Less useful:
```
/gentest UserController.swift
```

**Step 3 — Review the generated tests.**

The generated tests appear as a code block in the chatroom. Read through them — particularly the edge cases — before approving. Claude typically generates:
- One or more happy-path tests
- Validation failure tests (empty fields, invalid formats)
- Error handling tests (network failures, database errors)
- Boundary condition tests (empty lists, max-length strings)

**Step 4 — Write to disk.**

Tap the **Run** button on the code block to execute the test file creation command, or tap **Copy** and paste into your editor.

**Step 5 — Run the tests.**

In the same chatroom:
```
/submit run the new tests and fix any failures
```

CatClaw submits a background job that runs the test suite and fixes any syntax or import errors in the generated tests.

### Tips and best practices

- **Describe behavior, not files**: "the checkout flow when the cart is empty" produces better tests than "CartViewController.swift".
- **Ask for edge cases explicitly**: Add "including edge cases and error scenarios" to your description to push Claude toward defensive coverage.
- **Iterate**: If the first generation misses something, reply "also test what happens when the network is offline" to add more tests in the same session.
- **Use with `/spec`**: Generate a spec first with `/spec`, then use `/gentest` to generate tests for each acceptance criterion. This produces acceptance-test-style coverage that maps directly to your requirements.

---

## /security — Deep Security Scan

### When to use it

Run `/security` before any production deploy, before merging a PR that adds new dependencies, or after a major refactor that touched authentication or data handling. It's also a useful check when you onboard onto a new codebase — you may find issues that have been there for years.

### How it works

`/security` runs four sequential checks on your Mac via SSH:

1. **Dependency audit** — runs `npm audit --json`, `pip-audit`, or `cargo audit` depending on detected package managers. Parses severity and affected packages.
2. **Pattern scan** — greps your source files for ~40 common vulnerability patterns: hardcoded secrets (`sk-`, `ghp_`, `AKIA`, `password =`), dangerous function calls (`eval()`, `exec()`, `shell=True`), SQL injection patterns (`"SELECT " + var`), and XSS vectors (`innerHTML =`).
3. **Secret files check** — runs `git log --all --name-only` to find `.env`, `.env.local`, `.env.production`, `.pem`, and other secret file patterns that appear anywhere in the git history, even if deleted.
4. **`.gitignore` gaps** — compares your `.gitignore` against a list of 60+ common patterns that should be excluded (`.env*`, `*.pem`, `node_modules/`, `__pycache__/`, `.DS_Store`, etc.).

All four results are sent to Claude, which produces a prioritized list: **Critical** (fix before deploying), **High** (fix this sprint), **Medium** (schedule fix), **Low** (informational).

### Step-by-step walkthrough

**Step 1 — Run the scan.**

```
/security
```

The scan runs as a background job — typically 30–90 seconds depending on project size and number of dependencies.

**Step 2 — Review the prioritized report.**

When complete, the report appears in the chatroom. A typical output looks like:

```
## Security Scan Results

### Critical
- Hardcoded API key found in src/config.js:14 — "openai_key = 'sk-...'"
- .env.production found in git history (commit abc1234, 6 months ago)

### High
- 2 critical npm vulnerabilities: lodash <4.17.21 (Prototype Pollution), axios <1.6.0 (SSRF)
- SQL query concatenation in src/db/users.js:87

### Medium
- eval() with user input in src/utils/parser.js:23
- Missing .gitignore entries: *.pem, .env.local, .DS_Store

### Low
- shell=True in scripts/deploy.py:12 (safe if input is controlled)
```

**Step 3 — Fix Critical and High issues first.**

For each Critical issue, Claude includes a one-line fix suggestion. Tap **Apply fix** to dispatch a background job that makes the change, or handle it manually in the terminal.

**Step 4 — Rotate exposed secrets immediately.**

If any hardcoded API keys or secret files are found in the git history, rotate those secrets in your provider immediately — even if the commits are old. The secret is part of git history and can be recovered.

### Tips and best practices

- **Run before every major release**: Add it to your pre-release checklist alongside running tests.
- **Fix .gitignore gaps early**: A missing `.gitignore` entry is cheap to fix before secrets are committed — expensive to fix after.
- **Rotate first, then remove**: If a secret is in git history, removing the file doesn't help — the secret must be rotated in the external service first.
- **Combine with `/tribal`**: `/tribal` often surfaces "we know about this but haven't fixed it" context that explains why certain patterns exist in the codebase.

---

## /tribal — Tribal Knowledge Extraction

### When to use it

Use `/tribal` before onboarding new team members, before starting a major refactor of an unfamiliar module, or before handing off a project. It's most valuable on codebases older than 6 months where institutional knowledge has accumulated but not been written down.

### How it works

`/tribal` submits a background job that instructs Claude to analyze your project as a senior developer who has worked on it for years. It reads:
- Commit history for decision rationale
- Code comments for gotchas and TODOs
- File structure for architectural patterns
- Import graphs for hidden dependencies
- Naming conventions across all files
- README and any existing documentation

Claude then synthesizes findings into a structured document covering the categories below.

### Step-by-step walkthrough

**Step 1 — Run `/tribal` from the chatroom.**

```
/tribal
```

Or for a specific subdirectory:

```
/tribal ~/projects/myapp/payments-service
```

**Step 2 — Wait for the background job.**

`/tribal` is one of the longer-running commands — expect 3–8 minutes for a medium-sized project. You'll get a notification when it finishes.

**Step 3 — Review the output in the Jobs tab.**

Open the Jobs tab and tap the tribal knowledge job to see the full result. The output is structured as:

- **Architecture decisions**: "The event bus pattern was chosen over direct service calls in 2022 to decouple the notification service from billing. Do not add direct imports between them."
- **Gotchas for new developers**: "The `user.id` field is a string UUID in the API but an integer in the database. The mapping happens in `UserSerializer` — never use them interchangeably."
- **Hidden dependencies**: "The analytics module reads from the session store directly — sessions must be initialized before analytics middleware."
- **Naming conventions**: "`handle*` = event handler (sync), `process*` = async worker, `build*` = factory, `validate*` = throws on failure."
- **Performance constraints**: "The `loadUserPermissions()` call is expensive. Results are cached in Redis for 5 minutes — don't call it more than once per request."
- **Historical context**: "The v1 API layer in `src/v1/` is deprecated but still receives 20% of traffic. Don't remove it without a migration plan."
- **Testing gaps**: "The payment module has no integration tests — unit tests mock the Stripe client. Real payment flows are only tested manually."

**Step 4 — Save and distribute.**

Copy the output to a `CONTRIBUTING.md`, a Notion page, or a project wiki. You can also ask Claude to format it as onboarding documentation for a specific audience:

```
Format the tribal knowledge output as a one-hour onboarding guide for a junior developer
```

### Tips and best practices

- **Run on unfamiliar codebases**: When you first join a project, run `/tribal` before your first meeting with the team — you'll ask much better questions.
- **Repeat quarterly**: Tribal knowledge drifts as code changes. Run `/tribal` every quarter on active projects and diff the output against the previous version.
- **Combine with `/graph`**: After `/tribal` surfaces a "hidden dependency", use `/graph` to trace exactly how many files are affected.

---

## /spec — Spec-Driven Development

### When to use it

Use `/spec` before implementing any feature that has ambiguous requirements, touches multiple files, or will be reviewed by stakeholders. The spec step catches scope creep, missing edge cases, and ambiguous acceptance criteria before any code is written — which is significantly cheaper than catching them during review.

### How it works

`/spec` sends your feature description to Claude along with your project's file structure and existing patterns. Claude generates a formal specification using EARS (Easy Approach to Requirements Syntax) notation, which describes requirements in structured "When/The system shall" or "While/The system shall" format. The spec also includes a file change list, ordered implementation tasks, acceptance criteria, and identified risks.

### Step-by-step walkthrough

**Step 1 — Write a feature description.**

Be specific about what the feature does but not how:
```
/spec Add rate limiting to all public API endpoints — max 100 requests per minute per IP, with a 429 response and Retry-After header
```

**Step 2 — Review the generated spec.**

The spec appears in the chatroom. A typical spec for the example above includes:

**EARS requirements:**
```
WHEN an authenticated request arrives,
  THE SYSTEM SHALL track it against the user's rate limit bucket.

WHEN a request's IP address exceeds 100 requests in a 60-second window,
  THE SYSTEM SHALL return HTTP 429 with a Retry-After header set to the
  seconds remaining until the window resets.

WHILE the rate limit window is active,
  THE SYSTEM SHALL increment the request counter atomically to prevent
  race conditions under concurrent load.
```

**Files to create/modify:**
```
CREATE: src/middleware/rateLimiter.ts
MODIFY: src/app.ts — register middleware before route handlers
MODIFY: src/config/defaults.ts — add RATE_LIMIT_MAX and RATE_LIMIT_WINDOW
```

**Ordered tasks:**
```
1. Add Redis connection to config (prerequisite for distributed rate limiting)
2. Implement RateLimiter class with sliding window algorithm
3. Write unit tests for RateLimiter
4. Register middleware in app.ts before all public routes
5. Add integration test for 429 response
6. Update API documentation with rate limit headers
```

**Risks identified:**
```
- If Redis is unavailable, rate limiting will fail open (all requests pass). Consider a fail-closed mode for sensitive endpoints.
- The sliding window algorithm requires atomic operations — use Lua scripts in Redis to avoid race conditions.
```

**Step 3 — Approve or refine the spec.**

If the spec looks right, reply "execute" or tap **Execute with agents**. CatClaw dispatches a `/batch` run where each task in the ordered list becomes a worker job.

If the spec is incomplete, refine it:
```
Also add a per-user rate limit for authenticated requests, separate from the IP limit
```

Claude updates the spec with the new requirements before you execute.

**Step 4 — Monitor execution.**

Open the Jobs tab to watch the batch agents work through the spec tasks in order. Each task's result appears as a job card with a trajectory timeline.

### Tips and best practices

- **Write specs before starting any session with agents**: Specs prevent agents from making reasonable-but-wrong assumptions about scope.
- **Use EARS requirements in PR descriptions**: Paste the EARS requirements from the spec into your PR description — they make excellent acceptance criteria for reviewers.
- **Chain `/spec` → `/gentest`**: After executing a spec, use `/gentest` to generate tests for each acceptance criterion. The tests become your regression suite for the feature.
- **Save specs as skills**: After a successful spec execution, use the auto-skill suggestion banner to save the spec pattern as a reusable skill — useful for recurring feature types (e.g., "add a new API endpoint with rate limiting and tests").

---

## /graph — Codebase Knowledge Graph

### When to use it

Use `/graph` when you need to understand how something works across multiple files, find all places that depend on a module before changing it, or trace how data flows through your system. It's faster than grep for architectural questions because it understands relationships — not just text matches.

### How it works

`/graph` builds a lightweight in-memory graph of your project by reading:
- File system structure
- Import/require/include statements
- Class, function, and type definitions
- Export declarations

It then uses this graph to answer your question with specific file paths and line numbers. For large projects, it reads the most relevant files based on the question rather than the entire codebase.

### Step-by-step walkthrough

**Step 1 — Ask an architectural question.**

```
/graph how does the payment flow work from checkout to confirmation
```

**Step 2 — Read the traced answer.**

The result is a narrative with file references:

```
Payment flow (checkout → confirmation):

1. CheckoutController.submitOrder() [src/controllers/CheckoutController.ts:87]
   → validates cart contents via CartService.validate() [src/services/CartService.ts:34]

2. OrderService.create() [src/services/OrderService.ts:156]
   → persists order to DB with status "pending"
   → emits OrderCreated event [src/events/OrderEvents.ts:12]

3. PaymentService.charge() [src/services/PaymentService.ts:78]
   → calls Stripe API via StripeClient [src/clients/StripeClient.ts:23]
   → on success: emits PaymentConfirmed event
   → on failure: emits PaymentFailed event, updates order status to "failed"

4. OrderConfirmationHandler [src/handlers/OrderConfirmationHandler.ts:45]
   → listens for PaymentConfirmed
   → calls NotificationService.sendConfirmationEmail() [src/services/NotificationService.ts:112]
   → updates order status to "confirmed"
```

**Step 3 — Drill into a specific part.**

```
/graph what else calls PaymentService.charge besides CheckoutController
```

This returns every call site, which lets you assess the impact of any change to `PaymentService.charge()`.

### Common queries

| Query | When to use |
|-------|-------------|
| `/graph what depends on UserService` | Before changing a core service |
| `/graph trace where user.email is used after signup` | Data privacy audit |
| `/graph find all places that handle authentication` | Security review |
| `/graph where should I add logging middleware` | Architecture guidance |
| `/graph why does module A import from module B` | Dependency explanation |

### Tips and best practices

- **Use before a major refactor**: Run `/graph what depends on X` before deleting or renaming anything. Knowing the blast radius prevents broken builds.
- **Combine with `/spec`**: Use `/graph` to understand the existing architecture before writing a spec — this way the spec references real file names and follows existing patterns.
- **Ask "why" questions**: `/graph why does the notification module import from billing` is often more useful than knowing the import exists, because it explains intent.

---

## /hooks — Agent Hooks

### When to use it

Use `/hooks` to set up automated background jobs that fire whenever specific files change. The most common use case is auto-running tests on every save — it turns the Jobs tab into a continuous feedback panel without any manual intervention.

### How it works

`/hooks` registers a named watcher on your Mac. CatClaw polls for file modifications every 10 seconds by checking `mtime` on files matching your glob pattern. When a match is found, it dispatches a background job with your action string. A 30-second cooldown prevents the same hook from firing more than twice per minute regardless of how many files change.

### Step-by-step walkthrough

**Step 1 — Add a hook.**

```
/hooks add auto-test "**/*.swift" "run the unit tests for any files that changed and report failures"
```

The hook activates immediately. You'll see a confirmation message with the hook name, pattern, and cooldown.

**Step 2 — Work normally.**

Save a Swift file in Xcode. Within 10–20 seconds, a new background job appears in the Jobs tab with the action you specified. The job runs on your Mac via SSH exactly as if you had typed `/submit run the unit tests...`.

**Step 3 — Monitor in the Jobs tab.**

Open the Jobs tab. You'll see hook-triggered jobs listed with a hook indicator. Each job has a full trajectory timeline showing exactly which tests ran and which failed.

**Step 4 — Get notified on failure.**

Combine with a notification:
```
/notify when tests fail
```

Now you'll get a push notification whenever a hook-triggered test job finds failures — no need to check the Jobs tab manually.

**Step 5 — Manage your hooks.**

List active hooks:
```
/hooks list
```

Remove a specific hook:
```
/hooks remove auto-test
```

Stop all hooks before a large refactor:
```
/hooks stop
```

### Advanced hook patterns

**Documentation sync hook:**

```
/hooks add doc-sync "src/**/*.ts" "check if the README or docs/api.md need updating based on recent changes, and suggest specific edits"
```

Every time a TypeScript source file changes, Claude reviews the diff and checks whether the API documentation is still accurate.

**Dependency audit hook:**

```
/hooks add dep-audit "package.json" "run npm audit and report any new critical or high vulnerabilities"
```

Fires whenever `package.json` changes — catches newly added vulnerable packages immediately.

**Database migration check:**

```
/hooks add migration-check "*.sql" "check if the new migration is reversible and if there are any destructive changes"
```

Every new SQL migration file gets an automatic review before it's committed.

### Tips and best practices

- **Use specific patterns**: `"src/**/*.test.ts"` is better than `"**/*"` to avoid triggering on build artifacts and generated files.
- **Stop hooks before bulk operations**: If you're about to run a formatter on 200 files, stop your hooks first with `/hooks stop` — otherwise you'll dispatch 20 jobs in rapid succession.
- **Name hooks descriptively**: You may run 3–4 hooks simultaneously. Names like `swift-test`, `ts-lint`, `doc-sync` are easier to manage than `hook1`, `hook2`.
- **Cooldown is per-hook**: Each named hook has its own 30-second cooldown. Two different hooks on the same file will both fire independently.

---

## Combining Commands: Real-World Pipelines

The real power of these commands comes from combining them. Here are three pipelines that cover common development scenarios.

### Pipeline 1: Pre-Release Checklist

Run this sequence before any production release:

```
/security
```
Fix Critical and High findings.

```
/changelog v1.5.0..HEAD
```
Generate release notes and paste into GitHub release draft.

```
/health
```
Get a codebase snapshot (LOC, TODOs, uncommitted changes) to include in the release review.

### Pipeline 2: New Feature — Spec to Tests

Use this when starting any non-trivial feature:

```
/spec Add webhook delivery with retry logic and failure logging
```
Review the EARS spec and acceptance criteria. Execute with agents.

```
/gentest webhook delivery including retry backoff and permanent failure handling
```
Generate tests for each acceptance criterion. Review and approve.

```
/submit run the new webhook tests and fix any failures
```
Background job runs the tests and fixes import or syntax errors in the generated test code.

### Pipeline 3: Onboarding Pipeline — New Project or New Hire

Run this when joining a project or preparing for a new team member:

```
/tribal
```
Extract unwritten knowledge (architecture decisions, gotchas, naming conventions).

```
/graph what are the main modules and how do they relate to each other
```
Get a high-level architectural map with file references.

```
/security
```
Run a baseline security audit to understand existing debt.

```
/changelog HEAD~100..HEAD
```
Get a summary of the last 100 commits to understand what has changed recently.

Combine all four outputs into an onboarding document — you'll have covered architecture, history, risks, and unwritten knowledge in under 15 minutes.

### Pipeline 4: Continuous Quality Loop

Set this up at the start of a coding session and leave it running:

```
/hooks add auto-test "**/*.swift" "run unit tests for changed files"
/hooks add lint "**/*.swift" "run SwiftLint on changed files and report violations"
/notify when tests fail
```

Every file save triggers a test run and a lint check. You get a push notification only when something fails — the rest of the time, CatClaw works silently in the background.

At the end of the session:

```
/hooks stop
/changelog HEAD~20..HEAD
```

Stop the hooks and generate release notes from the session's commits.
