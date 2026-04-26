# Telegram Bot Notifications — Tutorial

> The simplest way to get instant iPhone push notifications when your Mac jobs complete. No Apple Developer account. No certificates. No cloud server to host. Five minutes from "I have an iPhone" to "my phone buzzes when my refactor finishes." This is the same pattern Hermes Agent uses — your Mac sends a Telegram message via your own bot, and Telegram already has push notifications wired up on iOS.

---

## Why Telegram?

CatClaw has three notification paths to choose from:

| Path | Latency | Setup | Reach |
|------|---------|-------|-------|
| Mac-only banners (`/notifier install`) | Instant | 1 command | Mac only |
| APNs push (`/notifier setup`) | Instant | Apple Developer account + .p8 key | iPhone, but single-developer |
| **Telegram (`/notifier telegram`)** | **Instant** | **5 min, no developer account** | **Anyone with Telegram on iPhone** |

Telegram is the universal middle ground. Telegram's iOS app already has rock-solid push notifications — they've solved that problem for you. You just create a bot via Telegram's official `@BotFather`, the Mac sends `curl` requests to Telegram's HTTPS API on each job completion, and Telegram routes the message to your phone.

The tradeoff is that the notification appears in the **Telegram app**, not in CatClaw — and tapping the banner opens Telegram. For most workflows that's fine. For sensitive job output (production secrets, customer PII) you should use the APNs path instead, since Telegram-routed text passes through Telegram's servers.

---

## What You Need

- iPhone with the **Telegram app** installed (App Store, free)
- A Telegram account (the same one you'll use on your phone)
- The CatClaw notifier daemon already installed on your Mac (`/notifier install` once)
- Active SSH connection to your Mac (the wizard writes config to your Mac via SSH)

That's it. No paid subscriptions, no Apple Developer Program membership, no certificates.

---

## Setup — 5 Minutes End to End

### Step 1: Create your bot in Telegram

Bots in Telegram are server-side entities Telegram hosts for free. Anyone can create one in 30 seconds.

1. Open the **Telegram app** on your iPhone
2. Search for **`@BotFather`** in the chat search (it's the official Telegram bot for managing bots — verified blue checkmark)
3. Tap **Start** if you've never messaged it before
4. Send `/newbot`
5. BotFather asks for a **display name** — anything you want, e.g. `My ClawTerminal`
6. BotFather asks for a **username** — must end in `bot`, e.g. `liqun_clawterm_bot`. Must be unique across Telegram.
7. BotFather replies with your **bot token** — a string that looks like `1234567890:AAEhBP1MAjFK...`. Treat it like a password.

Tap-and-hold the token to copy it.

### Step 2: Open the wizard in CatClaw

Back in CatClaw, in any chatroom, type:

```
/notifier telegram
```

The setup wizard opens as a sheet. Four steps with live status icons (⚪ pending, ⏳ in progress, ✅ done, ⚠️ failed).

### Step 3: Verify your bot token

Paste the token from BotFather into the field and tap **Verify**. The wizard hits Telegram's `getMe` API to confirm the token works and pulls back your bot's username (e.g. `@liqun_clawterm_bot`). You should see a green ✅ next to step 1.

If verification fails, double-check you copied the entire token (it includes a colon in the middle) and that you didn't accidentally include trailing whitespace.

### Step 4: Tell your bot your chat ID

Telegram has a clever rule: bots can't message you until you message them first. So before CatClaw can route notifications to you, you have to send your bot any message.

1. In the wizard, tap **Open @your-bot-username** — this deep-links into Telegram on your phone, opening a chat with your new bot
2. Send any message — `hi`, `start`, anything works
3. Switch back to CatClaw and tap **Refresh — Find My Chat ID**

The wizard polls Telegram's `getUpdates` API for up to 60 seconds, finds your most recent message, extracts the chat ID, and pre-fills it. Step 2 turns ✅.

If you're setting this up for a **group chat** (so multiple people get pinged), invite your bot to the group first, then send any message in the group. The chat ID will be a negative number, which is normal — Telegram uses negative IDs for groups.

### Step 5: Save and test

Tap **Save to Mac**. The wizard SSHes to your Mac and writes the bot token + chat ID to `~/.clawnotifier/telegram.env`. Step 3 turns ✅.

Tap **Send Test Message**. The wizard runs `~/.clawnotifier/test_telegram.sh` over SSH; the Mac fires off a `curl` to Telegram's `sendMessage` endpoint; within a couple seconds your iPhone's Telegram app shows a banner: "ClawTerminal — Test message — Telegram bot is wired up."

You're done.

---

## What Happens From Now On

The notifier daemon already running on your Mac (from `/notifier install`) gets a new fallback path. When any background job completes, it tries notification paths in this order:

1. **APNs** if you've configured `/notifier setup` — instant native iPhone push
2. **Telegram** if `~/.clawnotifier/telegram.env` exists — instant Telegram banner
3. **Local Mac banner** — your Mac's notification center via `terminal-notifier` or `osascript`

The first one that succeeds wins. So if you have both APNs and Telegram configured, APNs goes first; if APNs fails or isn't set up, Telegram takes over.

Try it: in any chatroom, type `/submit echo "telegram test"` and wait a few seconds. You should see a Telegram banner with the result.

---

## How It Actually Works

Under the hood the Mac runs a tiny script every 2 seconds:

```bash
curl -s -d "chat_id=$TELEGRAM_CHAT_ID" \
     --data-urlencode "text=*Job complete*\n<result>" \
     -d "parse_mode=Markdown" \
     "https://api.telegram.org/bot$TELEGRAM_BOT_TOKEN/sendMessage"
```

That's the entire mechanism. Telegram's HTTP API is unauthenticated past the bot token in the URL. No OAuth, no JWT signing, no certificate pinning. The result text is Markdown-formatted so you get a bold title and the body in plain text.

Telegram routes the message to your account, their iOS app gets it via their own APNs setup, and the banner appears on your phone. CatClaw never touches Apple's push infrastructure.

---

## Comparing Notification Paths Side By Side

You can have multiple paths configured at once — they cascade in priority order, so it's safe to leave fallbacks set up.

| Scenario | Best path |
|----------|-----------|
| Solo developer, want instant native banners | `/notifier setup` (APNs) |
| Sharing CatClaw with friends/teammates | `/notifier telegram` |
| Just need notifications when at your desk | `/notifier install` (Mac banners only) |
| Sensitive job content (secrets, PII) | APNs only — never Telegram |
| Group/family/team gets pinged | Telegram with the bot in a group chat |
| Want notifications on Mac, iPhone, and iPad together | Telegram (auto-syncs across devices via Telegram) |

---

## Troubleshooting

**"Refresh — Find My Chat ID" times out.** Make sure you sent a message to your bot AFTER tapping the wizard's "Open" button. Bots only see messages sent to them after they're created, and `getUpdates` only returns the last 24 hours of messages. If you waited too long, send another message and tap Refresh again.

**Test message succeeds but real jobs don't fire.** Re-run `/notifier install` to make sure the daemon picked up the new `push_telegram.sh` script and the cascading `fire()` function. Check the log: `cat ~/.clawnotifier/notifier.log` on your Mac should show `notifier started` and any push attempts.

**"Telegram replied: chat not found"** — your chat ID is wrong, or you blocked the bot. Open the bot in Telegram, tap the bot name at the top, make sure it says "stop bot" (not "restart"), and re-run the wizard.

**Bot token leaked.** Open `@BotFather` → `/revoke` → pick your bot → it issues a new token. Re-run `/notifier telegram` with the new token. The old token stops working immediately.

**Want to delete the bot entirely.** `@BotFather` → `/deletebot` → pick your bot → confirm. Then `rm ~/.clawnotifier/telegram.env` on your Mac to clean up.

---

## What This Costs

Nothing. Telegram bots are free. No rate limits matter for personal use (Telegram's bot API allows ~30 messages/second to a single chat — way more than any human would generate). No Apple Developer account fees. No cloud hosting.

The one ongoing cost is your trust in Telegram as the routing layer for job notifications. That's a real consideration — but for everyday dev work ("did the build pass," "did the deploy finish") it's the most pragmatic option available.
