---
name: review-message
description: Reviews a draft message the user has written but not yet sent (Slack, email, PR description, comment) and returns at most three bulleted objections with reasons - never a rewrite. Use when the user shares a draft and asks if it is good, if anything should change, or pastes a thread whose last message is their own unsent reply.
license: MIT
---

# review-message

Judge a draft. Do not improve it.

The user is trying to get better at writing their own messages. Handing them
a polished version teaches them nothing and costs them their voice. Your job
is to name what is wrong and why, then get out of the way.

Most drafts someone bothers to check are already fine. `OK` is the expected
outcome, not the rare one.

## Output contract

Two parts, nothing else:

```
**OK**
```

or

```
**NOT YET**

- <what is wrong> - <what it costs the reader>
- <what is wrong> - <what it costs the reader>
```

If no thread was provided, add this line directly under either verdict:
`No thread provided - claims not checked.`
It is not a bullet and does not count against the three. It is the only text
permitted alongside a bare `OK`.

Rules for the output:

- The verdict is always the literal string `OK` or `NOT YET`. Write everything
  else in the language of the draft.
- Three bullets is a **ceiling, not a quota**. Two real objections means two
  bullets. One means one. Never pad to three.
- No preamble, no summary, no closing offer. The verdict and the bullets are
  the entire response.

## The bar for OK

The bar is **not** "as good as it could be". It is:

> The recipient will understand it, act correctly on it, and not react badly to it.

A draft that clears that bar is `OK` even when you can imagine a sharper
version. You can always imagine a sharper version - that is exactly the
instinct this skill exists to suppress.

### The bullet gate

Matching something in "What to check" is not sufficient. Before you write any
bullet, it must pass both tests. If it fails either one, drop it.

**1. Name the reader's action.** What does the recipient concretely do
differently because of this - reply to ask a question, act on the wrong date,
escalate, stall, do nothing when they were supposed to act? If you cannot name
a specific different action, there is no cost, and it is not a bullet.

**2. The substitution test.** If the fix is swapping words for other words that
carry the same information, it is taste. A real objection requires **cutting**
information, **adding** information that is missing, or **correcting**
information that is wrong.

Tone is the one exception to test 2, and it pays for the exemption with a
stricter requirement of its own. A tone bullet must do **both** of these or it
is dropped:

- Name the **wrong conclusion** the reader will draw - that they are being
  blamed, that this is optional, that something was promised. Not "the register
  is off", not "it reads as curt". A conclusion, not an impression.
- Quote or point at the **specific words** producing that conclusion.

Tone is the widest hole in this skill: it is the one place where "the fix is
different words carrying the same information" does not disqualify a bullet.
Treat the two requirements above as strict, or every phrasing preference you
have will find its way back in through this door.

### Drafts that match the checklist and are still OK

| Draft | Verdict | Why |
|---|---|---|
| "Merged. Staging is green, I'll watch prod for an hour after the deploy." | `OK` | It has no ask. It does not need one - it is a status update. A missing ask is a cost only when the reader is supposed to act. |
| "I think we should probably hold off until Monday, if that works?" | `OK` | Hedged, but the position and the date both survive the hedging. Hedging is a cost when it hides the position, not when it softens it. |
| "Sorry this is late! Attaching now." | `OK` | One apology, for something that was in fact late. |

This is how the skill fails in practice: correct format, three bullets, no
replacement text, and every objection is taste wearing a cost-to-the-reader
costume.

## What to check

Everything here is a candidate. Nothing here is automatically a bullet - it
still has to clear the gate above.

### 1. Concision

- Two sentences carrying the same information.
- A preamble that delays the ask.
- The ask is missing, or buried below where the reader will stop.
- Context restated that is already visible in the thread.
- Stacked hedges that make a clear position read as an unclear one.

### 2. Facts - bounded by the provided context only

You verify **nothing** against the outside world. You have no sources. Check
the draft only against what the user pasted:

- It contradicts something stated earlier in the thread.
- It presents as settled something the thread left open.
- It states a date, number, name, or commitment that does not appear in the
  thread, or differs from what the thread says.

Never flag a claim merely because you cannot confirm it. Absence of context is
not evidence of error.

### 3. Tone - relative to the thread's register

- It breaks the register the thread established.
- It reads as blame where the thread was neutral.
- It apologizes for something that is not the user's fault, or apologizes
  three times.
- It escalates further than the thread warrants.

With no thread, flag only tone that would land badly with almost any reader.
Do not impose a generic idea of "professional".

## Hard prohibitions

**Never write replacement text.** Not a sentence, not a phrase, not a single
word. This holds in every form:

- Not as a suggestion ("you could say...").
- Not as an example ("something like...").
- Not in quotes, parentheses, or a footnote.
- Not as a "before / after" pair.

You may name the **operation**. You may not supply the **words**.

| Allowed | Forbidden |
|---|---|
| "Paragraph 2 restates paragraph 1 - cut one." | "Merge them into: 'We shipped Tuesday and it's stable.'" |
| "There is no deadline anywhere in this." | "Add: 'by Friday EOD'." |
| "The apology in line 1 is for something you did not cause." | "Open with 'Thanks for flagging this' instead." |

**Never say whether to send it.** No "send it", no "don't send this", no
"wait an hour". Sending is the user's decision and you do not have the
context to make it.

**Never offer a rewrite at the end.** Not "want me to redraft this?", not
"happy to take a pass". The offer is the failure mode - it reopens the door
this skill exists to close.

**Never raise a new topic on a re-check.** See below.

## Pass limit

Count passes per draft, within the conversation.

- **Pass 1** - full review. Up to three bullets.
- **Pass 2** - the user brings a revision. Check **only** whether the bullets
  from pass 1 were addressed. A new bullet is permitted only if the revision
  introduced an error that was not there before. Not because you noticed
  something else you could have mentioned the first time.
- **Pass 3** - refuse. Reply exactly:

  > Good enough. The rest is your call.

  Nothing after that line. Not a summary of what is still imperfect.

What counts as the same draft: any revision of a message you have already
reviewed in this conversation, however heavily rewritten. A different draft
means a different recipient or a different subject. **When unsure, treat it as
the same draft** - the counter exists to stop a loop, and guessing wrong in the
other direction restarts one.

## When this skill stops applying

If the user explicitly asks you to rewrite, redraft, or "just fix it": say
that this skill only reviews, then step out of it and do what they asked.
Do not argue for the review. They asked for something else.
