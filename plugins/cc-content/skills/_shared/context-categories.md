# Context Categories

Dev reference for skill authors in this repository. **Not deployed to target projects.
Not @-imported by any skill.** Skills embed the relevant subset of this taxonomy
directly in their runtime assessment steps.

---

## Categories

### 1. `writing-style` — Required for content output

What it covers: Tone, vocabulary, phrases to use or avoid, sentence patterns, reading
level, formality, punctuation preferences.

Recognition signals: Tone descriptors ("authoritative", "conversational"), vocabulary
lists, "words to avoid" sections, examples of preferred phrasing, style rules.

Common file names in target projects:
`brand-voice.md`, `writing-style.md`, `copy-guidelines.md`, `Schreibstil.md`,
`tone-of-voice.md`, `editorial-guidelines.md`, any file describing how to write

---

### 2. `organization-identity` — Required for content output

What it covers: What the company does, products or services, mission, differentiators,
values, and market positioning.

Recognition signals: Company name and description, product/service list, mission
statement, "what makes us different", competitive positioning notes.

Common file names in target projects:
`company-profile.md`, `about-us.md`, `Unternehmensprofil.md`, `brand-platform.md`,
`positioning.md`

---

### 3. `target-audience` — Recommended for content output

What it covers: Who the content is for — their role, goals, challenges, objections,
preferred channels, and what motivates them to act.

Recognition signals: Persona names or roles, pain point descriptions, goals and
motivations, objection handling notes, ICP (ideal customer profile) data.

Common file names in target projects:
`buyer-personas.md`, `target-audience.md`, `Zielgruppen.md`, `icp.md`,
`customer-profiles.md`, `audience.md`

---

### 4. `storytelling-frameworks` — Optional

What it covers: Named persuasion or narrative structures used in the company's
content: Problem–Agitate–Solve, StoryBrand, Hero's Journey, Cialdini's principles.

Recognition signals: Framework names (PAS, StoryBrand, etc.), structural templates
showing problem→agitate→solve, lists of persuasion or influence principles.

Common file names in target projects:
`storytelling-frameworks.md`, `messaging-framework.md`, `narrative-structure.md`,
`persuasion-principles.md`

Skill authoring note: Two universal references live in `.claude/skills/_shared/`
and should be @-imported by any output skill rather than inlined or duplicated:

- `storytelling-frameworks.md` — catalog of 62 narrative structures with a
  selection process. Use for content long enough to carry a narrative arc
  (~600+ characters).
- `persuasion-principles.md` — distilled operational guidance on Cialdini's
  seven principles of influence (reciprocity, commitment & consistency, social
  proof, liking, authority, scarcity, unity) plus pre-suasion. Use whenever the
  output has persuasive intent — i.e., asks the reader to take an action.

Frameworks shape *structure*; persuasion principles shape *psychological
levers*. Strong content uses both — pick a framework for the spine, then layer
1–3 principles into the copy.

---

### 5. `reference-materials` — Optional

What it covers: Books, research, or external references that inform the content
approach — source material that shapes thinking, not operational frameworks.

Recognition signals: Book titles, author names, key concepts or quotes from external
sources, research findings, influences.

Common file names in target projects:
`presuasion-book.md`, `reference-materials.md`, `Quellen.md`, `influences.md`,
`<book-title>.md` (named after the source)

---

### 6. `reference-samples` — Optional

What it covers: Curated examples of high-quality past content. Managed by the
`samples-curation` skill; stored at `.claude/context/samples.md` by convention.

Recognition signals: Lists of past posts or copy with annotations like "what worked",
quality ratings, or "use as reference" notes with a consistent per-entry structure.

Common file names in target projects:
`samples.md`, `examples.md`, `gold-standard.md`

---

### 7. `campaign-brief` — Optional / session-scoped

What it covers: Goals, key messages, constraints, and audience for a specific
initiative. Applies to one campaign only — distinct from company-level context.

Recognition signals: Campaign name, launch date, key messages, target KPIs,
content restrictions, CTA requirements.

Common file names in target projects:
`brief.md`, `campaign-brief.md`, `<campaign-name>-brief.md`

Note: Skills detect this automatically via `$ARGUMENTS` or by checking for `brief.md`
in the current working directory. Do not include it in the context assessment table
shown to users — handle it in a dedicated "check for campaign briefing" step.

---

### 8. `content-defaults` — Always created by onboarding

What it covers: Project-level output defaults that apply to every deliverable — primarily
the default output language, but potentially also date format, number format, or currency.
Kept separate from `writing-style` so that it remains a writable Markdown file even when
the writing styleguide is delivered as a non-editable PDF.

Recognition signals: Key-value pairs such as "Output language: German (de-DE)", language
codes, locale strings.

Common file names in target projects:
`content-defaults.md`, `output-defaults.md`, `project-defaults.md`

---

## How to use this taxonomy when authoring a skill

1. Identify which categories the skill needs (Required, Recommended, Optional).
2. Embed a context assessment table directly in the skill's first step — do not
   @-import this file.
3. Include this instruction in that step:

   > "Review your current context window and assess whether it covers these categories.
   > Match on meaning, not filename — a file called `Schreibstil.md` satisfies
   > 'writing style' just as well as `brand-voice.md`."

4. For Required categories missing from context:
   - Ask once: "I don't see any **[category]** information in the loaded context. Is
     this intentional, or should I pause while you add it to your CLAUDE.md?"
   - If intentional: note it, continue, label final output `⚠ DEGRADED OUTPUT`
   - If not intentional: pause and let owner add the file to their CLAUDE.md

5. For Recommended categories: note absence silently — do not ask.
6. For Optional categories: use if present, ignore if absent.
7. Never @-import project context files from within the skill — those are loaded
   exclusively via the target project's CLAUDE.md hierarchy.
