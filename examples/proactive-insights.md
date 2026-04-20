# Proactive AI Insights

> Four background detectors look for patterns in your activity and push a notification when something unusual surfaces. All local, all opt-in, max one insight per 72h per detector.

## Enable

**Settings → AI Intelligence → Proactive Insights**

1. Master toggle on
2. Grant notification permission when prompted
3. (Optional) toggle individual detectors off if any are noisy

All default to off.

---

## The four detectors

### WeekdayFailureRate

Fires when one day of the week has a significantly higher job failure rate than others.

**Example notification:**

> Your Thursday job failure rate is 34% — 3x higher than other days. Worth checking if a Thursday pipeline has a recurring issue.

Tap → details sheet with per-day breakdown + Accept / Dismiss.

### CommandRepetition

Fires when you have manually run the same command 3+ days in a row with no scheduled version.

**Example notification:**

> You have run `git fetch && git log origin/main..HEAD` manually 4 days in a row. Want to schedule it as a morning job?

Accept → opens the `/workflow save` flow pre-filled.

### SessionLengthTrend

Fires when average session length shifts >30% week over week.

**Example notification:**

> Your sessions averaged 45 min last week, 90 min this week. Crunch mode, or something else?

A health signal, not a performance one.

### CostSpike

Fires when last-24h spend exceeds 2× your 14-day baseline.

**Example notification:**

> Token spend spiked yesterday ($4.20 vs $1.10 daily average). Main driver: 8 `/race` runs on Opus.

---

## How it runs

- Executes during the nightly **BGProcessingTask** slot (~2 AM), same as Dream Mode
- 100% local — no network, no API calls
- Runtime 2–5 seconds per night
- Notifications fire only when confidence ≥ 0.75
- 72-hour dedup per detector — you will not get the same insight twice in three days

---

## The notification sheet

Tap any insight notification and you see:

```
WeekdayFailureRate

Thursday failure rate: 34% (17 of 50 jobs)
Other weekdays average: 11% (23 of 204 jobs)
Confidence: 0.91

Likely causes:
  • Thursday-specific deploys
  • End-of-week test flakiness
  • Weekend config drift

Suggested action: check your Thursday scheduled jobs

[ Dismiss ]  [ Accept — open Jobs tab ]
```

Accept → action happens (opens Jobs, opens workflow, etc.)
Dismiss → insight goes away, 72h dedup window starts

---

## Example weekly rhythm

On a typical busy week:

- Monday morning: "Command repetition — you ran `/health` 4 mornings in a row. Schedule it?"
- Tuesday: (nothing, dedup window)
- Wednesday: "Cost spike — spend is 3x baseline. `/race` is the main driver."
- Thursday: (nothing)
- Friday: "Session length trending longer — 90 min avg vs. 45 last week."
- Saturday: (nothing)

Three insights, three opportunities to act — without needing to ask.

---

## Privacy

- All analysis is local
- Loads persisted chatroom metadata and job counters only
- No message contents leave the device
- No external analytics of any kind
- You can see the full reasoning in the insight sheet before acting

---

## When to disable

- You get insights you consistently dismiss as irrelevant — turn off the specific detector
- Notification noise is high — `/notify` rules may already cover what you care about
- You are in a debug/research phase where patterns are expected to be irregular

## Tips

- **Start with all four on.** Typical volume is 2–5 insights per week total. You will not be spammed.
- **Accept freely** — Accept actions (schedule a command, open Jobs) are low-risk and often worth trying.
- **Pair with Dream Mode.** Dream learns your preferences. Insights learns your patterns. Different layers of self-awareness.
- **CostSpike is the one most worth enabling.** Budget awareness is hard to maintain manually.
