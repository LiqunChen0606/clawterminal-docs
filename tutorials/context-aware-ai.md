# Context-Aware AI — Tutorial

> CatClaw reads the room. Time of day, your calendar, whether you are walking or sitting, your battery level — these all get quietly folded into the prompt so Claude can give advice that fits the moment, not just the words you typed.

---

## What It Does

Four on-device sensors feed a `<device_context>` block into every message preamble:

| Sensor | What it provides | Permission |
|--------|------------------|------------|
| **Time of day** | Morning / afternoon / evening / night — Claude knows if you are starting fresh or burning midnight oil | None |
| **Calendar** | Your next event and how many minutes until it starts (e.g. "Standup in 25m") | Calendar access prompt |
| **Motion / activity** | Whether you are stationary, walking, driving, or on a train | None |
| **Battery level** | Percentage + charging state — below 20% Claude will skip long-winded answers | None |

The raw signals appear as a chip bar at the top of the chatroom:

```
🌙 Night   🔋 18%   🚶 Walking   📅 Standup 25m
```

Tap any chip for the detail.

---

## Why It Matters

Same prompt, different context = different answer.

**9 AM, battery 100%, calendar clear:**
> "How should I design the payment service?"
> Claude gives a thorough, multi-option architectural breakdown.

**11:47 PM, battery 14%, no calendar:**
> "How should I design the payment service?"
> Claude leads with a quick, shippable recommendation and says "Let's revisit the trade-offs tomorrow."

**8:15 AM, standup in 10 minutes:**
> "What did I ship yesterday?"
> Claude gives you a 3-bullet summary from git + chatroom history, formatted for standup.

You did not have to explain any of that. CatClaw knew.

---

## Setup

1. Open **Settings → AI Intelligence → Context Awareness**
2. Toggle on the master switch
3. Toggle individual sensors:
   - **Time of Day** — always on, no permission needed
   - **Calendar** — tap on, iOS will prompt for Calendar access once
   - **Motion** — always on, no prompt (uses `CMMotionActivityManager` which does not require a permission dialog on iOS 17+)
   - **Battery** — always on, no prompt

All sensors default to **off** the first time you launch the update. You opt in explicitly.

---

## The Chip Bar

Above the input bar in every chatroom you will now see a thin chip row showing which signals are active:

- **Tap a chip** → opens a small popover showing the exact value that is being injected into the preamble
- **Long-press a chip** → toggle that specific sensor off for this chatroom (does not affect other rooms)
- **Swipe down on the chip bar** → hide it for this session (pull up again to restore)

The chip bar only shows active signals. If you disable the battery sensor, the battery chip disappears entirely.

---

## What Claude Actually Sees

Here is the raw block injected before your message when all four sensors are active:

```
<device_context>
  time: Tuesday 11:47 PM (night)
  battery: 14% (not charging)
  activity: stationary
  next_calendar_event: none within 8 hours
</device_context>
```

Short, structured, cheap. Adds maybe 30 tokens per message — trivial cost for the adaptive behavior it unlocks.

---

## Privacy

- **Everything runs on device.** Calendar events are read via EventKit and formatted locally into a one-line summary. Full event details never leave your phone.
- **Motion data is classification only.** CatClaw does not have access to GPS coordinates or routes. It knows "walking" vs "stationary", nothing more.
- **Battery readings** use `UIDevice.batteryLevel` — the same API every app has.
- **You can see every byte** that gets injected by tapping the chip. There is no hidden signal.

Disable any sensor (or all of them) and the block disappears from the preamble entirely.

---

## Tips

- **Keep calendar on.** The "next event in N minutes" signal is the biggest quality win — Claude naturally shortens responses when you have a meeting coming up.
- **Combine with `/effort`.** Battery below 20% and `/effort low` stack: Claude gives you the absolute minimum required to keep moving.
- **Pair with Frugal Mode.** If you are on low battery and in a bad signal area, `/frugal` plus context-aware AI means Claude skips preamble, skips memory lookups, and gives you the fastest possible answer.
- **Turn motion off in the car.** If you are a passenger and do not want "driving" triggering terse answers, long-press the motion chip to pause it for the ride.

Context-aware AI is small in scope, big in feel. Once you have used it for a week, every other AI assistant starts to feel slightly blind.
