# Ambient Sound Design — Tutorial

> Four subtle iOS system sounds that tell you what is happening without you having to look at your phone. Opt-in, per-event, off by default.

---

## What It Does

Your eyes are already on your editor. Your phone is face-down on the desk. When a background job finishes, you want to *know* — but not by picking up the phone to check.

Ambient sound design adds four tiny, specifically-chosen iOS system sounds to key moments in CatClaw. Each is two or three tones, short enough to not be annoying, distinct enough to recognize without looking.

| Event | Sound | Feels like |
|-------|-------|------------|
| **AI thinking** | Soft upward chime | "Working on it" |
| **Job complete** | Clean two-tone confirm | "Done — come check" |
| **Job failed** | Descending tri-tone | "Something went wrong" |
| **Streaming start** | Subtle tick | "Response coming in" |

All four default to **off**. You opt in only to the ones you want.

---

## Setup

Open **Settings → Ambient Sound**. You will see:

- **Master switch** — kills all ambient sounds with one tap
- **AI thinking** — plays when a `/submit` job starts or Claude begins generating
- **Job complete** — plays when a background job finishes successfully
- **Job failed** — plays when a job fails (separate from "complete" so you can enable only failures if you want)
- **Streaming start** — plays the moment the first token arrives back from Claude

Turn on whichever combination matches how you work.

---

## Good Combinations

### "Heads Down" Developer

```
✅ Job complete
✅ Job failed
❌ AI thinking
❌ Streaming start
```

Only long-running events interrupt you. Short latency is ignored. This is the most popular combination.

### "Active Chat"

```
✅ AI thinking
✅ Streaming start
❌ Job complete
❌ Job failed
```

You want audio feedback on every turn of conversation — like a tiny "roger that." Good for pair-programming sessions where you want to confirm Claude received your message without glancing over.

### "Background Jobs Only"

```
❌ AI thinking
❌ Streaming start
✅ Job complete
✅ Job failed
```

Silent while you type, audible only for async work. Best for the `/submit` → walk away → hear the chime workflow.

### "Full Soundscape"

```
✅ All four
```

Every state transition makes a sound. Fun for a few hours. Usually turned off within a day. A good way to learn the sound palette.

---

## Why These Sounds?

They are drawn from the macOS / iOS system sound palette. The exact IDs:

| Event | System sound |
|-------|--------------|
| AI thinking | `1113` (Tink) |
| Job complete | `1025` (Telegraph) |
| Job failed | `1073` (ReceivedMessage, descending variant) |
| Streaming start | `1104` (Tock) |

Using system sounds means:

- **Respects silent mode.** Ring switch off = no sounds. No exceptions.
- **Respects Focus modes.** In Do Not Disturb / Focus, sounds are suppressed per iOS rules.
- **No custom audio file to load.** Zero battery impact, zero storage.
- **Consistent across devices.** An iPhone and an iPad running the same ambient config will sound identical.

---

## Tips

- **Start with just Job Complete and Job Failed.** That is 90% of the real value. Add the others only if you find yourself wanting them.
- **Different sounds for failure vs. success matters.** Your brain can learn the descending tri-tone as "oh no" after about three occurrences. Use it.
- **Combine with haptics.** Ambient sound + haptic feedback gives you two channels of information — especially useful when your phone is in a pocket (haptic only) or on a desk (sound).
- **Silent mode overrides everything.** If you are in a meeting and your phone is on silent, ambient sounds are silent too. No need to toggle them off manually.
- **Works on watchOS separately.** Your watch has its own haptic + sound behavior for job notifications. These settings only control the iOS app.

Ambient sound design is one of those features that is hard to describe and obvious the moment you try it. Turn on Job Complete for a week and see how much less you check your phone.
