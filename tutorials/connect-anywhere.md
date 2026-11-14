# Connect From Anywhere — Tutorial

> At home your iPhone reaches your Mac over Wi-Fi. On the train it needs Tailscale. You shouldn't have to think about which one, and now you don't: ClawTerminal tries both every time it connects and uses whichever answers. It reconnects on its own when your network changes, and it keeps the messages you type while the connection is down.

---

## Part 1: Home Network and Tailscale, Tried Together

Each time ClawTerminal connects (opening a chatroom, reconnecting, coming back to the app, opening a terminal tab), it checks every address it knows for your Mac **at the same time**:

- your Mac's **home network** address (the hostname you set up first)
- its **Tailscale name** (MagicDNS, like `my-macbook.tail1234.ts.net`)
- its **Tailscale IP** (a `100.x.x.x` address)

The first one that answers wins. Your home network gets a short head start, so you stay on the faster local path when you're home. The winner is remembered for the network you're on, which makes the next connect quicker. If the first choice fails, the next-best address is tried automatically.

What changes for you: away from home, the app no longer sits for half a minute and then gives up without trying Tailscale. It just connects.

---

## Part 2: Setting Up Tailscale (Mostly Automatic)

[Tailscale](https://tailscale.com) is a free private network that lets your iPhone reach your Mac from anywhere, without opening ports on your router.

1. Install Tailscale on your **Mac** and your **iPhone**, and sign in to the same account on both
2. Connect to your Mac once from home in ClawTerminal
3. ClawTerminal checks whether Tailscale is running on the Mac. If it is, the **My Mac** tab shows a chip: **"Tailscale detected: <your Mac>"**
4. Tap **Enable away-from-home access**. The Tailscale name and IP are filled into your connection profile

That's it. You can also enter them by hand: edit the connection and fill in the **Remote Access** section (**Remote hostname (Tailscale / VPN)** and **Tailscale IP (100.x, optional)**).

**Key-expiry warning.** Tailscale device keys expire periodically by default. When your Mac's key is about to expire, ClawTerminal warns you and explains how to turn off key expiry in the Tailscale admin console, so your away-from-home access doesn't stop without notice.

---

## Part 3: "Tailscale Is Off on This iPhone"

The most common reason a connection fails away from home is that Tailscale got switched off on the phone. ClawTerminal recognizes this and shows a card instead of a generic error:

> **Can't reach your Mac — Tailscale on this iPhone looks off**

Tap **Open Tailscale**, switch it on, then tap **Try again**. If you're actually at home, the card reminds you to check that your Mac is awake and Remote Login is on.

---

## Part 4: Reconnecting on Its Own

- **Wi-Fi ↔ cellular.** When your network changes, for example walking out of the house, ClawTerminal checks the connection and reconnects if needed, choosing the best route again.
- **Status pill.** While it works, a small pill shows what's happening: *Waiting for network…*, *Reaching Mac via LAN…*, *Reconnecting via Tailscale…*, then *Connected*. If the Mac can't be reached, the pill says so and offers **Retry**.
- **Faster recovery after sleep.** When your phone wakes, a dead connection is spotted within seconds instead of hanging.
- **Warmed up before you need it.** Tapping into the message box, or tapping a notification, starts reconnecting right away, so the link is usually ready by the time you hit Send.

---

## Part 5: Never Lose a Message

- **Typing during a reply.** Send a message while Claude is still answering and it waits in a queue (*"Queued — sends after this turn"*), then goes out automatically when the reply finishes. Before, it could be dropped.
- **Offline outbox.** In a Mac chatroom with no connection, your message waits (*"Will send when connected"*) and goes out once the connection is healthy again.
- **Old messages ask first.** If a queued message has waited more than 10 minutes, the pill asks *"send now?"* instead of sending it without asking.
- **Replies survive drops.** If the connection drops mid-reply, the reply is paused, not thrown away. Claude keeps working on your Mac, and the app picks the reply up when it reconnects.

The queue lives in memory while ClawTerminal is running. If you force-quit the app, anything still queued is cleared.

---

## Part 6: Host-Key Pinning

The first time you connect to a Mac, ClawTerminal remembers that Mac's SSH host key. If the key ever changes, you get a **Host Key Changed** alert showing the old and new fingerprints:

- **Trust New Key** if you know why it changed (you reinstalled macOS or regenerated SSH keys)
- **Cancel** if you don't. A key that changes for no reason can mean someone is intercepting the connection. The app won't keep retrying with a key you declined

Pinning is **on by default**: **Settings → Connections → Pin Host Keys**. To see or reset a Mac's pinned key, edit its connection and use **Forget Host Key**. The next connect pins the key again.

---

## Part 7: Faster Replies With Lean Launch (Optional)

Every reply starts a short-lived shell on your Mac. Normally that shell runs all your shell startup files. **Lean Launch** loads a saved copy of your Mac's login environment instead (PATH, API keys, Claude and Homebrew settings), so each reply can start about a second sooner.

- **Off by default.** Turn it on at **Settings → AI Intelligence → Lean Launch**
- The saved copy is refreshed on every connect and once a day
- If a tool the agent runs can't find something your shell profile normally sets up, turn it off

---

## Troubleshooting

| Symptom | Try |
|---------|-----|
| Works at home, fails away | Make sure Tailscale is on on your iPhone and running on your Mac. Check that the Remote Access fields are filled in |
| "Tailscale detected" chip never appears | Tailscale must be installed and running on the Mac. You can also fill in the Remote Access fields by hand |
| Host Key Changed alert you didn't expect | Tap Cancel, then check the Mac. Only trust the new key once you know why it changed |
| Everything is slow to start | Try Lean Launch. Run `/doctor` to see the round-trip time |
| Connection drops when the Mac sleeps | See "Keep Your Mac Awake" in the welcome tour (Settings → About → Welcome Tour) |
