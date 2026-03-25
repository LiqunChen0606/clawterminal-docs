# Collaboration Examples

Share your AI chatroom session in real-time. Guests join with a 6-character room code and watch your conversation as it streams — useful for pair programming, code review sessions, or demos.

---

## Requirements

- A relay server running on your network (see Setup below)
- Both host and guest connected to the same relay server
- Host must have an active ClawTerminal chatroom session

---

## 1. Host a shared room

**Step 1 — Open the Share sheet:**
In an active chatroom, tap the **Share button** (person.2.wave.2 icon) in the toolbar.

**Step 2 — Set your display name and share:**
On the **Host** tab:
- Enter your display name (optional — defaults to "Host")
- Tap **Share**

**Step 3 — Share the room code:**
A **6-character room code** appears in large monospace text, for example: `XK7M2P`

Share this code with anyone who wants to watch. You can tap **Copy Code** to copy it to clipboard, then paste it into Slack, Messages, or email.

**While hosting:**
- Your chatroom works exactly as before — send messages, run commands, use slash commands
- Guests see your messages and Claude's responses stream in real-time as you work
- The Share toolbar button shows a badge indicating active hosting
- Tap **Stop Sharing** to close the room and disconnect all guests

---

## 2. Join a room as a guest

**Step 1 — Open the Share sheet:**
In any ClawTerminal chatroom, tap the **Share button** in the toolbar.

**Step 2 — Switch to the Join tab and enter the code:**
On the **Join** tab:
- Enter the 6-character room code (case-insensitive, auto-uppercased)
- Tap **Join**

**While watching:**
- Messages appear as read-only bubbles — you see everything the host sees
- The view auto-scrolls as new content arrives
- You cannot send messages (read-only view)
- Tap **Leave** to disconnect from the room

**If the host stops sharing:**
You're notified and the guest view closes automatically.

---

## 3. Real-world use cases

### Pair programming session

**Alice** (host) is debugging an authentication issue. She shares a room code with **Bob** (guest) via Slack:

> "Here's my ClawTerminal session code: `TY4KPZ` — watch me debug this JWT issue"

Bob joins on his iPhone. He watches Alice send commands to Claude, sees Claude's responses streaming in real-time, and can follow along without screen sharing software.

### Code review demo

A team lead shares their room code in a team channel. Junior developers join to watch Claude review a PR in real-time — they see both the prompts and Claude's line-by-line review as it streams.

### Remote teaching

An instructor running a workshop shares a room code. Students on their phones join as guests and follow along with Claude explaining code concepts in a live chatroom session.

---

## 4. Set up the relay server

The shared chatroom requires a relay server running on your network. The relay server is included in the ClawTerminal repository at `relay-server/`.

**Step 1 — Install and start the relay server on your Mac:**

```bash
cd ~/ClawTerminal/relay-server
npm install
npm start
```

The server starts on port 8765 by default.

**Step 2 — Note your Mac's address:**

For local network: `ws://macbook.local:8765`
For Tailscale: `ws://macbook.tail12345.ts.net:8765`

**Step 3 — Configure ClawTerminal:**

1. Open Settings > Relay Server
2. Enter the relay server URL: `ws://macbook.local:8765`
3. Enter the authentication token (set in the relay server's config)
4. Tap **Save**
5. Tap **Test Connection**

A teal **"Relay"** chip appears in the connection health bar when connected.

**Step 4 — Both host and guest must connect to the same relay server.**
Share the relay URL and auth token with anyone who wants to join your rooms.

---

## 5. Troubleshooting

### "Relay server not connected" message

1. Confirm the relay server is running on your Mac: `ps aux | grep relay`
2. Check the URL in Settings > Relay Server — should start with `ws://` not `http://`
3. Verify the auth token matches what the server is using
4. Confirm both devices are on the same network (or connected via Tailscale)

### Guest sees no messages

Guests only see messages sent after they join. Ask the host to send a new message after the guest connects. Previous conversation history is not replayed.

### Room code doesn't work

- Codes are case-insensitive — `xk7m2p` and `XK7M2P` are the same
- Codes expire when the host taps **Stop Sharing** or disconnects from the relay
- Ask the host to confirm the room is still active and share a fresh code if needed

### Relay indicator is gray, not teal

The relay isn't connected. Check Settings > Relay Server and tap **Test Connection** to see the specific error.
