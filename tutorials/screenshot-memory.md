# Screenshot Memory — Tutorial

> Capture the visual state of any chatroom with `/screenshot`. Auto-tagged by topic, searchable, viewable as a gallery. Finally, a way to remember that one solution from three weeks ago that you can no longer find.

---

## What It Does

Conversations scroll. Memory fades. Three weeks ago Claude gave you a beautiful explanation of JWT refresh rotation, with a diagram — and now you cannot find it.

Screenshot Memory solves this with two commands:

- `/screenshot` — capture the current chatroom view as an image, auto-tag it, save it locally
- `/screenshots` — open a gallery of all captures with thumbnails, search, filters

It is a visual bookmark system that does not require you to remember where you put anything.

---

## Capturing a Screenshot

### Run it

```
/screenshot
```

CatClaw captures the current chatroom's visible area as a PNG and:

1. Saves to the app's Documents folder (local, not in iCloud Photos)
2. Runs a local heuristic tagger that reads the visible text and picks 1–3 topic tags (e.g. "JWT", "refresh tokens", "authentication")
3. Stores the tags alongside the image

A small banner confirms:

```
Screenshot saved
Tags: JWT, refresh tokens, authentication
```

No network calls. Everything is local.

### What gets captured

- The visible portion of the chatroom — message bubbles, code blocks, context chips
- NOT the system status bar
- NOT the input bar at the bottom
- NOT any modal sheets that are open

If you want a specific message captured, scroll it into view first.

---

## The Gallery — `/screenshots`

```
/screenshots
```

Opens a full-screen gallery sheet with:

- **Grid thumbnails** — 3 columns on iPhone, 5 on iPad
- **Search bar** at the top — filter by tag or visible text
- **Date headers** — "Today", "Yesterday", "Last Week", "Last Month"
- **Tap any thumbnail** — full-screen viewer with pinch-to-zoom, share button, delete button

### Search examples

```
jwt
refresh
auth
stripe webhook
race condition
```

Search is case-insensitive, matches against tags and OCR'd text (the tagger reads visible text for keyword extraction). Ranked by recency.

### Filters

Tap the filter icon (top right of the gallery) to narrow by:

- Date range
- Specific tags
- Chatroom of origin

---

## The Tagger

The tagger runs 100% locally. It:

1. Reads the visible text (already available in memory — no OCR needed)
2. Strips stopwords and common code tokens
3. Picks the 1–3 most prominent terms using a lightweight TF-IDF against a baseline corpus of dev terms
4. Adds a "language" tag if any code block is present (e.g. `swift`, `typescript`, `python`)

It is not perfect. It will sometimes tag a screenshot "error handling" when you wanted "stripe webhooks". The search bar is there for exactly that reason — if the tag is wrong, grep the visible text instead.

---

## Use Cases

### "That beautiful explanation"

You asked Claude to explain pointer semantics in Rust last Tuesday. The answer was a tiny masterpiece. Screenshot it. Four weeks later you are explaining Rust to a colleague — `/screenshots`, search "pointer", done.

### Bug snapshot

Your build breaks with a weird stack trace. You ask Claude. Claude diagnoses it. Before you close the chatroom: `/screenshot`. The whole conversation — error, diagnosis, fix — is now a single searchable artifact.

### Design artifacts

You used Claude to iterate on a schema or API design. Once you settle on a final version, screenshot it. Weeks later when you are implementing, you have the design rationale alongside the design.

### Meeting notes

Use CatClaw's chatroom as a scratchpad during a meeting. When the meeting ends, screenshot anything worth keeping. Beats re-typing into Notion.

---

## Privacy & Storage

- **All local.** Images are saved to CatClaw's Documents folder. They never sync to iCloud, never upload to any server, never get sent to Claude.
- **You can bulk delete.** `/screenshots` gallery has a "Select" mode — select all, tap delete. Instant wipe.
- **Storage impact is small.** Each screenshot is ~200–500 KB. 100 screenshots ~ 30 MB. Your chatroom history is way larger.
- **Export to Photos** — from the full-screen viewer, tap Share → Save to Photos if you want it in your camera roll.

---

## Tips

- **Screenshot before `/compact`.** If you know you are about to compress the chatroom, capture anything visually interesting first — once compacted, the bubble formatting is gone.
- **Tag conventions.** If the auto-tagger misses, the search falls back to full-text of visible content — so don't worry about "perfect" tagging.
- **Per-project gallery discipline.** Use the "Filter by chatroom" option to get a per-project view. Great for showing new team members "here's how we solved X."
- **Combine with Code Postcards.** `/screenshot` is for private memory, Code Postcards is for public sharing. Same idea, different audiences.

Screenshot Memory is your visual bookmark drawer. You do not need it until suddenly you *really* need it.
