# In-App Docs — Tutorial

> Every CatClaw tutorial and example lives inside the app. Open the docs browser with `/docs`, search offline, and tap "Try in chatroom" to drop an example command straight into your input bar. When you want fresh content, tap Refresh — CatClaw pulls the latest from GitHub in a few seconds.

---

## Part 1: Why In-App Docs

You already know the feeling — you remember there's a slash command that does *exactly* the thing you need, but you can't remember the name. You tab over to a browser, wait for a web page, skim three paragraphs to find the command, copy it, switch back, paste it. By the time you're back in the chatroom the flow is gone.

In-app docs kill that detour. Every tutorial and example that lives on the website is bundled inside the app at install time. You get a searchable reader one tap away, even on a plane or a subway.

When the team ships new docs between releases, a Refresh button pulls them down — no App Store update needed.

---

## Part 2: Two Ways to Open the Docs

### From any chatroom

```
/docs
```

The docs sheet slides up. You see a search field, an **All / Tutorials / Examples** segmented picker, and a scrollable list of every doc.

You can jump straight to a category or search:

```
/docs tutorials
/docs examples
/docs frugal
/docs handoff
```

A partial word is fine — substring match on titles and summaries.

### From Settings

**Settings → Help & Documentation** opens the same browser outside a chatroom context. Useful when you're exploring what CatClaw can do before you start a session.

In Settings mode the "Try in chatroom" button is hidden (there's no input to fill) — you can still read, share, and refresh.

---

## Part 3: Reading a Doc

Tap any row. The reader pushes in with:

- Rendered markdown — headings, lists, tables, inline code
- Syntax-highlighted code blocks in horizontal scroll views
- A **Share** toolbar item — sends the raw markdown via the iOS share sheet (save to Files, mail it to yourself, pipe into another app)
- A floating **Try in chatroom** button when the doc has an example command

Tap "Try in chatroom" and the sheet dismisses. Your input bar is now pre-filled with the first fenced command from the example. Edit it, then hit Send.

---

## Part 4: Refreshing from GitHub

Below the search field you'll see a line like `Last updated: 3 hours ago · Refresh`.

Tap Refresh and CatClaw fetches every tutorial and example from the `clawterminal-docs` GitHub repo. Updated files overwrite the bundled copies in a local cache; the next time you open a doc, you see the fresh version.

**Rate limit:** once per hour. If you tap Refresh twice in quick succession, the button is disabled with a tooltip that tells you when you can try again. This keeps GitHub's API rate limits friendly.

**Failure modes:**

- No network → "Refresh failed. Using bundled docs." Nothing is lost; the offline snapshot still works.
- GitHub rate-limited (rare) → an explanatory message with a retry time.
- Partial success → the files that downloaded are live; failed ones fall back to the bundled copy.

---

## Part 5: When to Use It

- **Forgot a command name.** `/docs <guess>` — substring search finds it fast.
- **Mid-flight on a plane.** The bundled snapshot is everything. No connection required.
- **Before a TestFlight session.** Tap Refresh to grab notes for features that shipped between app versions.
- **Teaching a teammate.** Open `/docs tutorials`, hand them the phone — they can browse guided content without leaving the app.
- **Writing a blog post.** Share a doc's raw markdown straight to your notes app.

The whole system is designed to keep you in flow. Search, read, try, move on.
