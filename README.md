# Claude Code Content Creation Skills

A [Claude Code](https://claude.ai/code) plugin that provides a suite of content creation skills for marketing projects — brand onboarding, LinkedIn posts, sample curation, and session management.

---

## Plugin: `cc-content`

| Skill                                     | What it does                                                                                                                                         |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/cc-content:cc-content-onboarding`       | Interviews you about your brand, voice, and audience, then populates `.claude/context/` with structured context files that all other skills can read |
| `/cc-content:cc-content-linkedin-post`    | Drafts LinkedIn posts that match your brand voice, format guidelines, and audience — with a built-in feedback step                                   |
| `/cc-content:cc-content-samples-curation` | Saves gold-standard content examples with annotations to `.claude/context/samples.md` so skills can use them as reference material                   |
| `/cc-content:cc-content-session-wrap`     | Reviews session deliverables, logs feedback and corrections, detects recurring patterns, and commits your work                                       |
| `/cc-content:cc-content-update`           | Updates the installed plugin files to their latest versions from the source repository                                                               |
| `/cc-content:cc-content-new-skill`        | Turns external research reports into a fully structured content-production skill for a new output format                                             |

---

## Installation

In Claude Code, run:

```
/install-plugin https://github.com/MichaelvanLaar/claude-code-skills-for-content-creation
```

This installs the `cc-content` plugin and makes all skills immediately available.

---

## Getting Started

**Step 1 — Run onboarding**

Open your marketing project in Claude Code and run the onboarding skill:

```
/cc-content:cc-content-onboarding
```

This populates `.claude/context/` with your brand voice, company profile, and buyer personas. The other skills read this context automatically.

**Step 2 — Create content**

Run any output skill:

```
/cc-content:cc-content-linkedin-post
```

**Step 3 — Curate good examples**

When a piece of content lands well, save it as a reference sample:

```
/cc-content:cc-content-samples-curation
```

**Step 4 — Wrap your session**

At the end of each working session, commit your work and log any corrections:

```
/cc-content:cc-content-session-wrap
```

**Step 5 — Stay up to date**

Pull the latest plugin files from the source repository:

```
/cc-content:cc-content-update
```

---

## Context Architecture

Skills follow a three-level context model:

| Level              | Location                                  | What goes here                                                                      |
| ------------------ | ----------------------------------------- | ----------------------------------------------------------------------------------- |
| **Company scope**  | `.claude/context/`                        | Brand voice, buyer personas, company profile — applies to all work for this company |
| **Format scope**   | Inside the plugin                         | Structure rules, length limits, tone calibrations for one output type               |
| **Campaign scope** | Project subfolders (e.g. `campaigns/q3/`) | Campaign briefings, key messages, one-off constraints                               |

Load company-scope context by importing it from your project-level `CLAUDE.md`:

```markdown
@.claude/context/brand-voice.md **Read when:** producing any written content
@.claude/context/company-profile.md **Read when:** working on any deliverable
@.claude/context/buyer-personas.md **Read when:** targeting content at a specific audience segment
```

---

## License

[MIT](LICENSE) — Copyright (c) 2026 Michael van Laar
