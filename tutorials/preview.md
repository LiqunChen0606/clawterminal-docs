# Tutorial: Live Web Preview with `/preview`

> You just finished a UI component at your desk. You want to see how it actually looks on a phone screen -- not a browser DevTools simulation, but the real thing, held in your hand. You type `/preview --start` and your dev server launches in the background, an SSH tunnel opens, and your iPhone shows a live preview of `localhost:3000`. You spot a layout overflow at 390px that would have shipped to production. You circle it, send it to Claude, and get a CSS fix in 10 seconds.

This tutorial covers everything from your first preview to multi-port full-stack apps, responsive testing, console log debugging, and screenshot annotation.

---

## How It Works

`/preview` creates an SSH local port-forwarding tunnel:

```
iPhone browser (WKWebView) → SSH tunnel → Mac localhost:PORT → your dev server
```

The same port-forwarding infrastructure used by the manual Port Forwarding tab, but wired up automatically from a single slash command. You do not need to configure any port forwarding rules manually.

---

## Part 1: Starting Your First Preview

### Prerequisites

1. Active SSH connection to your Mac
2. A web server running on your Mac (or use `--start` to auto-launch one)
3. No additional setup — the tunnel is created automatically

### Step 1: Start your dev server (if not running)

On your Mac in Terminal:

```bash
# React / Next.js
npm run dev

# Vue / Vite
npm run dev

# Flask
flask run

# Django
python manage.py runserver

# Rails
rails server

# Go
go run main.go
```

Or use `--start` and ClawTerminal will do this for you.

### Step 2: Run `/preview`

In your chatroom:

```
/preview
```

ClawTerminal scans the project directory for port configuration:

1. `package.json` — checks `scripts.dev` and `scripts.start` for `--port N` or `PORT=N` patterns
2. `.env` / `.env.local` — checks for `PORT=` or `VITE_PORT=`
3. `vite.config.ts` / `vite.config.js` — checks for `server.port`
4. Falls back to `3000` if none found

A preview sheet opens in the app with your dev server rendered in a WKWebView.

### Step 3: Interact with the preview

The preview is a real browser — you can:
- Tap links and navigate
- Submit forms
- Test touch interactions
- Scroll and pinch-zoom
- Long-press to open context menu

---

## Part 2: Auto-Start with `--start`

If your dev server isn't running yet:

```
/preview --start
```

ClawTerminal:
1. Reads `package.json` to find the dev command (usually `npm run dev`, `yarn dev`, or `pnpm dev`)
2. Launches it in a background tmux window on your Mac named `claw_preview_server`
3. Waits for the server to respond with HTTP 200 (polls every 2 seconds, up to 30 seconds)
4. Opens the SSH tunnel and preview

This is the fastest way to go from "nothing running" to "live preview" in one command.

```
/preview --port 5173 --start
# Explicitly specify port + auto-start
```

---

## Part 3: Multi-Port Preview for Full-Stack Apps

When you have a frontend and backend running on different ports:

```
/preview --port 3000,8080
```

A tab bar appears at the top of the preview sheet:
- **:3000** — your React frontend
- **:8080** — your Node.js or Go API

Tap between tabs to switch ports without closing the preview.

### Common full-stack combinations

```
/preview --port 3000,5000      # React + Flask
/preview --port 3000,8080      # React + Spring Boot / Go
/preview --port 4200,3000      # Angular + Express
/preview --port 8080,5432      # App + pgAdmin (if pgAdmin has a web UI)
```

---

## Part 4: Toolbar Features in Detail

The preview sheet has a toolbar with four buttons:

### Refresh (circular arrow)

Reloads the current page. Equivalent to pull-to-refresh in Mobile Safari.

### Viewport mode (phone/tablet/desktop icon)

Tap to cycle through viewport sizes:

| Mode | Width | Use for |
|------|-------|---------|
| Auto | Fill available space | General use |
| iPhone | 390px | iPhone layout testing |
| iPad | 810px | iPad layout testing |
| Desktop | 1280px | Desktop layout testing |

Viewport modes change the WKWebView's simulated width so your CSS `@media` queries respond correctly. This is essential for testing responsive designs without running a real device farm.

**Tip:** Always test at iPhone mode before creating a PR for any UI change. Many desktop-first bugs only appear at 390px width.

### Console logs (terminal icon)

Opens a panel showing all JavaScript console output captured from the preview page. Supports:

| Level | Color | Source |
|-------|-------|--------|
| `console.log()` | White | General output |
| `console.warn()` | Yellow | Warnings |
| `console.error()` | Red | Errors |
| Unhandled promise rejection | Red | Async errors |

Tap **Send errors to Claude** at the bottom of the panel to forward all captured console errors to your chatroom. Claude will read them and suggest fixes.

**Tip:** This is the fastest way to debug a blank page or broken component. Open the preview, reproduce the issue, open console logs, tap "Send errors to Claude."

### Screenshot + annotate (camera icon)

1. Tap the camera icon — a screenshot of the current preview is captured
2. An annotation editor opens with drawing tools: red, orange, blue, green, white pens + eraser + undo
3. Draw on the screenshot to highlight issues — circle a misaligned button, underline wrong text, arrow to a layout gap
4. Add a text description: "the search bar overlaps the header on iPhone 14 — see red circle"
5. Tap **Send to Claude** — the annotated screenshot and description are sent to your chatroom

Claude reads both the screenshot and the description and responds with specific CSS or layout fixes.

**Example workflow:**

```
/preview --port 3000

# (In the preview)
# Navigate to /settings page
# It looks broken on iPhone viewport

# Tap camera icon
# Draw a red circle around the broken section
# Type: "the settings form overflows on the right side at 390px width"
# Tap "Send to Claude"

Claude: "The form has `min-width: 400px` on line 23 of Settings.css —
         change it to `min-width: min(400px, 100%)` to fix the overflow."
```

---

## Part 5: Responsive Testing Workflow

`/preview` is especially useful as a pre-commit visual check. Here's a systematic responsive testing workflow:

### 1. Desktop check

Open preview, leave viewport on **Auto**. Verify the layout looks correct on the full iPhone screen width.

### 2. Mobile check

Tap viewport icon → **iPhone** (390px). This is the most common failure point for desktop-first CSS. Check:
- Navigation doesn't overflow
- Forms are usable at full width
- Text is readable (not too small, not too large)
- Buttons are large enough to tap (44px minimum)
- Images scale correctly

### 3. Tablet check

Tap viewport icon → **iPad** (810px). Check:
- Two-column layouts work correctly
- Sidebars don't collapse unexpectedly
- Touch targets are appropriate

### 4. Annotate issues

For each viewport, screenshot and annotate any issues. Send them to Claude for fixes.

### 5. Re-verify

After Claude applies fixes, tap **Refresh** in the preview to reload the page. Verify the fix without leaving the app.

---

## Part 6: Debugging with Console Logs

The console log panel is a lightweight alternative to opening Safari's Web Inspector when debugging JavaScript issues.

### Common debug patterns

**Debugging a blank page:**

```
/preview
# Page shows blank or loading spinner stuck

# Open console logs
# Look for red errors like:
# "Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')"
# "Failed to fetch: NetworkError when attempting to fetch resource"

# Tap "Send errors to Claude"
# Claude diagnoses the root cause
```

**Debugging broken API calls:**

```
/preview --port 3000,8080
# Switch to :8080 tab (your API)
# Navigate to an endpoint that should return JSON
# Open console logs — look for CORS errors or 4xx/5xx responses

# Send to Claude:
# "The frontend at 3000 is getting a CORS error when calling the API at 8080.
#  Console shows: Access-Control-Allow-Origin missing."
# Claude suggests the CORS middleware fix
```

**Debugging async errors:**

```
# Console logs panel shows:
# [Unhandled Promise Rejection] Error: Network request failed

# Tap "Send errors to Claude"
# Claude identifies which async function is throwing and suggests a fix
```

---

## Part 7: Closing the Preview

```
/preview stop
```

This closes the SSH tunnel. The dev server on your Mac continues running — only the port forwarding is torn down.

If you want to stop the dev server too, you need to do that separately. The server is running in a tmux window named `claw_preview_server` if you used `--start`:

```bash
tmux kill-session -t claw_preview_server
```

Or just `Ctrl-C` in the terminal where you started it manually.

---

## Part 8: Troubleshooting

### Blank white page in preview

**Most likely causes:**
1. Dev server not running — run `curl localhost:3000` on your Mac to confirm
2. Wrong port — try `/preview --port <actual-port>`
3. Server is running but not accepting connections — wait a few seconds and refresh

**Fix:** Use `/preview stop` then `/preview --port <correct-port>` with the explicit port.

### "Connection refused" error

The SSH tunnel connected but nothing is listening on that port.

**Fix:**
- Confirm the server is running: `curl localhost:<port>` on your Mac
- Use `/preview --start` to auto-launch it if it's crashed

### Preview loads but CSS looks wrong

**Most common causes:**
- Static assets being served from absolute paths that don't resolve through the tunnel
- The dev server isn't serving with HTTPS but your page expects it

**Fix:**
- Check the console log panel for 404s on CSS/JS files
- Try using relative asset paths
- For Vite: set `base: './'` in vite.config.ts

### Preview works but API calls fail

If your frontend makes API calls to a different port and that port isn't tunneled:

**Fix:** Use multi-port preview:
```
/preview --port 3000,8080
```

API calls to `localhost:8080` will work because that port is also tunneled.

### Console log panel is empty

The JavaScript console bridge is injected when the page loads. If you opened the console panel after navigating to a new page, the bridge may not have been injected.

**Fix:** Tap **Refresh** to reload the page with the console bridge active from the start.

---

## Supported Frameworks

`/preview` works with any HTTP server on any port. Tested with:

- React (Create React App, Vite)
- Next.js
- Vue.js (Vite)
- Nuxt.js
- Angular
- Svelte / SvelteKit
- Flask (Python)
- Django
- FastAPI
- Rails
- Express / Node.js
- Go (standard `net/http` or frameworks like Gin, Echo)
- Spring Boot
- Jupyter Notebook
- Storybook
- Swagger UI / Redoc

If it responds to HTTP on a port, `/preview` can show it.

---

## Pre-Commit Checklist Using `/preview`

Before running `/pr create`, use this checklist:

```
1. /preview --start
   ✓ Dev server launches
   ✓ Preview opens

2. Switch to iPhone viewport (390px)
   ✓ Layout looks correct
   ✓ No horizontal overflow

3. Check console logs panel
   ✓ No red errors
   ✓ No unhandled rejections

4. Navigate to the changed pages
   ✓ Visual regression check against expected behavior

5. Screenshot + annotate anything that needs fixing
   Send to Claude → fix → refresh → re-check

6. /preview stop
7. /pr create
```

See [examples/preview.md](../examples/preview.md) for copy-paste-ready examples.

---

## What's Next?

- **[GitHub PR Workflow](pr-workflow.md)** -- After your visual check passes, run `/pr create` to ship the PR without leaving the app.
- **[Session Handoff](handoff.md)** -- Found issues in the preview? Hand off to your Mac for a full-keyboard debugging session.
- **[AI Analysis & Automation](ai-analysis.md)** -- Use `/security` before shipping to catch vulnerabilities, or `/gentest` to generate tests for the feature you just previewed.
