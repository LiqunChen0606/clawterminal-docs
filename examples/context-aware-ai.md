# Context-Aware AI

> Four on-device sensors inject `<device_context>` into every prompt so Claude adapts to the time, your calendar, whether you are moving, and your battery level.

## Enable

**Settings → AI Intelligence → Context Awareness** → toggle master on. Then pick which sensors:

- **Time of Day** — no permission
- **Calendar** — iOS prompts for Calendar access on first enable
- **Motion** — no permission (iOS activity classification)
- **Battery** — no permission

All default to off.

## The chip bar

A thin chip row appears above the input bar in every chatroom showing active signals:

```
🌙 Night    🔋 18%    🚶 Walking    📅 Standup 25m
```

- **Tap a chip** — see the exact value being injected
- **Long-press a chip** — disable that sensor for this chatroom
- **Swipe the chip bar down** — hide for this session

## What Claude sees

A `<device_context>` block before your message:

```
<device_context>
  time: Tuesday 11:47 PM (night)
  battery: 14% (not charging)
  activity: stationary
  next_calendar_event: none within 8 hours
</device_context>
```

## Before / after examples

### Same question, 9 AM, full battery

> **You:** How should I design the payment service?
> **Claude:** Let me walk you through a clean architecture with three layers...
> *(full 600-word response, trade-offs, alternatives, diagrams)*

### Same question, 11 PM, 14% battery, low-effort pattern

> **You:** How should I design the payment service?
> **Claude:** Given the hour and your battery, here's a shipping-ready MVP — we can revisit the full architecture tomorrow...
> *(short response, one recommendation, save the deep dive)*

### Same question, 8 AM with a meeting in 10 minutes

> **You:** What did I ship yesterday?
> **Claude:** Three bullets for your standup — 1) webhook idempotency fix, 2) PR #418 review, 3) /preview multi-port bug resolved.

## When to use it

- You work from your phone in varied contexts (walking, on trains, on calls)
- You often have meetings in the middle of coding sessions
- You value responses that match your available time and energy
- You want AI behavior that feels human — a coworker who notices it is 1 AM would shorten their answer too

## When to disable it

- You want deterministic, time-independent responses for consistency
- You do not want calendar metadata in your Claude prompts
- You are in a research/benchmark setting where context should not vary

## Tips

- Keep calendar on — the "next event in N minutes" signal is the single biggest quality win.
- Combine with `/effort low` for a nuclear-short response when battery is low.
- Disable motion in cars where you are a passenger and want full responses.
