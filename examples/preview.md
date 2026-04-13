# /preview — Live Web Preview Examples

Open a live browser preview of your Mac's dev server on your iPhone or iPad via SSH port forwarding. Works with any HTTP server — React, Vue, Next.js, Flask, Rails, Go, etc.

---

## Basic Usage: Auto-Detect Port

```
/preview
```

ClawTerminal scans the project directory for port configuration (package.json, .env, vite.config) and opens the preview automatically. If no port is detected, falls back to `3000`.

---

## Specify Port Explicitly

```
/preview --port 3000
```

```
/preview --port 8080
```

Use when your server runs on a non-standard port or auto-detection doesn't find the right one.

---

## Multi-Port: Frontend + Backend

```
/preview --port 3000,8080
```

Opens a tab bar in the preview sheet so you can switch between ports:
- Tab 1: `localhost:3000` (React frontend)
- Tab 2: `localhost:8080` (Node.js API)

Useful for full-stack development — check the UI and the raw API response without leaving the app.

---

## Auto-Start Dev Server

```
/preview --start
```

Launches your dev server (detected from `package.json scripts.dev`) in a background tmux window on your Mac, waits for the server to be ready (HTTP 200), then opens the preview. No need to start the server separately.

---

## Auto-Start with Explicit Port

```
/preview --port 5173 --start
```

Good for Vite projects that run on 5173 by default.

---

## Close the Preview

```
/preview stop
```

Closes the SSH tunnel. The dev server on your Mac keeps running — only the tunnel is torn down.

---

## Toolbar Features

### Screenshot + Annotate

1. Tap the camera icon in the preview toolbar
2. A screenshot of the current preview is captured
3. Draw on it with colored pens (red, orange, blue, green, white)
4. Type a description: "the login button is misaligned on mobile"
5. Tap **Send to Claude** — the annotated screenshot + description goes to your chatroom

Claude then reads the annotation and proposes a CSS/layout fix.

### Console Logs

Tap the terminal icon to open the JS console panel. Captures:

- `console.log` output (white)
- `console.warn` (yellow)
- `console.error` (red)
- Unhandled promise rejections (red)

Tap **Send errors to Claude** to forward all captured errors to your chatroom for diagnosis.

### Responsive Viewport Modes

Tap the viewport icon to simulate different screen widths:

| Mode | Width | Use for |
|------|-------|---------|
| Auto | Fill available space | General preview |
| iPhone | 390px | Mobile layout testing |
| iPad | 810px | Tablet layout testing |
| Desktop | 1280px | Desktop layout testing |

---

## Real-World Examples

### React app with Vite

```
/preview --start
# Launches: npm run dev (in tmux)
# Waits for server ready
# Opens preview at detected port (usually 5173)
```

### Next.js app

```
/preview --port 3000 --start
# Launches: npm run dev
# Opens at localhost:3000
```

### Flask API + React frontend

```
/preview --port 3000,5000
# Tab 1: React at 3000
# Tab 2: Flask at 5000
```

### Django app

```
/preview --port 8000
# (Assuming server is already running: python manage.py runserver)
```

### Pre-commit visual check

```
# Start the dev server and take a final look before creating the PR
/preview --start
# Check layout at iPhone viewport
# Screenshot and annotate any issues
# Fix them, then:
/pr create
```

### Debug a JavaScript error

```
/preview --port 3000
# Open the console log panel
# Reproduce the error in the preview
# Tap "Send errors to Claude"
# Claude diagnoses and proposes a fix
```

---

## Auto-Detection Priority

ClawTerminal scans in this order for the port:

1. `package.json` — `scripts.dev` or `scripts.start` (extracts `--port N` or `PORT=N`)
2. `.env` / `.env.local` — `PORT=` or `VITE_PORT=`
3. `vite.config.ts` / `vite.config.js` — `server.port`
4. Falls back to `3000`

---

## Troubleshooting

**Preview shows a blank page**
- Make sure your dev server is actually running on your Mac (`curl localhost:3000` in Terminal)
- Try `/preview --port <correct-port>` with the explicit port
- Use `/preview stop` then `/preview --port 3000` to restart the tunnel

**"Connection refused" in preview**
- The server may have crashed — check the terminal on your Mac
- Use `/preview --start` to auto-relaunch

**Preview works but looks wrong on mobile**
- The dev server may be serving desktop-only assets — tap the viewport icon to switch to iPhone mode (390px)
- Make sure your CSS has proper `@media` queries or a responsive meta viewport tag
