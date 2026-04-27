# Trajectory Library — Tutorial

> When you run `/research` or use Search Mode, the winning result is great — but it's ephemeral. Close the result sheet and it's buried in message history. The next question you ask Claude has no idea that answer exists. Trajectory Library fixes that by letting you **pin** a result directly into your chatroom's preamble, so every subsequent message Claude sees is grounded by it.

---

## Part 1: What It Is

A pinned result is a compact snapshot of a research win: the winning text, the judge's score, and the scorer's rationale for why it won. That snapshot gets injected into the preamble for every future message in the chatroom, right alongside your skills and memory.

The effect is subtle but meaningful. Claude treats the pinned conclusion as established context — not something it needs to re-derive or hedge about. If you pinned "use JWTs with a server-side blocklist for revocation," the next time you ask "how should I implement the logout endpoint?" Claude already knows the auth architecture it's building within.

You can have up to **3 pins** per chatroom. Pins survive Frugal Mode (they're explicit user opt-ins, not auto-injected boilerplate). Pins do not expire — they stay until you remove them.

---

## Part 2: When to Use It

Pinning is most valuable when you've made a decision and want Claude to build on it rather than revisit it.

**Good fits:**

- You ran `/research` on an architecture question and got a strong winner (score ≥ 0.75). You're about to ask 5 follow-up implementation questions. Pin the answer first.
- You used Search Mode to settle a naming debate. You'll write copy and docs later in the same chatroom. Pin the name so Claude never wavers on it.
- You're using the chatroom as a lightweight architecture decision record (ADR). Pin the chosen approach so the chatroom remembers what was decided even after a long session.
- You ran research on a design principle ("errors should surface at boundaries, not be swallowed internally"). You want every subsequent code review in the room to apply it.

**Not great fits:**

- Quick lookups: "what's the Postgres default port?" — no need to pin a one-shot answer.
- Debugging sessions: the result is specific to one bug, not a cross-cutting context.
- Agentic tool work: pins inject into the text preamble; they don't affect tool calls or the agent's codebase access.
- Tasks where conclusions might change in the same chatroom: if you might revisit the decision, a pin commits you to it — use normal chat or unpin when the decision changes.

---

## Part 3: How to Pin a Result

### From a `/research` run

1. Type `/research <your question>` and tap send.
2. The Research Task card opens. Configure if needed, then tap **Run Research**.
3. The live progress sheet runs (~10–30 seconds), then the result sheet appears with the 🏆 winner.
4. Tap the **Pin** button in the result sheet's toolbar. It sits next to "Send as Message."
5. You're done. The result sheet closes and the 📌 chip appears in your chatroom's mode banner.

### From a Search Mode message

1. After a message lands in a Search Mode chatroom, tap the result message (the one with the score badge).
2. The result sheet opens showing the 🏆 winner and trajectory.
3. Tap **Pin** in the toolbar — same button, same effect.
4. The 📌 chip appears in the mode banner.

---

## Part 4: The 📌 Chip and What It Shows

The mode banner above the input field shows chips for active chatroom modes. A pinned result adds an indigo **📌 N** chip, where N is the count of active pins.

Tap the chip to open the **pin management sheet**. You'll see a list of all pinned results for this chatroom, each showing:

- The research task (truncated)
- A snippet of the winner's text
- The score it received
- How long ago it was pinned

Each row has a **⋯** menu. The only action in the menu is **Unpin** — removing the pin immediately and updating the preamble for your next message.

---

## Part 5: The Cap and Eviction Dialog

You can have a maximum of **3 pins** per chatroom. Attempting to add a 4th opens an eviction dialog:

> **You already have 3 pinned results.**
> The oldest — *"[task snippet]"* — will be removed to make room.
>
> [Cancel] [Replace]

Tap **Replace** to evict the oldest pin and add the new one. Tap **Cancel** to keep your current 3 and discard the new one. If you want to remove a specific pin instead of the oldest, cancel, open the 📌 chip management sheet, unpin the one you want gone, and then re-pin the new result.

The cap keeps the preamble lean. Three high-quality conclusions injected into every message is useful; more than that and you're adding noise.

---

## Part 6: Verifying the Pin Is Working

The surest way to confirm a pin is influencing Claude:

1. With a result pinned, send a follow-up question that touches on the pinned conclusion.
2. After the reply arrives, type `/think` with a question about the pinned topic. The thinking blocks Claude generates will often reference the pinned context explicitly — you can see it working.

You can also send a direct question: "What do you know about how we decided to handle auth in this project?" If the pin is in effect, Claude will summarize the winner without you having to re-explain it.

---

## Part 7: Frugal Mode Interaction

Frugal Mode strips most of the preamble to reduce token cost: skills, memory, dream profile, device context, tribal knowledge, and regular file pins all get dropped.

**Trajectory Library pins are NOT stripped by Frugal Mode.** They're treated as explicit user intent — you made a decision and opted in to keeping it in context. Frugal Mode respects that.

This means if you're working in Frugal Mode and you pin a research result, Claude will still see the pin even though everything else is stripped. This is intentional: your architectural decisions shouldn't disappear just because you're trying to save tokens on implementation questions.

---

## Part 8: Relationship to Search Mode and `/research`

Pins come from research results. They don't create new research — they preserve the outcome of research you've already run.

The typical workflow:
1. Start with Search Mode on (or run `/research`) to explore a question thoroughly.
2. Read the trajectory graph to understand why the winner won.
3. If you agree with the winner, pin it.
4. Flip Search Mode off (or just send normally for `/research`) for follow-up implementation work. The pin carries the decision forward.

You can also mix: keep Search Mode on and pin strong results as they come in. Each pinned result anchors one more decision, so later searches in the same chatroom are already building on established ground.

---

## Non-Goals (What It Doesn't Do)

A few things people often ask about:

- **No global library.** Pins are per-chatroom only. There's no cross-chatroom collection of pinned research results.
- **No auto-pin.** Every pin is manually initiated. Nothing gets pinned without you tapping the button.
- **No editing or renaming.** The pinned content is exactly the winner's text and rationale. You can't modify it.
- **No expiration.** Pins stay until you explicitly unpin them via the management sheet.

---

## Quick Reference

| Action | How |
|--------|-----|
| Pin a `/research` result | Tap **Pin** in the result sheet toolbar |
| Pin a Search Mode result | Tap the result message → result sheet → **Pin** in toolbar |
| See active pins | Tap the **📌 N** chip in the mode banner |
| Unpin a result | Tap 📌 chip → ⋯ menu next to the pin → **Unpin** |
| Add a 4th pin | The eviction dialog offers Cancel or Replace (removes oldest) |
| Verify pin is active | Use `/think` on a follow-up question; the pin appears in the reasoning |

**Cap:** 3 pins per chatroom.

**Survives Frugal Mode:** Yes — pins are always injected, even with Frugal on.

**Best for:** decisions you've made and want Claude to build on across many follow-up messages.
