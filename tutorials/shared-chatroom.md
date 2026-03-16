# Shared Chatroom (Session Sharing)

Share your AI chatroom session with others on the same network in real-time. Guests join via a 6-character room code and watch the conversation as it happens (read-only).

## Requirements

- **Relay server** running on your network (see Settings > Relay Server)
- Both host and guest must be connected to the same relay server
- Host must have an active chatroom session

## Hosting a Session

1. Open a chatroom with an active conversation
2. Tap the **Share** button in the toolbar (person.2.wave.2 icon)
3. The **Share Session** sheet opens with two tabs: **Host** and **Join**
4. On the **Host** tab:
   - Enter your display name (optional, defaults to "Host")
   - Tap **Share**
5. A **6-character room code** appears in large monospace text
6. Share this code with anyone who wants to watch your session

### While Hosting

- The Share toolbar button shows a badge indicating you're actively hosting
- Tap **Copy Code** to copy the room code to clipboard
- Tap **Stop Sharing** to close the room and disconnect all guests
- Your chatroom works exactly as before — guests see your messages and Claude's responses in real-time

## Joining a Session

1. Open any chatroom
2. Tap the **Share** button in the toolbar
3. Switch to the **Join** tab
4. Enter the **6-character room code** (auto-uppercased)
5. Tap **Join**

### While Watching

- You see the host's conversation streaming in real-time
- Messages appear as read-only bubbles (you cannot send messages)
- The view auto-scrolls as new content arrives
- Tap **Leave** to disconnect from the room

## How It Works

The shared chatroom feature uses ClawTerminal's relay server as the communication backbone:

1. **Host creates a room** — the relay server generates a unique 6-character alphanumeric code and associates it with the host's active CLI session
2. **Guest joins by code** — the relay server adds them to the room's guest list
3. **Output broadcast** — as Claude's response streams through the relay, it's simultaneously forwarded to all guests in the room
4. **Room cleanup** — when the host stops sharing, all guests are notified and the room is closed. If a guest disconnects, they're automatically removed from the room

## Relay Server Setup

The shared chatroom requires a running relay server. To configure:

1. Go to **Settings > Relay Server**
2. Enter your relay server URL (e.g., `ws://mac.local:8765`)
3. Enter the authentication token
4. Tap **Save** and **Test Connection**

A teal "Relay" indicator appears in the connection health bar when connected.

## Troubleshooting

### "Relay server not connected" message

- Check that the relay server is running on your Mac
- Verify the URL and auth token in Settings > Relay Server
- Ensure both devices are on the same network (or connected via Tailscale)

### Guest sees no messages

- The guest only sees messages sent **after** joining the room
- Make sure the host's chatroom is actively streaming (send a new message)

### Room code doesn't work

- Room codes are case-insensitive (auto-uppercased)
- Codes expire when the host stops sharing or disconnects
- Ask the host to confirm the code is still active
