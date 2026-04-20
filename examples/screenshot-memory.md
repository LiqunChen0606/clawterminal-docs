# Screenshot Memory

> `/screenshot` captures the current chatroom view, auto-tags it, and saves locally. `/screenshots` opens a searchable gallery.

## Capture a screenshot

```
/screenshot
```

Output:

```
Screenshot saved
Tags: JWT, refresh tokens, authentication
```

Captures:

- Visible chatroom area (message bubbles, code, chips)
- NOT the status bar or input bar
- NOT any open modals

Tags are auto-extracted locally (no network, no OCR) from the visible message text.

## Open the gallery

```
/screenshots
```

Full-screen gallery with:

- **Grid thumbnails** (3 columns iPhone, 5 iPad)
- **Search bar** at top — matches tags and visible text
- **Date headers** — Today, Yesterday, Last Week, Last Month
- **Tap a thumbnail** → full-screen viewer with zoom, share, delete

## Search examples

```
jwt
refresh
auth
stripe webhook
race condition
```

Case-insensitive, ranked by recency.

## Filters

Tap the filter icon (top right). Narrow by:

- Date range
- Specific tags
- Chatroom of origin

---

## Real scenarios

### "That one beautiful explanation"

Three weeks ago Claude explained JWT rotation with a diagram. Now you want it for a blog post.

```
/screenshots
[search: jwt]
→ 3 results, top one is exactly it
[tap → full screen → share to Notes]
```

### Bug postmortem

Your build broke with a weird stack trace. Claude diagnosed it brilliantly.

```
/screenshot
Tags: build error, SwiftData, migration
```

Three months later when the same error recurs: `/screenshots` → search "migration" → open the old diagnosis → fix in 2 minutes.

### Design decision archive

You iterated on a schema design with Claude for 40 minutes. You settled on a final version.

```
[scroll to final proposed schema]
/screenshot
Tags: database schema, payments, users
```

Weeks later when implementing: gallery → tap → you have the design + rationale as one artifact.

### Meeting capture

Use CatClaw as a meeting scratchpad. When meeting ends:

```
/screenshot
Tags: Q2 planning, auth refactor
```

Later: searchable, dated, context-preserved. Beats re-typing into Notion.

---

## Privacy & storage

- **All local.** Saved to CatClaw Documents folder. Never synced to iCloud. Never sent to Claude.
- **~200–500 KB per screenshot.** 100 screenshots ≈ 30 MB.
- **Bulk delete.** Gallery has "Select" mode — select all → delete.
- **Export to Photos.** Full-screen viewer → Share → Save to Photos.

---

## When auto-tagging misses

The tagger picks the 1–3 most prominent keywords via local TF-IDF. It is not perfect. When it mis-tags:

- Search falls back to full-text of the visible content, so you can still find the screenshot
- Or use the filter dropdown to find by date / chatroom

## Tips

- **Screenshot before `/compact`.** Compaction changes bubble formatting — capture the visual state first.
- **Per-project view.** Use "Filter by chatroom" for a project-specific album. Great for showing new teammates how you solved something.
- **Combine with Code Postcards.** `/screenshot` = private memory. Code Postcards = public sharing. Same instinct, different audience.
- **Long-press screenshot actions.** In the viewer, long-press the image for "Save to Photos", "Copy", "Share".
