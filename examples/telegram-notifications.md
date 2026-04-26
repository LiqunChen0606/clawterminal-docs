# Telegram Bot Notifications — Examples

Real-world scenarios for the `/notifier telegram` path. All examples assume you've completed the one-time wizard setup.

---

## Scenario 1: "Did my refactor finish?"

You kicked off a 12-minute multi-agent refactor before catching the train. You're on the platform now and your phone buzzes — Telegram banner:

> **ClawTerminal — Job complete**
> Refactored OAuth flow into AuthService.swift. 4 files changed, 18 tests pass. Branch: refactor/auth-service.

You skim the body, tap to open the result in Telegram for the full output, and decide whether to open CatClaw to inspect the diff or just merge from your phone.

This is the bread-and-butter use case. Long-running background jobs, instant notification when they're done, no need to keep CatClaw open or to babysit the Mac.

---

## Scenario 2: Family or team gets pinged when production deploys finish

You're on a small team and want everyone to know when a deploy lands. Setup:

1. In Telegram, create a group called "ClawTerminal Deploys" and invite yourself + teammates
2. Add your bot to the group as an admin
3. In the group chat, send any message (so the bot sees a chat to reply to)
4. In CatClaw, run `/notifier telegram` again — same wizard, but tap "Refresh" after sending the message in the **group**
5. The chat ID will be a negative number (Telegram convention for groups)
6. Save

From now on, every `/submit` job that includes a deploy step pings the entire group. No Slack workspace required, no webhook setup, no admin permissions.

---

## Scenario 3: Test passes / fails, color-coded

The notifier daemon detects errors heuristically — if the last 500 chars of job output contain `error`, `fail`, or `failed`, the title becomes "Job **failed**" instead of "Job complete." Telegram doesn't color-code, but you'll see the difference in the title text:

> **ClawTerminal — Job complete**
> 47 tests pass.

vs

> **ClawTerminal — Job failed**
> AssertionError in test_auth_flow.py line 142: expected 200, got 401.

Pair this with `/submit` for your CI command (`/submit make test`) and you'll know within seconds whether the test suite is green without leaving wherever you are.

---

## Scenario 4: Mute notifications during focus time

Telegram's per-chat mute is your friend here. In Telegram, open the chat with your bot → tap the bot name at the top → **Mute** → pick a duration (1 hour, 8 hours, until I turn it back on).

While muted, the messages still arrive (your iPhone records them and the bot history is intact), but no banner / no sound. Perfect for a deep-work session where you want jobs to keep running without interrupting you. Unmute and tap into the bot to scroll through what completed.

---

## Scenario 5: Switch from APNs back to Telegram

You set up the APNs path (`/notifier setup`) but moved to a different iPhone for testing and don't want to re-paste your device token. Just delete the APNs config:

```bash
ssh you@your-mac "rm ~/.clawnotifier/apns/auth.p8 ~/.clawnotifier/device_token"
```

The notifier's cascading `fire()` falls through to the Telegram path automatically — no other change needed. You'll start getting Telegram banners again on the next job completion.

(Or run `/notifier setup` and uncheck things in the wizard once that's wired.)

---

## Scenario 6: Want both APNs and Telegram, just in case

Configure both. The Mac's notifier daemon tries APNs first; if APNs fails (network blip, bad token, expired profile), it automatically falls back to Telegram. You'll never miss a notification because of one path's failure.

The cascade is:

1. APNs (Sprint 64 — fast, native, but single-developer)
2. Telegram (Sprint 66 — fast, universal)
3. Local Mac banner (always works as last resort)

First success short-circuits the rest, so you never get duplicate notifications.

---

## Scenario 7: Custom alert message style

The Mac script formats messages as Markdown. Edit `~/.clawnotifier/push_telegram.sh` if you want different formatting:

```bash
TEXT=$(printf "🟢 *%s*\n\`%s\`" "$TITLE" "$BODY")    # Code-formatted body
TEXT=$(printf "🤖 %s\n\n_%s_" "$TITLE" "$BODY")       # Italic body, robot emoji
```

Telegram's Markdown supports `*bold*`, `_italic_`, `` `code` ``, ```` ```pre``` ```` blocks, and inline links `[text](url)`. Restart the notifier (`launchctl bootout` + `launchctl bootstrap`) after editing.

---

## Scenario 8: Debugging — why didn't I get a notification?

Run on your Mac:

```bash
tail ~/.clawnotifier/notifier.log
```

You'll see one of:

- `notified <jobID> (ClawTerminal — Job complete)` — daemon detected the sentinel, went through `fire()`. If you didn't see a Telegram banner, the curl call returned non-200; scroll up for the response.
- Nothing recent — the daemon isn't seeing the job's output file. Confirm it's running with `launchctl list | grep clawterminal`.
- `APNs push failed; trying next` then `Telegram push failed; trying next` then `local banner` — both push paths failed, fell through to the Mac banner.

Also test the Telegram path independently:

```bash
~/.clawnotifier/test_telegram.sh
```

Should return `200` if everything works.
