# review-message

**An agent skill that judges your draft instead of rewriting it.**

Ask any assistant to look at a message before you send it and you get a better
message back. You send that one. You learned nothing, and it does not sound
like you.

`review-message` gives you a verdict and at most three objections, each with the
reason it costs the reader something. It never writes a replacement sentence,
never tells you whether to hit send, and refuses to review the same draft more
than twice.

## What it does

```
**NOT YET**

- Paragraph 2 restates paragraph 1 - the reader pays twice for one idea.
- You commit to Friday; the thread never established that date - if it is an
  estimate it needs to read as one.
- The apology in the opening is for a delay that was not yours.
```

and, more often than you would expect:

```
**OK**
```

## The three constraints

| Constraint | Why |
|---|---|
| **Never writes replacement text** | Naming the operation ("cut one of these") teaches. Supplying the words does the work for you. |
| **Three bullets maximum, and three is a ceiling** | An unbounded critique of any text is infinite. A bounded one forces the model to rank. |
| **Two passes, then it refuses** | Pass three is where "helpful" turns into an endless polish loop. It stops with *"Good enough. The rest is your call."* |

It also does not tell you to send or not send. That is your call and it does
not have the context to make it.

## What it checks

- **Concision** - repetition, preamble that delays the ask, the ask buried too
  low, context the reader already has.
- **Facts** - **only against the context you paste.** It verifies nothing
  against the outside world. Paste the thread with your unsent reply at the
  bottom and it will catch where your draft contradicts what was already said,
  or commits to a date or number that appears nowhere above it. With no thread,
  it says so rather than inventing a check.
- **Tone** - relative to the register the thread already established, not
  against a generic idea of "professional".

## Install

**Any supported agent** ([`npx skills`](https://github.com/vercel-labs/skills)
covers Claude Code, Cursor, Codex, Copilot and ~75 others):

```bash
npx skills add mihajloS/review-message
```

**Claude Code**, manually:

```bash
git clone https://github.com/mihajloS/review-message ~/.claude/skills/review-message
```

**Cursor / Codex / VS Code**, manually - clone into `.agents/skills/review-message`
in your project, or `~/.agents/skills/review-message` for all projects.

**ChatGPT** - there is no install-from-git in the ChatGPT app. Download this
repo, zip it, and upload it under Skills → Create → Upload. Skills in ChatGPT
are currently limited to Business, Enterprise, Healthcare and Edu plans.

## Usage

Paste your draft and ask. The thread above it is optional but makes the fact
and tone checks real:

```
review this before I send it

--- thread ---
Ana: can you get the migration notes over before the review?
Ana: also we agreed to hold the rollout until QA signs off

--- my draft ---
Sorry for the delay! Notes attached. I'll go ahead and start the rollout
Thursday since QA is basically done.
```

If you want a rewrite, ask for one directly - the skill steps aside rather
than lecturing you about it.

## Format

A single [Agent Skills](https://agentskills.io) `SKILL.md` - the open standard
originally developed by Anthropic and now maintained as a vendor-neutral spec,
read by Claude Code, ChatGPT, Codex, Cursor, Copilot and others.

## License

MIT
