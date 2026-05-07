# Content Skill Research Brief Template

Dev reference for skill authors in this repository. **Not deployed to target projects.
Not @-imported by output skills.** Used by the `new-content-skill` skill to guide
research and check coverage before synthesizing a `format-guidelines.md`.

---

## How to use this template

1. Replace `[FORMAT]` with the content format you are building a skill for
   (e.g., "B2B blog article", "marketing email", "whitepaper", "video script").
2. Run each research pass in your preferred AI tool. Recommendations:
   - **Perplexity** — best for citation-backed factual research (passes 1, 2)
   - **Claude.ai or ChatGPT** — best for framework synthesis and nuanced analysis (passes 3–6)
   - Any tool with long context — run the combined mega-prompt for a single pass
3. Save each response as a `.md` file in `.claude/skill-drafts/<skill-name>/`
   (one file per pass, or a single combined file — the skill reads all `.md` files
   in that folder).
4. When research is complete (or complete enough), run `/new-content-skill <format-name>`
   in Claude Code. The skill checks coverage, flags any gaps, and synthesizes the files.

---

## Research Pass 1 — Universal Structure

Covers: **Layer 1** of `format-guidelines.md` (universal best practices, regardless of
audience or goal).

```
You are a marketing research expert. Provide a comprehensive, structured overview of
universal best practices for [FORMAT] content. Cover:

- Optimal length ranges (characters, words, or pages) and when to deviate
- Recommended structural sections and their purpose (e.g., hook, body, CTA)
- Opening / hook principles: what makes a strong opening, common high-performing patterns
- Closing / CTA principles: what CTAs work best, placement, phrasing approaches
- Formatting conventions: use of headers, bullets, white space, visual breaks
- Reading patterns: how readers scan or consume this format (e.g., F-pattern, Z-pattern)
- Publishing and distribution factors that affect format choices (e.g., preview text for
  emails, above-the-fold for web articles)
- What consistently separates high-performing from low-performing [FORMAT] content

Cite established frameworks (AIDA, PAS, StoryBrand, etc.) where applicable. Focus on
evidence-based practices. This research will be used as a brand-neutral baseline —
no industry-specific examples needed.
```

---

## Research Pass 2 — B2B vs. B2C Differences

Covers: **Layer 2** of `format-guidelines.md` (audience-specific variations), B2B/B2C axis.

```
You are a marketing research expert. Explain how best practices for [FORMAT] content
differ significantly between B2B and B2C contexts. For each difference, include the
underlying psychological or behavioral reason — not just what differs, but why.

Cover:
- Optimal length (which context prefers longer or shorter, and why)
- Tone and formality level
- Evidence and proof types (data, ROI, case studies vs. testimonials, social proof, emotion)
- Decision-maker psychology:
    B2B: buying committee, multi-stakeholder, risk aversion, ROI justification
    B2C: individual, immediate gratification, emotional triggers, identity signaling
- Buying cycle length and how it affects content depth and nurturing approach
- What constitutes a strong CTA in each context
- Typical reading context (when/where/how people read this format in each context)
- Skimming vs. deep reading behavior differences

Where B2B itself splits further (e.g., technical buyer vs. executive sponsor, SMB vs.
enterprise buying committee), note those sub-variations.
```

---

## Research Pass 3 — Content Goal Variations

Covers: **Layer 3** of `format-guidelines.md` (goal axis).

```
You are a marketing research expert. Explain how the structure, tone, and approach of
[FORMAT] content should change based on the primary content goal.

Address each goal separately:

1. Thought leadership / credibility building — audience is not considering buying yet;
   goal is to establish expertise and trust
2. Awareness — audience may not know the problem or solution exists; goal is to create
   recognition and relevance
3. Lead generation — goal is to get the reader to take a specific action (fill a form,
   book a call, download a resource)
4. Nurturing — reader is already in a relationship (subscriber, CRM contact); goal is
   to deepen engagement and move them forward
5. Conversion — reader is evaluating options; goal is to tip them toward a decision
6. Retention / customer success — reader is already a customer; goal is to reduce churn,
   deepen usage, or generate referrals

For each goal, cover:
- How length and depth change
- What evidence or proof type is most effective
- How the CTA changes (directness, specificity, ask size)
- How explicitly promotional the content should or should not be
- What emotional or rational appeals are most appropriate
```

---

## Research Pass 4 — Funnel Stage Adaptations

Covers: **Layer 3** of `format-guidelines.md` (funnel stage axis). Complements pass 3 —
where pass 3 covers goal type, this pass covers where the reader is in their journey.

```
You are a marketing research expert. Explain how the optimal approach for [FORMAT]
content changes across buyer funnel stages.

Top of funnel (TOFU) — reader is problem-aware or not yet problem-aware.
Goal: educate, build trust, attract.

Middle of funnel (MOFU) — reader is solution-aware and comparing options.
Goal: demonstrate fit, differentiate, build preference.

Bottom of funnel (BOFU) — reader is in decision stage, evaluating specific vendors.
Goal: remove objections, build confidence, drive action.

For each stage, cover:
- Vocabulary level and jargon tolerance
- How specific claims and proof should be (broad trends vs. precise data)
- Proof types that are most persuasive at this stage
- How direct and explicit the CTA should be
- Optimal content length and depth
- Reader mindset: what are they trying to accomplish, and what is their time budget
- Common mistakes content teams make at this stage
```

---

## Research Pass 5 — Audience Expertise Adaptations

Covers: **Layer 3** of `format-guidelines.md` (audience maturity axis).

```
You are a marketing research expert. Explain how [FORMAT] content should adapt based on
the audience's level of expertise or familiarity with the topic being covered.

Cover three expertise levels:

1. Novice / newcomer — the topic is genuinely new to them; they need orientation,
   definitions, and reassurance that this is worth their time
2. Familiar / practitioner — they know the basics and are looking for depth, nuance,
   or perspectives they haven't considered
3. Expert / advanced — deep practitioner knowledge; they will disengage if you
   over-explain basics or make claims they know to be oversimplified

For each level:
- Appropriate vocabulary and assumption-making
- How much background context to include (and when context becomes condescending)
- What credibility signals resonate (credentials, experience signals, data precision)
- How dense or skimmable the content should be
- What the reader expects to get from this format at their level
- Tone calibrations (reassuring vs. peer-level vs. intellectually challenging)
```

---

## Research Pass 6 — Psychological and Persuasion Principles

Covers: persuasion layer of `format-guidelines.md`.

```
You are an expert in marketing psychology and persuasion. Explain which psychological
and influence principles are most applicable to [FORMAT] content, and how to implement
them effectively in this specific format.

For each relevant principle — draw from:
- Cialdini's 7 principles: reciprocity, commitment & consistency, social proof, liking,
  authority, scarcity, unity
- Pre-suasion (Cialdini): how the opener primes the reader's receptivity
- Cognitive biases: anchoring, loss aversion, the Von Restorff effect, the peak-end rule,
  the IKEA effect, the Zeigarnik effect, or others you find particularly relevant

For each principle you cover:
- How does it apply specifically to [FORMAT]?
- What concrete implementation techniques work best in this format?
- Does effectiveness or implementation differ between B2B and B2C?
- Are there overuse risks or failure modes to avoid (e.g., scarcity backfiring if
  perceived as manipulative in a B2B context)?

Prioritize principles with the strongest evidence base for this specific format.
Rank or weight by impact if possible.
```

---

## Combined mega-prompt (for tools with long context)

Use this when you want to cover all six passes in a single research session. Works well
with Claude.ai (Project with long context), ChatGPT-4o with extended thinking, or
Perplexity Deep Research mode.

```
You are a marketing research expert and persuasion specialist. I am building a
comprehensive, brand-neutral best practice guide for [FORMAT] content. This guide will
be used as a reference by marketing and content teams across different industries,
company sizes, and markets — so it must be universally applicable, not industry-specific.

Provide a thorough, structured response covering all six areas below. Use headers to
separate each area. Cite named frameworks (AIDA, PAS, StoryBrand, Cialdini's principles,
etc.) throughout.

1. UNIVERSAL STRUCTURE
   Optimal length, section structure (with the purpose of each section), hook principles,
   CTA principles, formatting conventions, reading patterns, publishing factors that
   affect format choices, and what separates high-performing from low-performing content.

2. B2B vs. B2C DIFFERENCES
   For each significant difference, include the underlying behavioral or psychological
   reason. Cover: length, tone, evidence types, decision-maker psychology (buying
   committee vs. individual), buying cycle length, CTA expectations, reading context.
   Include sub-variations within B2B where relevant (SMB vs. enterprise, technical
   buyer vs. executive sponsor).

3. CONTENT GOAL VARIATIONS
   How structure and approach shifts for each of these goals, covering length, evidence
   type, CTA directness, and how promotional the content should be:
   (1) thought leadership, (2) awareness, (3) lead generation,
   (4) nurturing, (5) conversion, (6) retention / customer success.

4. FUNNEL STAGE ADAPTATIONS
   TOFU, MOFU, and BOFU: vocabulary, claim specificity, proof types, CTA directness,
   content length, and reader mindset and time budget at each stage. Include common
   mistakes content teams make at each stage.

5. AUDIENCE EXPERTISE ADAPTATIONS
   Novice, familiar, and expert readers: appropriate vocabulary, context-setting depth,
   credibility signals, content density, and tone calibrations.

6. PSYCHOLOGICAL AND PERSUASION PRINCIPLES
   Which Cialdini principles and cognitive biases are most applicable, concrete
   implementation techniques for this format, B2B/B2C variation in effectiveness,
   overuse risks and failure modes. Prioritize by evidence base and impact.
```
