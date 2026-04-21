# Learn Hub

> One searchable sheet for tutorials, docs, and examples. Open it from the Learn button in the chatroom toolbar, or type `/docs`, or `/tutorial`, or visit Settings → Learn Hub. Works offline; tap Refresh to pull the latest.

## Commands

```
/docs                — open the Learn Hub
/docs tutorials      — filter to written Tutorials
/docs examples       — filter to Examples
/docs interactive    — filter to Interactive tutorial tracks
/docs <query>        — substring search across everything
/tutorial            — open the Hub filtered to Interactive tracks
/tutorial <track>    — launch a specific interactive track directly
```

---

## Example: Finding a command you half-remember

Halfway through a refactor, you recall there's a slash command that predicts what a shell command would do. You forget whether it's `/preview` or `/whatif` or something else.

```
/docs whatif
```

The hub opens already filtered to the single match. Orange **Example** badge, one tap, the floating **Try in chatroom** button drops `/whatif git reset --hard HEAD~3` into your input bar. Edit the command, send it. Ten seconds, no tab-switching.

---

## Example: Bootstrapping a new user

Your teammate is trying CatClaw for the first time and doesn't know where to start. Instead of narrating ten features, you hand them the phone:

```
/docs interactive
```

The hub opens filtered to the six interactive tracks, each with its current progress. They pick **beginner**, tap through, and the tutorial engine validates their real commands as they go. You can walk away and pour coffee — the app teaches itself.

---

## Example: Offline on a plane

You're on a red-eye with no Wi-Fi. You remember there's a workflow command that generates a structured release spec from a feature description, but you can't recall the syntax.

```
/docs spec
```

The bundled snapshot resolves the search locally — no network required. A blue **Tutorial** badge and an orange **Example** badge appear. The example has the exact syntax you forgot. Try-in-chatroom drops the template in. Shipping from 35,000 feet.

---

## Example: Refreshing after a between-release feature

The team announced a new feature on the changelog but the latest App Store build doesn't have its docs bundled yet.

1. Open `/docs`
2. Glance at the header: `Last updated: 6 days ago · Refresh`
3. Tap **Refresh**
4. Three seconds later: `Last updated: just now · Refresh`
5. Search for the new feature — its written tutorial and example are live inside the hub

Interactive tracks don't refresh this way — they ship with the app binary — but any written content is one tap away.

---

## Example: Settings access without a chatroom

You just installed the app, opened Settings to poke around, and want to browse what's possible before connecting your Mac.

**Settings → Learn Hub** opens the same view. The Try-in-chatroom button is hidden (there's no input bar to fill), but search, reading, share-to-markdown, and Refresh all work — a tour of the app before you ever open a chatroom.

---

## Tips

- **Badges as category filter.** Interactive (green), Tutorial (blue), Example (orange). Glance-and-go.
- **One hub, four doors.** Toolbar Learn button, `/docs`, `/tutorial`, Settings → Learn Hub — pick whatever's closest to your thumb.
- **Progress survives restarts.** Interactive tracks remember where you left off, even across app updates.
- **Share sheet works on the raw markdown.** Paste a tutorial into your notes app, email it to yourself, pipe it into another tool.
- **Pair with `/pin`.** Read a tutorial in the hub, share its markdown to a file on your Mac, then `/pin` that file so Claude has the context every message.
