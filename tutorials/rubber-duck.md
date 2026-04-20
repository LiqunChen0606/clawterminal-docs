# Rubber Duck Mode — Tutorial

> Stop asking Claude for answers. Start having Claude ask you questions. `/duck` turns the AI into a Socratic rubber duck — it never tells you what to do, only what to ask yourself.

---

## What It Does

Classic rubber-duck debugging: you explain your problem to an inanimate rubber duck, and somewhere in the act of explaining it aloud, you solve it yourself. The duck does not need to be smart — it just needs to listen.

`/duck` turns Claude into that duck. Once active:

- Claude only asks probing questions
- Claude never suggests solutions
- Claude never writes code
- Claude never gives answers
- Every response is a question designed to make you think harder

It sounds restrictive. It is remarkable.

---

## When to Use It

- You are stuck on a bug you have been staring at for 30 minutes
- You are choosing between two approaches and cannot decide
- You think you know the answer but want to pressure-test your reasoning
- You are about to write a design doc and need to clarify your thinking first
- You have been copy-pasting AI-generated code all day and you want to *earn* the next answer

Rubber duck is a focus tool. It works because you already know more than you think — you just have not surfaced it yet.

---

## Getting Started

### Turn it on

```
/duck
```

The chatroom header shows a yellow "🦆 Duck" chip. Every response from now on is a question.

### Exit when you are done

```
/duck off
```

Back to regular Claude. Chip disappears.

---

## A Real Session

**You:** I have a race condition in my webhook handler. Sometimes duplicate events get processed.

**Duck:** What makes you confident it is a race condition and not retry logic?

**You:** Because I see two rows with the same external_id getting inserted within 50ms of each other.

**Duck:** What guarantees uniqueness on external_id in your table?

**You:** Oh. There is no unique constraint.

**Duck:** If you added one, what error would your handler see on the duplicate insert?

**You:** It would throw a constraint violation. Which I'm not catching. Which means the second handler would crash and Stripe would retry, which would crash again... okay wait. I think I need to add both the constraint AND idempotency at the handler level.

**Duck:** What would make the handler idempotent?

**You:** Check for the external_id before inserting. But that's still a race. Let me think. If I wrap the check+insert in a transaction with a serializable isolation level, or use INSERT ... ON CONFLICT DO NOTHING at the database layer—

**Duck:** Which of those options aligns with how the rest of your database access layer handles concurrency?

**You:** We use `ON CONFLICT` elsewhere. Okay. I know what to do.

Notice what happened. The duck did not solve the bug. *You* solved the bug — the duck just kept you honest about your assumptions. Every answer was a question.

---

## The Duck's Questioning Style

The duck has been trained to favor a few specific question types:

| Question type | Example |
|---------------|---------|
| **Assumption surfacing** | "What makes you confident that X?" |
| **Scope narrowing** | "Is this the whole scope, or a specific case?" |
| **Consistency checking** | "How does this compare to how Y is handled elsewhere?" |
| **Next-step prompting** | "What would you need to verify to rule that out?" |
| **Trade-off framing** | "What would you give up to get that?" |

It will never say "I think the issue is..." or "You should try...". If you ask it directly for an answer, it will respond with: "What do you think the answer is, and why?"

---

## Tips

- **Talk at it, not at the keyboard.** Type like you are explaining to a colleague, not querying a search engine. Long, rambly messages work better than short, pointed ones.
- **Let it push back.** When the duck asks "are you sure about that?" — actually stop and check. Do not just say "yes" and move on.
- **Pair with `/effort low`.** The duck is not supposed to think hard. It is supposed to ask short, simple questions. Low effort keeps it from overcomplicating.
- **Set a time limit.** 10–15 minutes is usually enough. If the duck is still asking questions after 20 minutes, you are avoiding something. Exit duck mode and ask for a direct answer.
- **Use it before design docs.** Five minutes of duck before you start writing a spec will clarify half the decisions you would otherwise hand-wave.

Rubber duck debugging works. It has worked for 30 years. `/duck` just means you do not need an actual duck on your desk.
