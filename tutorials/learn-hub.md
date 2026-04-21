# Learn Hub — Tutorial

> Everything you can learn about CatClaw lives behind one search field. Interactive tutorials, written guides, and example walkthroughs — all in a single sheet, all searchable offline, all one tap from inside any chatroom. If you ever thought "there must be a command for this," the Learn Hub is where you find out.

---

## Part 1: Why One Hub

CatClaw used to have two separate learning surfaces. A Tutorials list showed interactive guided tracks. A Docs browser showed bundled markdown. They both existed for good reasons, and they both were rarely opened twice in the same session because you had to remember which one held what you were looking for.

The Learn Hub collapses the two. Now every piece of learning content — interactive track, written tutorial, example — is one search field away. You type a fuzzy guess at what you want, and everything that matches renders in a single list with a colored badge that tells you what kind of thing it is.

It's offline by default (the full snapshot ships inside the app), refreshable on demand (tap a button to pull the latest from GitHub), and available from four places — so you never have to remember how to get there.

---

## Part 2: Four Ways to Open the Hub

### 1. The Learn button in the toolbar

Every chatroom has a **Learn** button in the toolbar (the open-book icon). Tap it — the hub slides up. This is the fastest way if you're already in a chatroom and just want to browse.

### 2. The `/docs` slash command

```
/docs
/docs tutorials
/docs examples
/docs interactive
/docs frugal
```

No argument opens the hub with no filter. `tutorials`, `examples`, and `interactive` pre-filter by category. Any other word is treated as a search query and the hub opens already filtered to matches.

### 3. The `/tutorial` slash command

```
/tutorial
/tutorial beginner
```

With no argument, `/tutorial` opens the hub filtered to the Interactive category — handy when you want a guided track but don't remember the name. With a track name, it still launches that track directly (same behavior as before the unification), so your muscle memory is safe.

### 4. Settings → Learn Hub

Outside any chatroom, **Settings → Learn Hub** opens the same view. Useful when you're exploring what CatClaw can do before you start a session. In Settings mode the "Try in chatroom" button is hidden (there's no input to fill) — you can still read, search, and refresh.

---

## Part 3: Three Kinds of Content, Three Colored Badges

Every row in the hub wears a category badge so you can scan the list and know immediately what you're looking at:

- **Interactive** (green) — a guided track that validates your real commands as you go. Six tracks ship today: beginner, terminal, workflows, agents, devops, power. Progress is tracked per-track.
- **Tutorial** (blue) — a written walkthrough of a feature. Same tone as the docs on the website, bundled inside the app so it works on a plane.
- **Example** (orange) — a short scenario-based guide that shows the command in a realistic context. Every example has a "Try in chatroom" button that drops the first command into your input bar.

Tap any row and the reader pushes in. Rendered markdown, syntax-highlighted code blocks, a Share toolbar that sends the raw markdown via the iOS share sheet, and — for examples — that floating Try-in-chatroom button.

For interactive rows, the detail panel shows your progress (steps completed / total steps) so you can pick up mid-track without losing your place.

---

## Part 4: Searching

The search field at the top of the hub does substring matching across:

- Title
- Summary / first paragraph
- Body text (bundled markdown)

A partial word is fine. `frug` finds Frugal Mode. `conflict` finds the merge conflicts guide. `whatif` finds the what-if simulator.

A few tips:

- **Start broad, then narrow.** Type one word, scan the badges, pick the category you want.
- **Search before asking.** If you're about to ask Claude "is there a command that does X?", try `/docs X` first — the Hub is faster and free.
- **Use category filters to learn laterally.** Tap the **Interactive** filter and scroll — it's a decent map of what CatClaw can do.

---

## Part 5: Refreshing from GitHub

Below the search field you'll see a line like `Last updated: 3 hours ago · Refresh`.

Tap Refresh and CatClaw fetches the latest tutorials and examples from the `clawterminal-docs` GitHub repo. Updated files overwrite the bundled copies in a local cache; the next time you open a doc, you see the fresh version.

**Rate limit:** once per hour. If you tap Refresh twice in quick succession, the button is disabled with a tooltip that tells you when you can try again. This keeps GitHub's API rate limits friendly.

**Failure modes:**

- No network → "Refresh failed. Using bundled docs." Nothing is lost; the offline snapshot still works.
- GitHub rate-limited (rare) → an explanatory message with a retry time.
- Partial success → the files that downloaded are live; failed ones fall back to the bundled copy.

Interactive tracks are shipped with the app itself (they're Swift code, not markdown) so they don't need a refresh — just pull the latest App Store build.

---

## Part 6: When to Reach for the Hub

- **Forgot a command name.** Tap Learn, type a guess. Fastest path to remembering.
- **Mid-flight on a plane.** The bundled snapshot is everything. No connection required.
- **Teaching a teammate.** Open the hub, hand them the phone — they browse guided content without leaving the app.
- **Before a TestFlight session.** Tap Refresh to grab notes for features that shipped between app versions.
- **Pre-onboarding.** Brand new to CatClaw? Open Settings → Learn Hub, filter to Interactive, and run the beginner track.

One sheet, one search, every piece of learning content. That's the whole system.
