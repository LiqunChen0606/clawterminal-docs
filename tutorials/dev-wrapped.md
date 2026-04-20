# Dev Wrapped — Tutorial

> Spotify Wrapped, but for your code week. `/wrapped` produces a swipeable 9-page recap of your developer activity — top commands, peak hour, top topics, longest session, personality stats, and a final share card.

---

## What It Does

`/wrapped` scans the last 7 or 30 days of your chatroom activity, background jobs, and git commits. It then produces a Spotify-Wrapped-style interactive recap — 9 full-screen pages you swipe through, each highlighting one dimension of your week.

It is part data visualization, part personal retrospective, part dopamine hit.

---

## Getting Started

### Last 7 days

```
/wrapped
```

or explicitly:

```
/wrapped week
```

### Last 30 days

```
/wrapped month
```

The recap takes about 2–3 seconds to generate. Then the swipeable story opens full-screen.

---

## The 9 Pages

### Page 1 — The Opener

Big title card with date range and a headline stat ("You shipped 127 messages and 8 jobs this week").

### Page 2 — Activity Heatmap

A week-by-hour heatmap showing when you worked. Deep purple where you were most active. Reveals whether you are a morning, afternoon, or night developer.

### Page 3 — Top Commands

Your top 5 most-used slash commands, ranked, with counts. Usually surprising — "I had no idea I ran `/submit` 34 times."

### Page 4 — Spend

Your total token spend for the period, model-by-model breakdown. Bar chart. Same data as `/cost` but prettier.

### Page 5 — Peak Hour

"Your peak hour was Tuesday at 10 AM — you sent 18 messages and kicked off 3 jobs." Identifies your single most productive hour in the period.

### Page 6 — Top Topics

The five topics you discussed most (extracted from chatroom content by topic clustering). Things like "Stripe webhooks", "SwiftUI animations", "auth refactor". This page often reveals what you actually cared about, which may not match what you thought you were working on.

### Page 7 — Agent Personality

If you used `/personality` at all, shows which persona you invoked most. If you did not, shows your *implied* personality based on interaction style (pushy / patient / skeptical / curious).

### Page 8 — Longest Session

The single chatroom session you spent the most continuous time in. Shows date, duration, message count, and the closing message (often a commit like "all tests green").

### Page 9 — Share Card

A composite summary image — your top 4 stats on one gradient postcard. Tap the share button to send via iMessage, Twitter, Slack, etc. It is rendered using the same Code Postcard engine (indigo→purple gradient, ClawTerminal wordmark).

---

## How Data Is Collected

All analysis is **local**. No API calls.

- **Activity** — parsed from `chatrooms.json` message timestamps
- **Commands** — grepped from user message text
- **Spend** — summed from per-job cost metadata
- **Topics** — TF-IDF keyword extraction across user messages
- **Peak hour** — message timestamp bucketing
- **Longest session** — continuous activity blocks (defined as <30 min gap between messages)

No external service sees your data. The share card is rendered on-device. You choose whether to post it anywhere.

---

## When to Run It

- **End of each week** — Friday afternoon, before you close the laptop. Great way to notice patterns.
- **Monthly retros** — `wrapped month` for a bigger picture. Good input for 1:1s.
- **After a big sprint** — "Here's what that sprint actually looked like."
- **End of quarter** — run `wrapped month` three times in a row to get a quarter view (not automated yet, but the three reports side by side paint the picture).

---

## Example Output Highlights

From a real user's `/wrapped week`:

- **Total messages:** 412
- **Top command:** `/submit` (38 runs)
- **Spend:** $8.47 (Opus 62%, Sonnet 30%, Haiku 8%)
- **Peak hour:** Wednesday 2 PM — 31 messages, 4 jobs
- **Top topics:** "auth refactor", "JWT", "race conditions", "SwiftData migrations", "test coverage"
- **Longest session:** 3h 47m on Tuesday night, 82 messages

That last one is often the revealing number. "I worked 4 hours straight on Tuesday night" is either something you want to celebrate or something you want to avoid doing again.

---

## Tips

- **Run `wrapped week` every Friday.** Makes for a natural weekly ritual. Two minutes.
- **Share the last page.** The composite share card is designed to look good on social. Dev Twitter eats these up.
- **Compare week over week.** Screenshot page 2 (heatmap) each Friday and you will build a visual log of your working patterns over months.
- **Watch the spend page.** If one model is dominating unexpectedly, check your `/race` usage — that can spike spend fast.
- **Combine with `/recap`.** `/recap` is per-session. `/wrapped` is per-week. Different zoom levels, same instinct.

`/wrapped` is equal parts useful and fun. Most people run it once out of curiosity and then make it a weekly habit by week three.
