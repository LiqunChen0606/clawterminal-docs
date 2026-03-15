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
| App is in the foreground, same tab | Yes (banner + sound) |
| App is in the foreground, different tab | Yes (banner + sound) |
| App is in the background | Yes (banner + sound) |
| Screen is locked | Yes (lock screen notification) |

ClawTerminal shows notifications even when the app is in the foreground, so you see them regardless of which tab you are on. The `UNUserNotificationCenter` delegate is configured to present banners and play sounds in all states.

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
