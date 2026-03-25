# Background Jobs Examples

Background jobs let Claude work on long tasks on your Mac while you put your phone away. You get a push notification when the work is done. All examples use the `/submit` command.

---

## 1. Submit a simple job

Send a task to run in the background. ClawTerminal opens the chatroom, submits the task to Claude, and returns immediately — you don't have to wait for a response.

**Example: Write unit tests**
```
/submit Write comprehensive unit tests for src/auth/login.py. Cover happy path, invalid credentials, account lockout, and password reset. Use pytest.
```

**Example: Audit for security issues**
```
/submit Review the entire codebase for security vulnerabilities. Focus on SQL injection, hardcoded secrets, insecure deserialization, and missing input validation. Write a prioritized report.
```

**Example: Update documentation**
```
/submit Read all Python files in src/ and update the docstrings to match the actual function signatures. For functions with no docstring, add one.
```

**What happens:**
- The job appears in the Jobs panel (clipboard icon in chat toolbar) with a "Running" badge
- Claude works on your Mac in a tmux window
- When done, you get a push notification: "Job complete: Write unit tests for src/auth/login.py"
- Tap the notification to open ClawTerminal and read the result
- The result is automatically injected into the chatroom context for your next message

---

## 2. Submit with automatic checkpoints

Add `--ckpt` to track progress as Claude works. Useful for long jobs where you want to know how far along Claude is without reading the full log.

**Example: Refactor with progress tracking**
```
/submit --ckpt Refactor the database layer to use the repository pattern. Create a BaseRepository class, then move all raw SQL queries in src/db/ into typed repository classes.
```

**What you see in Jobs > Job Detail:**

```
  Checkpoint timeline
  •  Analyzed existing database layer (14 files)       2:03 PM
  •  Created BaseRepository class                       2:07 PM
  •  Write: src/db/repositories/user_repo.py           2:09 PM
  •  Write: src/db/repositories/order_repo.py          2:11 PM
  •  Write: src/db/repositories/product_repo.py        2:14 PM
  •  All repository classes complete                    2:18 PM
```

**Example: Multi-phase migration**
```
/submit --ckpt Migrate the app from Flask to FastAPI.
Phase 1: audit all Flask-specific code.
Phase 2: rewrite each route handler.
Phase 3: update tests to use the FastAPI test client.
Phase 4: update requirements.txt and README.
Output [CHECKPOINT: Phase N complete] after each phase.
```

The `[CHECKPOINT: label]` markers you include in the prompt appear in the timeline alongside the automatically detected tool-use events.

---

## 3. Submit with skills attached

Use `--skills` to inject a skill's system prompt into the job's context. Skills provide domain knowledge that shapes how Claude approaches the task.

**Example: Write tests with TDD skill active**
```
/submit --skills tdd Add the login feature to src/auth/. Start with failing tests, then implement the minimum code to pass them.
```

**Example: Security audit with security skill**
```
/submit --skills security Review the payment processing module in src/payments/. Flag any PCI-DSS violations, unsafe crypto usage, and logging of sensitive data.
```

**Example: Multiple skills**
```
/submit --skills backend,testing Implement rate limiting on all public API endpoints. Add tests that verify the limits are enforced.
```

**Enable skills first:**
Tap the Skills button (wand icon) in the chat toolbar to browse and enable skills. Installed skills appear in the palette when you type `--skills` and start typing a name.

---

## 4. Check job status in the Jobs panel

Open the Jobs panel anytime by tapping the **clipboard icon** in the chat toolbar.

**What you see:**

- Each job row shows: task preview, status badge (Running / Complete / Failed), and elapsed time
- **Scheduled jobs** show a teal clock badge with a countdown to the next run
- **Batch/orchestration groups** are collapsible — tap the group header to expand individual agent jobs

**Tap a job row to open Job Detail:**
- Request text (what you asked)
- Result (Claude's output)
- Checkpoint timeline (if `--ckpt` was used)
- Tool used (Claude, Codex, Gemini, Aider)
- Timestamps

**Long-press a job row** to open the context menu:
- Set Schedule / Edit Schedule
- Copy result
- Delete

---

## 5. Scheduled jobs — run the same task automatically

Turn any background job into a repeating schedule: hourly, daily, weekly, or a custom interval.

### Example: Daily test suite report at 9 AM

**Step 1 — Submit the job once to verify it works:**
```
/submit Run `pytest --tb=short -q` in ~/Projects/myapp and summarize the results. If any tests fail, list the test names and error messages.
```

**Step 2 — After it completes successfully, set a schedule:**
1. Open Jobs panel
2. Long-press the job row
3. Tap **Set Schedule**
4. Enable Schedule: on
5. Frequency: **Daily**
6. First run: tomorrow at 9:00 AM
7. Max runs: blank (unlimited)
8. Tap Save

A teal clock badge appears: `in 14h 23m`.

Every morning at 9 AM (when ClawTerminal is open and SSH is connected), Claude runs your test suite and delivers a summary.

### Example: Hourly log monitor

```
/submit Tail the last 50 lines of ~/logs/app.log. Summarize any ERROR or WARN entries. If there are none, just say "all clear".
```

Set schedule: **Hourly**, max runs: blank.

### Example: Weekly dependency audit

```
/submit Run `npm audit` and `npm outdated` in ~/Projects/myapp. List any packages with high/critical vulnerabilities and suggest which ones to update first.
```

Set schedule: **Weekly**, first run: Monday at 10 AM.

### Example: Custom interval — every 4 hours

Set schedule: **Custom**, interval: `240` minutes.

---

## 6. Notification actions

When a job completes, the push notification has two action buttons (swipe or long-press the notification):

- **View Result** — opens ClawTerminal directly to the job result
- **Copy Result** — copies the full result text to clipboard, so you can paste it anywhere without opening the app

---

## Tips

- **Test first, schedule second.** Always run a job as a one-shot first to confirm it works. Then attach a schedule.
- **Use `--ckpt` on anything over 2 minutes.** It costs nothing extra and gives you a live progress view.
- **The result is injected automatically.** After a job completes, the next message you send in that chatroom already has the job result in Claude's context — you don't need to paste it.
- **Jobs survive disconnection.** The Mac's tmux session keeps running even if your iPhone loses WiFi. Reconnect later and poll for the result.
