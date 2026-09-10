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

Most agents read skills straight off the filesystem, and one command covers
all of them - [`npx skills`](https://github.com/vercel-labs/skills) supports
Claude Code, Cursor, the ChatGPT desktop app, Codex, Copilot and ~75 others:

```bash
npx skills add mihajloS/review-message
```

Add `-g` to install it for every project instead of the current one, and run
`npx skills update` later to pull a newer version.

Manual paths, if you would rather clone: `~/.claude/skills/review-message`
for Claude Code, `.agents/skills/review-message` in a project or
`~/.agents/skills/review-message` globally for Cursor, Codex and VS Code.

### Claude Desktop

Chat in Claude Desktop has no filesystem access, so the skill has to be
uploaded to your account. Claude Code inside the same app is separate and is
already covered by the command above.

1. Install the skill, which is also what puts the file on disk for you to
   pick in step 4:

   ```bash
   npx skills add mihajloS/review-message
   ```

2. Open Claude Desktop → **Settings** → **Skills**, under *Customize* in the
   sidebar - below Capabilities and Claude Code, not inside them.
3. Click **Add** → **Upload skill**.
4. Select `SKILL.md` from `~/.agents/skills/review-message`. Press `⌘⇧.` in
   the file dialog to reveal the hidden `.agents` folder.
5. Start a **new chat** and invoke it with `/review-message`, followed by
   your draft.


https://github.com/user-attachments/assets/bdab060e-7ee4-4ec2-b74d-e97db2723826


### ChatGPT

The desktop app reads `~/.agents/skills`, so the command above already covers
it - nothing to upload. Only chatgpt.com in a browser needs a manual upload:
download this repo, zip it, then Skills → Create → Upload. Skills in ChatGPT
are currently limited to Business, Enterprise, Healthcare and Edu plans.

## Usage

**This skill never fires on its own.** You invoke it deliberately or it stays
out of the way - `/review-message` in Claude Code, `$review-message` in Codex
and ChatGPT, `@review-message` in Cursor. A skill that jumps in every time a
draft appears in the conversation is the thing it was built to avoid.

The thread above your draft is optional, but it is what makes the fact and tone
checks real:

```
/review-message

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

Explicit-only invocation has to be declared twice, because no single field
covers both families:

- `disable-model-invocation: true` in `SKILL.md` - Claude Code, Cursor, Pi.
  It is not one of the six fields in the neutral spec, so `skills-ref validate`
  reports it as unexpected. Nothing in the install path enforces that, and
  runtimes that do not know the field ignore it.
- `policy.allow_implicit_invocation: false` in `agents/openai.yaml` - Codex and
  ChatGPT, which do not read the frontmatter field
  ([openai/codex#10585](https://github.com/openai/codex/issues/10585)).

The description carries the same instruction in plain language, which is the
only brake left on runtimes that honour neither.

## License

MIT
