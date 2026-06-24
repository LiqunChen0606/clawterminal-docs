# Smart Notifications

> ClawTerminal sends intelligent notifications beyond just background job completions --- it detects when Claude finishes a heavy task or completes all your to-dos, even when you are on another screen.

---

## Notification Types

ClawTerminal fires local notifications in the following situations:

### 1. Background Job Complete

When a `/submit` background job finishes, you receive a push notification with the job status. This notification includes quick actions:

| Action | Description |
|--------|-------------|
| **View Result** | Opens the app and navigates to the Jobs panel |
| **Copy Result** | Copies the job result text to the clipboard without opening the app |

### 2. Long Response Complete (30+ Seconds)

When Claude's streaming response takes longer than **30 seconds** to complete (indicating a complex or computationally heavy task), a notification fires to let you know the response is ready. This is useful when you switch to another app while waiting for Claude to finish a detailed analysis or code generation.

### 3. All To-Dos Complete

When Claude uses the `todo_write` tool and marks **all tasks as completed**, a notification fires to let you know all work items are done. This is especially useful during multi-step tasks where Claude is working through a checklist.

---

## When Notifications Fire

| Situation | Notification fires? |
|-----------|-------------------|
| App is in the foreground, viewing the chatroom | Held back while "Quiet While App Is Open" is on (default) |
| App is in the foreground, different tab | Yes (banner + sound) |
| App is in the background | Yes (banner + sound) |
| Screen is locked | Yes (lock screen notification) |

---

## Quiet While App Is Open

There's no point buzzing you about a job you're already watching. While ClawTerminal is open and in the foreground, completion notifications for that session are held back; they resume the moment you leave the app or lock your screen. This applies both to on-device notifications and to Mac-side push (the notifier daemon honors the same signal).

- **On by default.** Toggle at **Settings → AI Intelligence → "Quiet While App Is Open."** Turn it off to get banners even while you're looking at the app.
- **If you use the Mac notifier daemon** (`/notifier install`), re-run `/notifier install` once so the daemon learns to stay quiet while you're present.
- **Self-healing:** if the app is force-quit while open, suppression lifts on its own within a few minutes, so you never miss a notification permanently.

---

## ntfy — Push With No Account

ntfy is the lowest-friction way to get job-completion notifications on your phone — **no login, no registration, no API key.**

1. Run `/notifier ntfy` in a chatroom.
2. Pick a **topic** (any hard-to-guess name, e.g. `claw-7f3a9c2e`) — leave the server as `https://ntfy.sh` or point it at your own self-hosted ntfy.
3. Tap **Test**, then **Save**.
4. Install the free **ntfy** app from the App Store and **subscribe to the same topic**. That's it — completions now arrive as banners.

Notes:
- The topic name is the only secret (it's a public namespace), so choose something unguessable, or self-host ntfy for a private server.
- It slots into the notifier cascade after Apple Push and Telegram, so if you have one of those configured, it stays primary and ntfy is the fallback.
- ntfy is also a channel in the `/hermes` / `/openclaw` chat-push wizard.
- If you set up the Mac notifier daemon earlier, re-run `/notifier install` once so it learns the ntfy sender.

---

## Notification Permissions

ClawTerminal requests notification permission on every launch. If you have not yet granted permission, iOS shows the standard authorization dialog.

If you previously denied notifications:

1. Open **Settings (iOS) --> ClawTerminal --> Notifications**
2. Toggle **Allow Notifications** on
3. Customize alert style, sounds, and badges as desired

---

## Tips

- **Keep notifications on:** Notifications are the primary way to know when background jobs finish. Without them, you have to manually check the Jobs panel
- **Quick actions:** When a job completion notification arrives, swipe down on it to reveal the **View Result** and **Copy Result** action buttons --- no need to open the app and navigate to the Jobs panel
- **30-second threshold:** The long-response notification only fires for responses that take 30 seconds or more of streaming time. Quick responses (under 30 seconds) do not trigger a notification, keeping your notification center clean
