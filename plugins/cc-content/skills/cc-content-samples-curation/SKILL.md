---
name: cc-content-samples-curation
description: >
  Use this skill when the owner wants to save a gold-standard content example,
  curate approved samples, add a reference post to the samples library, or
  annotate a piece of content for future reference. Invoke when the user says
  "save this as a sample", "add to samples", "curate an example", "record this
  good post", or "update samples.md".
allowed-tools: Read, Write, Bash
---

# Samples Curation Skill

You are helping the owner capture and annotate gold-standard content examples.
These examples are saved to `.claude/context/samples.md` and used by output-format
skills as high-signal reference material for on-brand content generation.

## Step 1: Identify the content

Ask the owner:

> "Please share the content you'd like to save as a gold-standard example.
> You can either paste it here, or give me a file path and I'll read it."

Wait for the response.

- If the owner **pastes content**, acknowledge it and proceed to Step 2.
- If the owner **provides a file path**, read the file:

  ```bash
  cat "<path>"
  ```

  Confirm you've read it, then proceed to Step 2.

## Step 2: Annotation interview

Walk through these questions one at a time:

1. **Output type** — "What type of content is this? (e.g., LinkedIn post, blog article
   intro, email subject line, whitepaper section)"

2. **Key qualities** — "What makes this example good? Describe the qualities you want
   future content to replicate. (You can skip this if you prefer.)"

3. **Caveats** — "Any caveats or context? For example: 'works for B2B, not B2C', or
   'written for a product launch, tone may be more urgent than usual'. (Optional.)"

If the owner skips a question, omit that field from the entry — do not add placeholders.

## Step 3: Preview and confirm

Present a formatted preview of the entry before saving:

```
─────────────────────────────────────────────
Sample entry preview
─────────────────────────────────────────────
Output type: <type>
Key qualities: <qualities>          ← omitted if skipped
Caveats: <caveats>                  ← omitted if skipped

Content:
<content>
─────────────────────────────────────────────
```

Ask: "Shall I save this entry to `.claude/context/samples.md`? (yes / no / edit)"

- **Yes** → proceed to Step 4.
- **No / cancel** → say "Entry discarded. Nothing was written." Then go to Step 5.
- **Edit** → ask what to change, apply the edit, show the preview again.

## Step 4: Append to samples.md

Check whether `.claude/context/samples.md` exists:

```bash
ls .claude/context/samples.md 2>/dev/null && echo "exists" || echo "missing"
```

**If missing**, create it with a header first:

```markdown
# Content Samples

Gold-standard content examples curated by the owner.
Skills reference this file to produce on-brand output.

---
```

Then append the entry in this format:

```markdown
## Sample: <output-type> — <YYYY-MM-DD>

**Output type:** <type>
**Key qualities:** <qualities>
**Caveats:** <caveats>
```

<content>
```

---

```

Omit `**Key qualities:**` and `**Caveats:**` lines if the owner skipped those fields.

After writing, confirm:
> "✓ Saved to `.claude/context/samples.md`."

Count the total `## Sample:` entries in the file and report:
> "The file now contains <N> sample(s)."

## Step 5: Loop

Ask: "Would you like to curate another example? (yes / no)"

- **Yes** → return to Step 1.
- **No** → say "Samples curation complete. You can invoke this skill again any time to add more."
```
