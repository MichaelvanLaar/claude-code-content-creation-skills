# Claude Code Skills: Marketing & Content Creation

Reusable Claude Code skills for setting up and maintaining marketing/content creation projects. Each skill is a single `SKILL.md` file. Users install skills into their own projects via a curl install script (like the pattern at `MichaelvanLaar/claude-code-config-skills`).

## Key Config Files

| File | Purpose |
|------|---------|
| `.claude/settings.json` | Permissions, hooks, environment variables           |
| `.claude/skills/cc-init/SKILL.md` | Bootstrap Claude Code configuration skill           |
| `.claude/skills/cc-optimize/SKILL.md` | Audit and optimize Claude Code configuration skill  |
| `.claude/skills/cc-update/SKILL.md` | Update skills to latest versions skill              |
| `.githooks/pre-commit` | Runs sync-config-table.sh before each commit        |
| `.gitignore` | Git ignore patterns                                 |
| `CLAUDE.md` | Project instructions, loaded every message          |
| `scripts/sync-config-table.sh` | Keeps Key Config Files table in CLAUDE.md in sync   |

## Structure

```
.claude/skills/[skill-name]/SKILL.md   one directory per skill
install.sh                              TODO: distributes skills to target projects
```

## SKILL.md Frontmatter

Every skill must have this YAML frontmatter:

```yaml
---
name: skill-name
description: When to invoke (routing key — Claude reads this to decide when to trigger the skill)
allowed-tools: Read, Write, Edit, Bash, Grep, Glob
argument-hint: "[optional: describe expected args]"
---
```

- `description` is the routing key — write it as specific trigger statements, not a generic summary
- `allowed-tools` lists only the tools the skill actually needs; don't over-grant
- `argument-hint` is optional; omit if the skill takes no arguments

## Commands

- TODO: `bash install.sh` — once `install.sh` exists, installs skills into target project
- No build or test commands yet; validate SKILL.md frontmatter by inspection

## Don't

- Don't commit secrets or credentials to git
- Don't use `--force` flags — fix the underlying issue instead
- Don't add company-specific context here — that belongs in the target project's `.claude/context/`
- Don't embed target-project knowledge (brand voice, personas, etc.) in skill definitions

## Learnings

When the user corrects a mistake or points out a recurring issue, append a one-line summary to `.claude/learnings.md`. Don't modify CLAUDE.md directly.

## Compact Instructions

When compacting, preserve: list of modified files, open TODOs, and key decisions made.
