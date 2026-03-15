# Scheduling Recurring Background Jobs

> Automate repetitive tasks by attaching a repeating schedule to any background job. ClawTerminal will re-submit the job at the interval you choose --- hourly, daily, weekly, or custom.

---

## What Are Scheduled Jobs?

A standard `/submit` job is a one-shot task: you send it, Claude runs it, and you get the result. A **scheduled job** adds a repeating timer on top of that. Once configured, ClawTerminal automatically clones and re-submits the job at each scheduled interval --- no manual action required.

Common use cases:

- **Daily code health checks** --- "Run the test suite and report any failures"
- **Hourly log monitoring** --- "Tail the last 50 lines of `/var/log/app.log` and summarize errors"
- **Weekly dependency audits** --- "Check for outdated npm packages and list security advisories"
- **Periodic backups** --- "Archive `~/Projects/myapp` to `~/Backups/` with today's date"

---

## Setting Up a Schedule

### Step 1 --- Submit a Job

First, create a normal background job using the `/submit` slash command in any chatroom:

```text
/submit run the test suite and report any failures
```

The job appears in the **Jobs** panel (tap the clipboard icon in the chat toolbar).

### Step 2 --- Open the Schedule Editor

1. In the **Jobs** panel, find the job you want to schedule
2. **Long-press** the job row to open the context menu
3. Tap **Set Schedule**

The **Schedule Editor** sheet appears.

### Step 3 --- Configure Frequency

Toggle **Enable Schedule** on, then choose your repeat interval:

| Frequency | Interval | Notes |
|-----------|----------|-------|
| **Hourly** | Every 60 minutes | Good for monitoring tasks |
| **Daily** | Every 24 hours | Good for health checks, reports |
| **Weekly** | Every 7 days | Good for audits, cleanup |
| **Custom** | 5 -- 10,080 minutes | Set any interval, minimum 5 minutes |

For **Custom**, use the stepper to set the exact number of minutes between runs.

### Step 4 --- Set the Start Time

Use the **First run** date picker to choose when the first scheduled execution should fire. All subsequent runs are calculated from this start time based on your chosen frequency.

### Step 5 --- Set a Run Limit (Optional)

In the **Max runs** field, enter a number to cap how many times the job repeats. Leave it blank for unlimited runs.

| Max runs | Behavior |
|----------|----------|
| Blank | Runs indefinitely until you pause or delete the schedule |
| `5` | Runs exactly 5 times, then stops automatically |
| `1` | Runs once at the scheduled time (delayed one-shot) |

### Step 6 --- Save

Tap **Save**. The job row in the Jobs panel now shows a teal clock badge with a countdown to the next run.

---

## How the Scheduler Works

ClawTerminal runs a **60-second tick loop** (the `ScheduledJobManager`) whenever you are connected to your Mac via SSH:

1. Every 60 seconds, the scheduler checks all jobs with an active schedule
2. For each job where `nextRunAt` is in the past (i.e. the job is due), it:
   - Advances the `nextRunAt` to the next interval (prevents double-firing)
   - Increments the run counter
   - Clones the job's request text and submits it as a brand-new one-shot `/submit` job
3. The cloned job runs exactly like a normal background job --- in its own tmux window on your Mac, with full streaming progress and push notification on completion

> **Important:** The scheduler requires an active SSH connection. If you disconnect from your Mac, scheduled jobs will not fire until you reconnect. See the section on background notifications below for what happens when the app is suspended.

---

## Monitoring Scheduled Jobs

### Clock Badge

Jobs with an active schedule display a teal **clock badge** in the job row:

- `in 45m` --- next run in 45 minutes
- `in 2h 15m` --- next run in 2 hours and 15 minutes
- `overdue` --- the scheduled time has passed but the job has not fired yet (usually because SSH was disconnected)

### Run History

Each time a scheduled job fires, it creates a new standalone job entry in the Jobs panel. You can:

- View the result of each individual run
- Compare results across runs
- Save any run's log as Markdown

### Editing a Schedule

To modify an existing schedule:

1. Long-press the scheduled job row
2. Tap **Edit Schedule**
3. Adjust the frequency, start time, or max runs
4. Tap **Save**

### Pausing a Schedule

To temporarily stop a schedule without deleting it:

1. Long-press the job row
2. Tap **Edit Schedule**
3. Toggle **Enable Schedule** off
4. Tap **Save**

The clock badge disappears, and no new runs will fire. Toggle it back on to resume.

### Deleting a Scheduled Job

Swipe left on the job row and tap **Delete**, or use the context menu. This removes both the schedule and the template job. Previously completed runs remain in the Jobs panel as standalone entries.

---

## Background Notifications

When ClawTerminal is suspended (app is in the background or the screen is locked), iOS does not allow the scheduler to run continuously. Instead, ClawTerminal uses **iOS Background App Refresh** to check for due jobs:

1. When the app enters the background, it registers a `BGAppRefreshTask`
2. iOS wakes the app approximately every 15 minutes (timing is at iOS's discretion)
3. On wake, ClawTerminal reads the persisted job data and counts how many scheduled jobs are overdue
4. If any jobs are due, a local notification is posted: **"N scheduled job(s) ready to run. Open ClawTerminal to execute."**
5. Tapping the notification opens the app, which reconnects to SSH and fires the due jobs

> **Note:** Background App Refresh cannot establish an SSH connection or run Claude. It can only notify you. The actual job execution happens when you open the app and the SSH connection is active.

---

## Tips

- **Start small:** Test your scheduled task as a one-shot `/submit` first. Once you confirm it works, attach a schedule.
- **Use max runs for experiments:** Set `maxRuns = 3` to trial a schedule before committing to unlimited.
- **Custom intervals:** The minimum custom interval is 5 minutes. For very frequent checks, consider whether a single long-running job with a loop might be more efficient.
- **Multiple schedules:** You can have multiple scheduled jobs active simultaneously across different chatrooms. Each chatroom has its own job store and scheduler.
- **Model selection:** Scheduled jobs use whatever model is selected when the clone fires. If you switch models with `/model`, future scheduled runs use the new model.

---

## Example: Daily Test Suite Report

```text
/submit run `npm test` in ~/Projects/myapp and summarize the results.
If any tests fail, list the failing test names and the error messages.
```

After the first run completes successfully:

1. Long-press the job in the Jobs panel
2. Tap **Set Schedule**
3. Toggle **Enable Schedule** on
4. Set **Frequency** to **Daily**
5. Set **First run** to tomorrow at 9:00 AM
6. Leave **Max runs** blank
7. Tap **Save**

Every day at 9:00 AM (when ClawTerminal is open and connected), Claude will run your test suite and deliver a summary.
