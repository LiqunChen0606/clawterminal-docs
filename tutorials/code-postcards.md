# Code Postcards — Tutorial

> Long-press any Claude response → "Share as Postcard" → a beautiful 1080×1080 image of the code, with syntax highlighting and a gradient backdrop, ready to drop into Twitter, Discord, Slack, or a blog post.

---

## What It Does

Code screenshots are everywhere on social media. They look great. They are also a pain to make — you have to open the code, find a theme that works, crop it right, maybe add the logo.

Code Postcards is a one-gesture version of that whole workflow:

1. Long-press any message in a chatroom
2. Tap **Share as Postcard**
3. A full-resolution 1080×1080 PNG is rendered on-device:
   - Indigo-to-purple gradient background
   - Syntax-highlighted code in a monospaced font
   - Subtle ClawTerminal wordmark in the bottom corner
   - Rounded corners, drop shadow
4. The native iOS share sheet opens
5. Send to Twitter, Discord, Slack, iMessage, email — whatever

No external tool. No cropping. No fiddling with themes.

---

## Getting Started

### Create a postcard

Open any chatroom. Long-press any AI response. A context menu appears with the usual options (Copy, Pin, Reply, etc.) plus a new one:

- **Share as Postcard**

Tap it. After ~400ms of rendering, the iOS share sheet appears with the postcard image ready to go.

### Your first postcard

Try this: send Claude a message like "Show me a neat Swift extension for reversing a string." Get the response. Long-press → Share as Postcard. Tap Twitter or Discord. Post it.

You now have a polished, on-brand code screenshot in about 8 seconds.

---

## What the Postcard Looks Like

- **Canvas:** 1080 × 1080 (Instagram square ratio; also fits Twitter/X cards perfectly)
- **Background:** Diagonal gradient from #5B49FF (indigo) to #B537FF (purple). Subtle grain overlay.
- **Code block:** White rounded-rectangle card centered, with ~10% margin on all sides
- **Syntax highlighting:** Auto-detected language (Swift, Python, TypeScript, Go, Rust, etc.); keywords in purple, strings in coral, comments in slate
- **Font:** SF Mono 28pt
- **Wordmark:** "ClawTerminal" in bottom right corner, 40% opacity

If the code is too long for a single card, it is rendered as a vertical strip up to 1080 × 1920, still shareable as an Instagram Story.

---

## What Gets Shared

Only the code block(s) in the message. Surrounding explanation text is not included — the goal is the code, not the whole conversation.

If the message contains multiple code blocks, all of them are included in one postcard, separated by thin dividers labeled with the language name.

If the message contains no code blocks at all, "Share as Postcard" does not appear in the context menu.

---

## Examples of Good Uses

- **Neat one-liner you want to brag about on Twitter** — "This Swift trick ended up being a one-liner"
- **Bug fix commit — share a before/after** — long-press two messages, share both
- **Teaching moment in a Discord server** — "Here's how I'd structure the reducer"
- **Portfolio post** — snippets from a project, gradient-branded
- **Slide decks** — import postcards as images into Keynote for clean code slides

---

## Tips

- **Works best with short, beautiful code.** Ten lines, cleanly written, reads well on a 1080px square. Long functions get squeezed.
- **Pick the "good" responses.** Not every Claude response deserves a postcard. Be selective — the point is to share code you are proud of.
- **Edit in Photos first if needed.** iOS's built-in crop/markup works on the postcard image. Add emojis or annotations before sharing.
- **Save to Photos** as a third option on the share sheet, if you want to batch up a few and post later.
- **Respects Dynamic Type.** Code rendered in postcards uses a fixed size (28pt) for consistency, not your Dynamic Type setting. Screenshots always look the same regardless of your accessibility settings.

Code Postcards are a small feature that makes CatClaw a little more social. If you already post code screenshots, this is a free upgrade. If you never have, give it a try — your timeline will like it.
