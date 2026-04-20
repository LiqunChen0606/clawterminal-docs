# Code Postcards

> Long-press any Claude message → **Share as Postcard** → 1080×1080 PNG with gradient background and syntax-highlighted code. One gesture, ready to share.

## Create a postcard

1. Long-press any AI response in the chatroom
2. Tap **Share as Postcard** from the context menu
3. Wait ~400ms while the image renders on-device
4. iOS share sheet opens with the image attached
5. Send to Twitter, Discord, Slack, iMessage, email, or Save to Photos

No cropping. No theme picking. No external editor.

## What gets captured

- Only the code block(s) from the message
- Surrounding explanation text is NOT included (by design — the point is the code)
- Multiple code blocks in one message → all included in one postcard, labeled by language
- No code blocks → no **Share as Postcard** option

## The image

- **Size:** 1080 × 1080 px (Instagram square; fits Twitter/X cards perfectly)
- **Background:** Diagonal gradient #5B49FF → #B537FF with subtle grain
- **Code card:** White rounded rectangle centered, 10% margin
- **Font:** SF Mono 28pt
- **Syntax colors:** keywords purple, strings coral, comments slate
- **Wordmark:** "ClawTerminal" in bottom-right corner, 40% opacity

Long code → rendered as 1080 × 1920 vertical strip for Instagram Stories.

## Example use cases

### Twitter brag

You solved a problem elegantly. Long-press Claude's one-line answer:

```swift
extension String {
    var reversed: String { String(self.reversed()) }
}
```

Postcard → Tap Twitter. 10 seconds total. Polished, on-brand, instantly shareable.

### Discord server teaching

Explaining reducer patterns in a Swift Discord:

```swift
enum Action { case increment, decrement }

func reducer(state: inout Int, action: Action) {
    switch action {
    case .increment: state += 1
    case .decrement: state -= 1
    }
}
```

Postcard → Share to Discord → pastes inline as an image embed.

### Portfolio post

Build a collection of your best Claude-assisted snippets as postcards. Save to Photos, organize in an album, post as a carousel.

### Slide decks

Import postcards as images into Keynote, PowerPoint, or Figma. Cleaner than trying to format code in slide-native text boxes.

---

## Multi-block postcard

A message with three code blocks becomes one postcard, with dividers:

```
─── Swift ───

struct User { ... }

─── JSON ───

{ "id": 1, ... }

─── Shell ───

curl -X POST ...
```

All three are readable in one share.

---

## Tips

- **Pick the best responses.** Not every Claude response deserves a postcard. Save it for snippets you are proud of.
- **Works best with 10-line functions.** Long files get squeezed; short snippets look crisp.
- **Edit in Photos before posting.** iOS markup works on the postcard image — add arrows, highlights, emojis.
- **Save to Photos** as a third option if you want to queue up a few and batch-post later.
- **Consistent look.** The gradient is fixed — your postcards all match, making a visual series.

Code Postcards is a tiny feature that makes your social presence slightly more polished. Free upgrade for anyone who already posts code screenshots.
