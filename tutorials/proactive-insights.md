# Proactive AI Insights — Tutorial

> CatClaw watches patterns in the background — when your jobs fail more often on Thursdays, when you keep running the same command three nights in a row, when your cost just spiked — and tells you about it. Overnight, via push notification, opt-in.

---

## What It Does

Most AI features are reactive. You ask; they answer. Proactive Insights is the opposite: four background detectors watch your activity patterns and fire a notification when they spot something you would probably want to know.

The four detectors shipped so far:

| Detector | Fires when |
|----------|------------|
| **WeekdayFailureRate** | Your job failure rate on a specific weekday is significantly higher than other days |
| **CommandRepetition** | You have run the same command manually 3+ days in a row (candidate for a scheduled job) |
| **SessionLengthTrend** | Your session lengths are trending significantly longer or shorter week over week |
| **CostSpike** | Token spend in the last 24 hours is more than 2× your 14-day baseline |

All run locally, during the dream BGProcessingTask slot (approximately 2 AM nightly).

Each detector is opt-in, fires only when confidence ≥ 0.75, and dedups aggressively — you will not get the same insight twice within 72 hours.

---

## Example Insights

You open your phone Tuesday morning and see:

> **CatClaw Insight**
> Your Thursday job failure rate (34%) is 3x higher than other days. Might be worth checking if your Thursday deploy pipeline has a recurring issue.

Or:

> **CatClaw Insight**
> You have run `git fetch origin && git log origin/main..HEAD` manually 4 days in a row. Want to schedule it as a morning job?

Or:

> **CatClaw Insight**
> Token spend spiked yesterday ($4.20 vs. $1.10 daily average). Main driver: 8 `/race` runs on Opus.

Each notification has an Accept / Dismiss action. Accept expands to a sheet with:
- Full detail of the pattern
- The underlying data points
- A suggested action (e.g. "Schedule this command" or "Reduce `/race` usage")
- A "Why am I seeing this?" link explaining the detector logic

---

## Enabling Proactive Insights

1. Open **Settings → AI Intelligence → Proactive Insights**
2. Toggle the master switch on
3. Grant notification permission when iOS prompts (insights are delivered as notifications)
4. (Optional) Toggle individual detectors off if a specific one is noisy

Defaults to **off**. You opt in with clear awareness of what it does.

---

## How It Works Under the Hood

Every night when the existing `BGProcessingTask` fires for Dream Mode analysis, CatClaw now also runs the insights engine:

1. Load persisted chatroom history and job metadata (local only)
2. For each enabled detector, compute a metric over the relevant window
3. Compare against thresholds and confidence cutoffs
4. If threshold crossed and confidence ≥ 0.75 and no identical insight was fired in the last 72 hours → schedule a local notification

Typical runtime: 2–5 seconds. Zero network calls. Zero API cost.

---

## The Four Detectors in Detail

### WeekdayFailureRate

Groups the last 30 days of jobs by day-of-week. Computes failure rate per day. Fires when one day's rate is 2x higher than the 7-day average AND the sample size is significant (≥10 jobs on that day).

Typical catch: "Monday deploys fail more often because of weekend config drift" — a pattern you might not notice without a detector.

### CommandRepetition

Tracks which slash commands are run manually (not from `/workflow` or `/submit --schedule`). Fires when the same command (normalized, ignoring arguments) appears on 3+ consecutive days with no scheduled version.

Typical catch: "You keep running `/health` every morning. Want it scheduled?"

### SessionLengthTrend

Compares the last 7 days' average session length to the prior 7 days. Fires when the change is >30% in either direction.

Typical catch: "Your sessions were 45m average last week, 90m average this week. Crunch mode?" — a health signal, not a performance one.

### CostSpike

Rolling 14-day baseline of daily spend. Fires when the last 24h exceeds 2× baseline. The insight includes the top 3 contributing commands or models.

Typical catch: "Opus usage jumped because you kicked off 3 long `/team` runs with --routing quality."

---

## Tips

- **Start with all four on.** Insights are rare (typical: 2–5 per week). You will not be spammed.
- **Dismiss liberally.** If a detector fires an insight that does not match your actual situation, dismiss it. The dedup window will keep it quiet.
- **Use the "Schedule this" action from CommandRepetition insights.** One tap to turn a manual habit into a `/workflow` or scheduled job.
- **Calibrate CostSpike to your patterns.** If you routinely have spiky spend days (e.g. you do big batches on Fridays), the detector may be noisier — turn it off and rely on `/cost` manually.
- **Proactive Insights + Dream Mode work well together.** Dream learns your preferences; Insights learns your *patterns*. Different layers of self-awareness.

Most AI tools are good at answering questions. Proactive Insights is about surfacing the questions you did not know to ask.
