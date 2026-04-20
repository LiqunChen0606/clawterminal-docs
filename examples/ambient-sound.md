# Ambient Sound Design

> Four subtle iOS system sounds for key events. Opt-in, per-event, off by default.

## Enable

**Settings → Ambient Sound**

Toggle the master switch and pick individual events:

- **AI thinking** — soft upward chime when Claude starts processing
- **Job complete** — clean two-tone confirm when a job finishes
- **Job failed** — descending tri-tone when a job errors out
- **Streaming start** — subtle tick as the first token arrives

All default to off.

---

## Popular combinations

### Heads-down developer

```
✅ Job complete
✅ Job failed
❌ AI thinking
❌ Streaming start
```

Only long-running events interrupt. Short latency is silent.

### Active chat

```
✅ AI thinking
✅ Streaming start
❌ Job complete
❌ Job failed
```

Audio feedback on every turn. Good for pair-programming.

### Background jobs only

```
❌ AI thinking
❌ Streaming start
✅ Job complete
✅ Job failed
```

Silent while typing, audible only for async work. Best for `/submit` → walk away → hear the chime.

---

## iOS system sound IDs

| Event | ID | Name |
|-------|----|----|
| AI thinking | 1113 | Tink |
| Job complete | 1025 | Telegraph |
| Job failed | 1073 | ReceivedMessage (descending) |
| Streaming start | 1104 | Tock |

Using system sounds means:

- Silent mode (ring switch off) silences all ambient sounds
- Focus modes (Do Not Disturb) suppress them per iOS rules
- No custom audio files loaded — zero storage, zero battery
- Consistent across iPhone and iPad

---

## Real-world scenarios

### Long `/submit` job, phone face-down

You submit a 40-minute refactor via `/submit`. Phone goes face-down. You keep working. Twenty minutes in, the Telegraph chime plays. You pick up the phone — job done. You never had to glance over once.

### Failed build during pair programming

You fire off `/submit` during a pair session. Phone is on the desk. Thirty seconds later the descending tri-tone plays — failure. You both hear it, switch focus, tap the notification, see the error, diagnose together.

### Ignoring low-latency events

You have AI thinking + streaming start disabled because short interactions are too frequent. Only when a background job completes do you hear the chime. Perfect signal-to-noise.

---

## Tips

- **Start with just Job Complete and Job Failed.** That is 90% of the real value.
- **Different sounds for success vs. failure matters.** Your brain learns the descending tri-tone as "oh no" within three occurrences.
- **Combine with haptics.** Sound + haptic gives two channels — useful when phone is in pocket (haptic) or on desk (sound).
- **Silent mode is respected.** No need to toggle manually during meetings.
- **watchOS has its own haptic/sound behavior.** These settings are iOS-only.
