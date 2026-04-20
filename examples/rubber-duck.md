# Rubber Duck Mode

> `/duck` — Claude only asks probing questions. Never gives answers. Classic rubber-duck debugging, formalized.

## Turn it on

```
/duck
```

Yellow "🦆 Duck" chip appears in the chatroom header. Every response from here is a question.

## Turn it off

```
/duck off
```

Chip disappears. Back to normal Claude.

---

## Example session

**You:** I have a race condition in my webhook handler. Duplicate events sometimes get processed.

**Duck:** What makes you confident it's a race condition and not retry logic?

**You:** Because I see two rows with the same external_id inserted within 50ms.

**Duck:** What guarantees uniqueness on external_id in the table?

**You:** There isn't a unique constraint. Huh.

**Duck:** If you added one, what would your handler see on the duplicate insert?

**You:** A constraint violation. Which I'm not catching. So the handler crashes, Stripe retries, it crashes again... okay, I need both the constraint AND idempotency.

**Duck:** What would make the handler idempotent?

**You:** Either check-before-insert in a serializable transaction, or INSERT ... ON CONFLICT DO NOTHING at the DB level.

**Duck:** Which pattern matches how the rest of your DB access layer handles concurrency?

**You:** ON CONFLICT everywhere else. Got it.

---

## When to use `/duck`

- Stuck on a bug you have been staring at too long
- Choosing between two approaches and stuck in analysis paralysis
- Pressure-testing your reasoning before a design doc
- You have been accepting AI code too easily and want to think for yourself again
- Onboarding junior devs who need to build debugging instincts

## When NOT to use `/duck`

- You actually need an answer fast
- The question is factual (syntax, API, constant)
- You are pair-programming under time pressure

---

## The duck's favorite question patterns

| Pattern | Example |
|---------|---------|
| Assumption surfacing | "What makes you confident that X?" |
| Scope narrowing | "Is this the whole scope or a specific case?" |
| Consistency check | "How does this compare to how Y is handled elsewhere?" |
| Next-step prompt | "What would you need to verify to rule that out?" |
| Trade-off framing | "What would you give up to get that?" |

---

## Combining with other commands

### Duck + low effort

```
/duck
/effort low
```

The duck stays short and simple. Does not overthink the questions.

### Duck for design reviews

```
/duck
Here's my proposed API design for the notifications service: [paste spec]
```

The duck will ask about edge cases, scale limits, migration story, backward compatibility — without telling you the answer.

## Tips

- Talk at it, not at the keyboard. Long rambly messages work better than short ones.
- When the duck says "are you sure?" — actually stop and check. Do not auto-say yes.
- Set a 10–15 minute time limit. If the duck is still asking after 20, you are avoiding something.
- Exit duck mode and ask for direct help when you are genuinely stuck.
