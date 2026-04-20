# In-App Docs

> Browse every CatClaw tutorial and example inside the app — offline, searchable, and one tap from Try-in-chatroom. `/docs` opens the browser; Settings → Help & Documentation opens the same view.

## Commands

```
/docs                — open the browser
/docs tutorials      — filter to Tutorials
/docs examples       — filter to Examples
/docs <query>        — substring search on titles + summaries
```

---

## Example: Offline on a plane

You're on a red-eye with no Wi-Fi. You remember there's a slash command that predicts what a shell command will do, but you forget the name.

```
/docs whatif
```

The browser opens already filtered to the match. You read the example, tap **Try in chatroom**, and the `/whatif` command drops into your input bar. You edit it and ship a safe preview — no network required.

---

## Example: Quickly finding a forgotten command

Halfway through a refactor, you want to capture a gotcha into tribal knowledge. You know there's a command but can't remember whether it's `/tribal` or `/gotcha` or something else.

```
/docs tribal
```

Browser opens, one match, tap through, Try in chatroom — back in flow in 10 seconds.

---

## Example: Refreshing after a between-release feature

The team just announced a new feature on the changelog. You'd rather not wait for the next App Store release to see its docs.

1. Open `/docs`
2. Notice the header: `Last updated: 6 days ago · Refresh`
3. Tap **Refresh**
4. 3 seconds later: `Last updated: just now · Refresh`
5. Search for the new feature — its tutorial is live inside the app

Rate limit is 1 refresh per hour, which is more than enough for normal reading.

---

## Example: Teaching a new user

You're showing CatClaw to a colleague. Instead of alt-tabbing to the website, you open:

```
/docs tutorials
```

Hand them the phone. They scroll, tap a tutorial, read, tap Try in chatroom to run one of the examples. They never leave the app.

---

## Example: Settings access without a chatroom

You just installed the app, opened Settings to poke around, and want to browse what's possible before creating your first connection.

**Settings → Help & Documentation** opens the same browser. No Try-in-chatroom button (there's no chatroom to insert into), but search, reader, share, and refresh all work.

---

## Tips

- Fresh off an App Store update? The welcome tour re-launches automatically and includes a page pointing to `/docs`.
- Every time you build from source, a pre-build script rsyncs the sibling `clawterminal-docs` repo into the app bundle — so local builds always ship a current snapshot.
- The share toolbar item sends the raw markdown. Paste into your notes app or email it to yourself for later reading.
- Combine with `/pin` — `/docs` the doc, share the markdown to a file on your Mac, then `/pin` that file so Claude has the context every message.
