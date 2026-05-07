# Claude Code Content Creation Skills

This repository distributes the `cc-content` Claude Code plugin — a suite of
content creation skills for marketing projects.

## Plugin Structure

```
plugins/cc-content/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    ├── _shared/                         shared reference files for skills
    ├── cc-content-linkedin-post/
    ├── cc-content-new-skill/
    ├── cc-content-onboarding/
    ├── cc-content-samples-curation/
    ├── cc-content-session-wrap/
    └── cc-content-update/
```

Each skill lives in its own subdirectory containing a `SKILL.md` and any
companion files (e.g., `format-guidelines.md`).

## Skills

| Skill                         | Purpose                                            |
| ----------------------------- | -------------------------------------------------- |
| `cc-content-onboarding`       | Populate `.claude/context/` via interview          |
| `cc-content-linkedin-post`    | Draft LinkedIn posts                               |
| `cc-content-samples-curation` | Save and annotate gold-standard content examples   |
| `cc-content-session-wrap`     | Review session, collect feedback, commit work      |
| `cc-content-update`           | Update plugin files to their latest versions       |
| `cc-content-new-skill`        | Build a new content-production skill from research |

## Context Architecture

Marketing skills follow a three-level context architecture:

| Level              | Location                                  | What goes here                                                                      |
| ------------------ | ----------------------------------------- | ----------------------------------------------------------------------------------- |
| **Company scope**  | `.claude/context/`                        | Brand voice, buyer personas, company profile — applies to all work for this company |
| **Format scope**   | Inside the plugin skill directory         | Structure rules, length limits, tone calibrations for one output type               |
| **Campaign scope** | Project subfolders (e.g. `campaigns/q3/`) | Campaign briefings, key messages, one-off constraints                               |

### Hierarchical CLAUDE.md Pattern

Place a `CLAUDE.md` at each directory level in your marketing project. Claude Code
reads CLAUDE.md files up the directory tree, so a session in any subfolder
automatically inherits all relevant context.

```
my-marketing-project/
├── CLAUDE.md                   ← @-imports all .claude/context/ files
├── .claude/
│   └── context/
│       ├── company-profile.md
│       ├── brand-voice.md
│       └── buyer-personas.md
└── campaigns/
    └── q3-product-launch/
        ├── CLAUDE.md           ← @-imports campaign-specific files
        └── brief.md
```

**Root CLAUDE.md** — imports company-scope context:

```markdown
@.claude/context/company-profile.md **Read when:** working on any deliverable for this company
@.claude/context/brand-voice.md **Read when:** producing any written content
@.claude/context/buyer-personas.md **Read when:** targeting content at a specific audience segment
```

**Campaign CLAUDE.md** — imports campaign-scope context:

```markdown
@brief.md **Read when:** creating content for this campaign
@key-messages.md **Read when:** writing copy or posts for this initiative
```
