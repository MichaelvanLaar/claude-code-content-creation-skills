---
name: cc-content-onboarding
description: >
  Use this skill when setting up a new marketing project with Claude Code, populating
  company-level context files, or initializing the .claude/context/ folder.
  Invoke when the user asks to "onboard a new client", "set up context files",
  "create company profile", "populate .claude/context", "initialize marketing project",
  or "set up brand context". Also use when context files are missing and the user
  wants to create them through an interview.
allowed-tools: Read, Write, Edit, Bash
argument-hint: "[optional: context category to regenerate, e.g. writing-style]"
---

# Onboarding Skill

You are setting up the company-level context for a marketing project. Your goal is to
populate `.claude/context/` with structured Markdown files that skills will load via
the project's CLAUDE.md hierarchy — and to wire all those files into CLAUDE.md with
the appropriate @-imports.

## Step 1: Discover existing context

Before asking any questions, discover what context files already exist and classify
them by category.

Run:

```bash
grep -s '^@' CLAUDE.md 2>/dev/null || echo "(no CLAUDE.md or no @-imports)"
find .claude/context/ -type f -name "*.md" 2>/dev/null | sort || echo "(no .claude/context/)"
```

Read each discovered file. Classify it into one of these categories based on its
**content** (not its filename):

| Category                    | What it covers                                              |
| --------------------------- | ----------------------------------------------------------- |
| **content-defaults**        | Default output language and other project-level defaults    |
| **writing-style**           | Tone, vocabulary, phrases to use/avoid, sentence patterns   |
| **organization-identity**   | What the company does, products, mission, positioning       |
| **target-audience**         | Personas, goals, challenges, preferred channels             |
| **storytelling-frameworks** | Named frameworks: PAS, StoryBrand, Cialdini, etc.           |
| **reference-materials**     | Books, research, or external sources informing the approach |
| **reference-samples**       | Curated past content examples with quality annotations      |

Present the coverage table to the owner:

```
Context coverage
─────────────────────────────────────────────
Category                Status    File(s)
─────────────────────────────────────────────
writing-style           ✓ present  <path>
organization-identity   ✗ missing  —
target-audience         ✗ missing  —
storytelling-frameworks ✗ missing  —
reference-materials     ✗ missing  —
reference-samples       ✗ missing  —
─────────────────────────────────────────────
```

Omit categories that are both missing and clearly not applicable to the project type.
If `$ARGUMENTS` names a specific category to regenerate, treat that category as
missing regardless of whether a file already exists for it.

## Step 2: Handle required categories

For each of these categories that is **missing**: `writing-style`, `organization-identity`

Ask:

> "The **[category]** category is missing. Would you like to:
> (a) Create it now through a short interview
> (b) You already have a file for this — tell me the path
> (c) Skip for now"

**Option (a) — Create via interview:**

First ask: "Where should I save this file? (Suggested: `.claude/context/<name>.md` —
or enter any path like `brand/voice.md` and I'll create it at `.claude/context/brand/voice.md`)"

Then conduct the interview. Ask questions one at a time and wait for each answer.
If the owner skips a question, use `TODO: [question text]` as a placeholder.

**writing-style interview:**

1. "How would you describe the brand's tone? (e.g., authoritative, conversational, witty, warm)"
2. "What words or phrases does the brand use frequently?"
3. "What words or phrases should the brand never use?"
4. "Is there a specific reading level or vocabulary level to target?"
5. "Any punctuation, capitalization, or formatting rules? (e.g., no exclamation marks, em-dash preferred)"

Write the file:

```markdown
# Writing Style

## Tone

<answer>

## Signature phrases & vocabulary

<answer>

## Words & phrases to avoid

<answer>

## Reading level

<answer>

## Formatting rules

<answer>
```

**organization-identity interview:**

1. "What is the company's name and one-sentence description of what it does?"
2. "What are the main products or services? (List them briefly.)"
3. "What is the company's mission or core purpose?"
4. "What makes this company different from competitors? (Key differentiators.)"
5. "Are there any values or principles the company is known for?"

Write the file:

```markdown
# Organization Identity

## Name & Description

<answer>

## Products & Services

<answer>

## Mission

<answer>

## Differentiators

<answer>

## Values

<answer>
```

After writing, confirm: "✓ Created `.claude/context/<path>`."

**Option (b) — Existing file:**

Ask for the path. Read the file and verify it covers the category.

- If it covers the category: note it as present and continue.
- If it does not: say so and offer options (a), (b), (c) again.

**Option (c) — Skip:**

Note it as intentionally absent and continue.

## Step 3: Set content defaults (unconditional)

This step runs regardless of whether a `content-defaults` file already exists.

Check whether a `content-defaults` file is present:

```bash
find .claude/context/ -type f -name "*.md" 2>/dev/null | xargs grep -l "Output language" 2>/dev/null | head -1
```

**If a file is found:** read it and show its current contents:

> "I found an existing content-defaults file at `<path>`:
>
> ```
> <file contents>
> ```
>
> Would you like to update it? (yes / no)"

- **No**: keep the existing file and proceed to Step 4.
- **Yes**: proceed with the interview below and overwrite the file.

**If no file is found:** proceed with the interview.

Ask: "Where should I save the content defaults file?
(Suggested: `.claude/context/content-defaults.md`)"

Then ask:

> "What is the default output language for all content created in this project?
> (Examples: German / de-DE, English / en-US, French / fr-FR)"

Write the file:

```markdown
# Content Defaults

**Output language:** <answer>
```

After writing, confirm: "✓ Created `.claude/context/<path>`."

## Step 4: Handle recommended category

For the `target-audience` category, if missing:

Ask:

> "The **target-audience** category is missing. This helps skills tailor content to
> the right reader. Would you like to:
> (a) Create it through a short interview
> (b) You have a file for this — tell me the path
> (c) Skip — skills will generate without specific audience context"

**Option (a) — Create via interview:**

Ask: "Where should I save this file? (Suggested: `.claude/context/target-audience.md`)"

Then ask:

1. "How many distinct audience segments does this company target? (1–4 recommended.)"

For each segment, ask:

- "What is this persona's role or title?"
- "What are their primary goals or motivations?"
- "What are their biggest challenges or frustrations?"
- "What objections do they typically raise?"
- "What content formats or channels do they prefer?"

Write the file:

```markdown
# Target Audience

## Persona: <role/title>

**Goals:** <answer>
**Challenges:** <answer>
**Objections:** <answer>
**Preferred channels:** <answer>
```

After writing, confirm: "✓ Created `.claude/context/<path>`."

Handle Options (b) and (c) the same as in Step 2.

## Step 5: Handle optional categories

Ask once, listing both at the same time to minimize questions:

> "Would you like to add any of these optional context files?
>
> - **storytelling-frameworks** — named frameworks you use (PAS, StoryBrand, Cialdini, etc.)
> - **reference-materials** — books or research that inform your content approach
>
> For each: (a) create via quick interview, (b) point to an existing file, or (c) skip.
> Or say 'none' to skip all."

**storytelling-frameworks (if creating):**

Ask: "Where should I save this file? (Suggested: `.claude/context/storytelling-frameworks.md`)"

Then: "Which frameworks or persuasion principles do you use? List them and I'll
document their structure."

Write the file using the owner's descriptions.

**reference-materials (if creating):**

Ask: "Where should I save this file? (Suggested: `.claude/context/reference-materials.md`)"

Then: "What books, research, or external sources inform your content approach? For
each, give the title and a brief note on what it contributes."

Write the file using the owner's descriptions.

Note: `reference-samples` is managed by the `cc-content:cc-content-samples-curation` skill. Do not create
it here.

## Step 6: Wire files into CLAUDE.md

Collect all context files that were created or confirmed in Steps 2–4.

Check the current CLAUDE.md state:

```bash
ls CLAUDE.md 2>/dev/null && echo "exists" || echo "missing"
grep -s '^@' CLAUDE.md 2>/dev/null
```

For each confirmed context file, determine whether an @-import already exists in
CLAUDE.md. Use the file's content (category) to pick the right trigger phrase:

| Category                | @-import trigger                                                  |
| ----------------------- | ----------------------------------------------------------------- |
| content-defaults        | `**Read when:** producing any content for this project`           |
| writing-style           | `**Read when:** producing any written content`                    |
| organization-identity   | `**Read when:** working on any deliverable for this company`      |
| target-audience         | `**Read when:** targeting content at a specific audience segment` |
| storytelling-frameworks | `**Read when:** structuring persuasive content`                   |
| reference-materials     | `**Read when:** drawing on reference sources for this project`    |

**If CLAUDE.md is missing:**

Create it with a header using the company name from `organization-identity` (or
`[Company Name]` if that file is also absent):

```markdown
# [Company Name]
```

Then propose adding @-imports for all confirmed context files:

> "I'll add these @-imports to your CLAUDE.md:
>
> ```
> @<path> **Read when:** <trigger>
> @<path> **Read when:** <trigger>
> ```
>
> Shall I proceed? (yes / edit / skip)"

- **Yes**: write the @-imports to CLAUDE.md. Confirm: "✓ CLAUDE.md updated."
- **Edit**: let the owner adjust paths or trigger phrases, then apply.
- **Skip**: note "CLAUDE.md not updated — add @-imports manually when ready."

**If CLAUDE.md exists:**

Show only the @-imports that are missing (already-present ones need no action). Ask
for confirmation before adding.

## Step 7: Summary

Print a final summary:

```
Onboarding complete
──────────────────────────────────────────────────────
content-defaults        ✓ .claude/context/<path>
writing-style           ✓ .claude/context/<path>
organization-identity   ✗ skipped intentionally
target-audience         ✓ .claude/context/<path>
storytelling-frameworks ✗ skipped
reference-materials     ✗ skipped
CLAUDE.md               ✓ updated (or: ✓ created / ✗ skipped)
──────────────────────────────────────────────────────
```

Then say:

> "Your marketing project context is ready. Skills like `cc-content:cc-content-linkedin-post` will
> automatically load these files via your CLAUDE.md when invoked."
