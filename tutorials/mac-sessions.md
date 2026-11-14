# Mac Sessions — Tutorial

> You started a Claude Code session in Terminal at your desk, then had to leave. With Mac Sessions you open ClawTerminal on your iPhone, type `/sessions`, and see every Claude Code session on your Mac: which ones are working, which are waiting for you, and which are idle. Tap **Continue on iPhone** and keep going with the full history. The session on your Mac stays exactly as you left it.

---

## Part 1: What Mac Sessions Shows

Mac Sessions lists **every Claude Code session on your Mac**, including the ones you started yourself in Terminal, not only the ones ClawTerminal started. Sessions are grouped by status:

| Group | Meaning |
|-------|---------|
| **Working** | Claude is busy on a task right now |
| **Needs Input** | The session is waiting for you, for example for a permission or an answer |
| **Idle** | Open, but nothing is running |
| **Finished** | A background session that has ended |

The list refreshes every 10 seconds while it's open, and only then. Sessions that your iPhone is already driving are hidden, so you only see work that happened somewhere else.

---

## Part 2: Opening It

Three ways in, all opening the same sheet:

- Type **`/sessions`** in any chatroom
- Tap the **…** menu in the chatroom tab bar (or on the My Mac screen), then **Mac Sessions**
- Open the drawer and tap **Mac Sessions**

You need to be connected to your Mac first. If you aren't, the sheet says so and offers a **Reconnect** button.

---

## Part 3: Row Actions

Every session row has three actions.

### Continue on iPhone

Opens a chatroom on your iPhone with **your own copy** of the session, including its full conversation history. Claude Code's fork feature makes the copy:

- Your messages from the iPhone go to the copy, not to the original
- **The session on your Mac is never changed.** If it's still running at your desk, it keeps running, undisturbed
- The new chatroom shows a short banner noting where it came from

ClawTerminal always makes a copy, even when the session looks idle. The list can be up to 10 seconds old, and a copy is the only way to be sure nothing at your desk gets overwritten.

### Open in Terminal Tab

Opens the session in a Terminal tab on your iPhone, for when you want the full interactive Claude Code interface:

- An interactive session opens as a copy, the same as Continue on iPhone
- A background session is attached directly
- If your Mac didn't report a full session ID, attaching from a Terminal tab is the only option, and the row says so

### Copy Session ID

Copies the session ID to your clipboard, ready for `claude --resume <id>` anywhere else.

---

## Part 4: A Typical Day

1. **Morning, at your desk.** You start `claude` in Terminal and kick off a long refactor.
2. **Lunch.** On your iPhone, type `/sessions`. The refactor shows under **Needs Input**: Claude is asking which test framework to use.
3. Tap **Continue on iPhone**, answer in the chatroom, and let your copy carry on.
4. **Back at your desk.** The original session is still there, exactly as you left it.

---

## Requirements

- An SSH connection to your Mac (on your home network, or from anywhere with Tailscale; see [Connect From Anywhere](connect-anywhere.md))
- A recent Claude Code on your Mac. If the sheet says your version is too old, run `claude update` on the Mac
- Mac Sessions chatrooms run Claude (not Codex, Gemini, or Aider)

---

## Tips

- **Needs Input is the one to watch.** It shows which sessions are stuck waiting for you.
- **Copies are cheap.** Continuing a session doesn't touch the original, so feel free to explore from your phone.
- **Hand work back with `/handoff mac`.** It opens a chatroom's session in a tmux window on your Mac. See [Session Handoff](handoff.md).
