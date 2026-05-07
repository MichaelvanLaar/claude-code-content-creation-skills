---
name: cc-content-session-wrap
description: >
  Use this skill at the end of a content creation session to review deliverables,
  collect feedback, detect recurring correction patterns, and create a git commit.
  Invoke when the user says "wrap up", "end session", "session wrap", "wrap session",
  "commit session work", or "close out this session".
allowed-tools: Read, Write, Edit, Bash
---

# Session Wrap Skill

You are closing out a content creation session. The goals are: (1) review what
was produced, (2) collect corrections for any skills used, (3) surface recurring
patterns from past learnings, (4) promote patterns if warranted, and (5) commit
all session work with a clean, well-labelled message.

## Step 1: Review deliverables

Run git status to understand what changed during the session:

```bash
git status --short
```

If git status reports **no changes**:

> "No changes detected in git. Would you still like to collect feedback and
> check for correction patterns? (yes / no)"

If the owner says no, skip to Step 5. If yes, continue but skip the file summary.

If changes **exist**, group them into four categories and present them:

```
─────────────────────────────────────────────
Session deliverables
─────────────────────────────────────────────
Context files (.claude/context/):
  <list of modified/untracked files in .claude/context/>

Deliverables (content output files):
  <list of modified/untracked files that are not in .claude/ or skill folders>

Skill files (.claude/skills/):
  <list of modified/untracked files in .claude/skills/>

Other:
  <any remaining files>
─────────────────────────────────────────────
```

Omit any empty categories. If all changes are in a single category, still use
the labelled format so the owner sees what type of work was done.

## Step 2: Identify skills used

Ask:

> "Which output-format skills did you use during this session?
> (Examples: cc-content:cc-content-linkedin-post, other skills you may have invoked.
> You can list multiple, or say 'none'.)"

Wait for the response.

- If the owner says **none** or gives an empty response: skip Step 3 and proceed
  to Step 4.
- If the owner lists one or more skills: proceed to Step 3.

## Step 3: Collect feedback per skill

For each skill the owner listed, ask:

> "How did **`<skill-name>`** perform? Did the output meet expectations?
> If you have a correction, describe it — or press Enter to skip."

- If the owner **provides a correction**: record it for the learnings step.
- If the owner **skips**: note "no correction for `<skill-name>`" and move on.

After collecting feedback for all listed skills, proceed to Step 4.

## Step 4: Append to learnings

For each correction collected in Step 3, append a tagged, dated entry to
`.claude/learnings.md`.

First, check whether the file exists:

```bash
ls .claude/learnings.md 2>/dev/null && echo "exists" || echo "missing"
```

If **missing**, create it with a header:

```markdown
# Learnings

Corrections and feedback collected during content sessions.
Entries are tagged by skill and dated.

---
```

Then append each correction as:

```
[<skill-name>] <correction summary> — <YYYY-MM-DD>
```

Use today's date (available from the session context: `currentDate`).

After all entries are appended, confirm:

> "✓ <N> learning(s) saved to `.claude/learnings.md`."

If no corrections were collected, skip this step silently.

## Step 5: Detect recurring patterns

Read `.claude/learnings.md` (if it exists):

```bash
cat .claude/learnings.md 2>/dev/null
```

Parse all entries that follow the `[<skill-name>] <text> — <date>` format.
Group them by skill tag. For each skill tag, look for three or more entries that
express **semantically similar** corrections — same underlying concern, even if
worded differently.

Examples of semantically similar corrections:

- "hooks are too long" / "opening line rambles" / "first two lines need to be
  punchier" → all about hook brevity/impact

If **no skill tag has three or more similar entries**: say "No recurring patterns
detected." and proceed to Step 6.

If **one or more patterns are detected**: present each group:

```
─────────────────────────────────────────────
Recurring pattern detected: <skill-name>
─────────────────────────────────────────────
The following corrections appear related:
  • [<date>] <correction 1>
  • [<date>] <correction 2>
  • [<date>] <correction 3>
─────────────────────────────────────────────
```

For each group, ask:

> "This pattern has come up <N> times. Would you like to:
> (a) Add a rule to `<skill-name>`'s format guidelines
> (b) Add a rule to the project `CLAUDE.md`
> (c) Dismiss — leave it in learnings for now"

Handle responses:

**Option (a) — format guidelines**:

Ask: "How should I phrase the rule? (Or describe the correction and I'll draft it.)"

Determine the skill's format guidelines path. For `cc-content:cc-content-linkedin-post`,
the format guidelines file is at `${CLAUDE_SKILL_DIR}/../cc-content-linkedin-post/format-guidelines.md`
(relative to this skill's directory). For other skills, look for a `format-guidelines.md`
inside the skill's directory.

Read the file, then append the new rule under an appropriate existing section or
create a new `## Promoted Rules` section at the bottom.

Confirm: "✓ Rule added to `<path>`."

**Option (b) — CLAUDE.md**:

Ask: "How should I phrase the rule in `CLAUDE.md`? (Or describe it and I'll draft it.)"

Read `CLAUDE.md`, locate the `## Learnings` section, and append the rule there.
If no `## Learnings` section exists, add it before appending.

Confirm: "✓ Rule added to `CLAUDE.md`."

**Option (c) — dismiss**:

Say: "Pattern noted but not promoted. It will remain in `.claude/learnings.md`
for future reference."

Process all detected patterns before moving on.

## Step 6: Commit

Check for files to commit:

```bash
git status --short
```

If nothing to commit, say: "Nothing to commit — session is clean." and close with
the summary in Step 7.

If there are changes, stage them explicitly. Do NOT use `git add -A` or
`git add .`. Instead, list each file that should be committed:

Ask:

> "I'll stage the following files for this commit:
> <list of modified/untracked files from the session, excluding any .env or secrets/ patterns>
>
> Shall I proceed? (yes / no / edit the list)"

- **Yes**: stage each file individually, then continue.
- **No**: skip the commit step and close with Step 7.
- **Edit**: ask which files to add or remove, update the list, show it again.

After staging, propose a commit message following Conventional Commits with gitmoji.

Use the session changes to determine the type:

| Change type                             | Conventional Commits prefix | Gitmoji |
| --------------------------------------- | --------------------------- | ------- |
| New content deliverables                | `feat`                      | ✨      |
| Context file updates (brand voice, etc) | `feat` or `docs`            | 📝      |
| Skill files added or updated            | `feat`                      | ✨      |
| Learnings / corrections only            | `docs`                      | 📝      |
| Mixed session                           | `feat`                      | ✨      |
| Bug fix or correction to existing file  | `fix`                       | 🐛      |

Present the proposed message:

```
─────────────────────────────────────────────
Proposed commit message
─────────────────────────────────────────────
<gitmoji> <type>: <short description>

<optional body with bullet points if multiple distinct changes>
─────────────────────────────────────────────
```

Ask: "Shall I commit with this message? (yes / edit / skip)"

- **Yes**: run the commit.
- **Edit**: ask for the owner's preferred message, then commit with that.
- **Skip**: say "Commit skipped. Files are staged but not committed."

After a successful commit, show the commit hash:

> "✓ Committed: `<hash>` — <first line of commit message>"

## Step 7: Session summary

Close with a brief summary:

```
─────────────────────────────────────────────
Session complete
─────────────────────────────────────────────
Deliverables reviewed:   <N files>
Learnings logged:        <N entries (or "none")>
Patterns promoted:       <N (or "none")>
Committed:               <hash> (or "not committed")
─────────────────────────────────────────────
```

Then say:

> "Session wrapped. Run `/cc-content:cc-content-session-wrap` again at the end of your next session."
