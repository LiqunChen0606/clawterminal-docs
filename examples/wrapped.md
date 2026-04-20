# Dev Wrapped

> `/wrapped` — Spotify Wrapped for your code week. Nine swipeable pages of stats, topics, and a final share card.

## Run it

### Last 7 days (default)

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

Takes ~2–3 seconds to generate. Opens full-screen with swipeable pages.

---

## The 9 pages

| # | Page | Shows |
|---|------|-------|
| 1 | **Opener** | Date range + headline stat ("127 messages, 8 jobs, 1 sprint") |
| 2 | **Activity Heatmap** | Week × hour grid, darker = more active |
| 3 | **Top Commands** | Top 5 slash commands with counts |
| 4 | **Spend** | Model-by-model token cost bar chart |
| 5 | **Peak Hour** | Your single most-active hour |
| 6 | **Top Topics** | 5 subjects you discussed most (TF-IDF extracted) |
| 7 | **Agent Personality** | Most-used `/personality` or implied style |
| 8 | **Longest Session** | Single continuous chatroom session with stats |
| 9 | **Share Card** | Composite postcard with your top 4 stats |

---

## Example output (from a real week)

### Page 1
> You shipped **412 messages** and **31 jobs** this week.
> Biggest day: **Wednesday, 87 messages**.

### Page 3 — Top commands
```
1.  /submit         38
2.  /race           19
3.  /health         14
4.  /pr             11
5.  /recap           9
```

### Page 4 — Spend
```
Opus      $5.22  ████████████████ 62%
Sonnet    $2.52  ████████ 30%
Haiku     $0.73  ██ 8%
─────────────────
Total     $8.47
```

### Page 5 — Peak hour
> Your peak was **Wednesday 2 PM** — 31 messages, 4 jobs launched.

### Page 6 — Top topics
```
1. auth refactor
2. JWT refresh
3. race conditions
4. SwiftData migrations
5. test coverage
```

### Page 8 — Longest session
> **Tuesday 8 PM → 11:47 PM (3h 47m)**
> 82 messages, 2 jobs completed.
> Closing message: "Great — all tests green. Shipping in the morning."

### Page 9 — Share card
Tap the share button. A 1080×1080 postcard renders with the top 4 stats on an indigo→purple gradient. Share to Twitter, Discord, etc.

---

## Privacy

All analysis is **local** — no API calls.

- Timestamps from `chatrooms.json`
- Commands grepped from user message text
- Spend summed from per-job cost metadata
- Topics via local TF-IDF (no external service)
- Peak hour via timestamp bucketing
- Longest session via continuous-activity blocks (<30 min gap)

The share card is rendered on-device. You choose if it goes anywhere.

---

## When to run it

- **End of each week** — Friday afternoon, before closing the laptop
- **Monthly retros** — `/wrapped month` for 1:1 input
- **End of a sprint** — capture what the sprint actually looked like
- **End of quarter** — three monthly wraps side by side tell the story

---

## Tips

- **Screenshot page 2 weekly.** Build a visual log of your work patterns over months.
- **Share page 9.** The composite card looks great on dev Twitter.
- **Compare weeks.** Two back-to-back `/wrapped week` runs reveal shift in focus.
- **Watch page 4.** If one model is dominating spend unexpectedly, check your `/race` usage.
- **Pair with `/recap`.** `/recap` is per-session. `/wrapped` is per-week. Different zoom, same instinct.
