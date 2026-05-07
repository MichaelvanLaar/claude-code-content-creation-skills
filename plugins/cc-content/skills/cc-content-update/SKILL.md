---
name: cc-content-update
description: >
  Update the cc-content plugin files to their latest versions from the source
  repository. Use when the user says "update content skills", "update
  cc-content skills", "get the latest content skills", "refresh content
  skills", or "cc-content update".
allowed-tools: Bash, Read
---

# Update cc-content Skills

Fetch the latest versions of all cc-content plugin files from the source
repository and replace the local copies.

## Step 1: Locate the plugin root

The plugin root is two levels above this skill's directory:

```bash
PLUGIN_ROOT="${CLAUDE_SKILL_DIR}/../.."
ls "$PLUGIN_ROOT/.claude-plugin/plugin.json" 2>/dev/null && echo "found" || echo "NOT_FOUND"
```

If `NOT_FOUND`, abort and tell the user:

> "Cannot locate the plugin root. Expected `plugin.json` at
> `${CLAUDE_SKILL_DIR}/../../.claude-plugin/plugin.json`. The plugin may
> have been installed in an unexpected location — check your Claude Code
> plugin configuration."

If found, confirm: "✓ Plugin root located at `${CLAUDE_SKILL_DIR}/../..`."

## Step 2: Download and replace all plugin files

```bash
BASE_URL="https://raw.githubusercontent.com/MichaelvanLaar/claude-code-skills-for-content-creation/main/plugins/cc-content"
PLUGIN_ROOT="${CLAUDE_SKILL_DIR}/../.."

update_file() {
  local remote_path="$1"
  local local_path="$2"
  if curl -fsSL "$BASE_URL/$remote_path" -o "$local_path"; then
    echo "✓ $remote_path"
  else
    echo "✗ $remote_path (download failed)"
  fi
}

# Self-update first (always)
update_file "skills/cc-content-update/SKILL.md" "${CLAUDE_SKILL_DIR}/SKILL.md"

# Skill files
update_file "skills/cc-content-linkedin-post/SKILL.md"       "$PLUGIN_ROOT/skills/cc-content-linkedin-post/SKILL.md"
update_file "skills/cc-content-linkedin-post/format-guidelines.md" "$PLUGIN_ROOT/skills/cc-content-linkedin-post/format-guidelines.md"
update_file "skills/cc-content-new-skill/SKILL.md"           "$PLUGIN_ROOT/skills/cc-content-new-skill/SKILL.md"
update_file "skills/cc-content-onboarding/SKILL.md"          "$PLUGIN_ROOT/skills/cc-content-onboarding/SKILL.md"
update_file "skills/cc-content-samples-curation/SKILL.md"    "$PLUGIN_ROOT/skills/cc-content-samples-curation/SKILL.md"
update_file "skills/cc-content-session-wrap/SKILL.md"        "$PLUGIN_ROOT/skills/cc-content-session-wrap/SKILL.md"

# Shared reference files
update_file "skills/_shared/context-categories.md"           "$PLUGIN_ROOT/skills/_shared/context-categories.md"
update_file "skills/_shared/content-skill-research-brief.md" "$PLUGIN_ROOT/skills/_shared/content-skill-research-brief.md"
update_file "skills/_shared/persuasion-principles.md"        "$PLUGIN_ROOT/skills/_shared/persuasion-principles.md"
update_file "skills/_shared/storytelling-frameworks.md"      "$PLUGIN_ROOT/skills/_shared/storytelling-frameworks.md"
```

## Step 3: Report and wrap up

After the downloads complete:

1. List each file: updated (✓) or failed (✗).
2. If any downloads failed, advise the user to check their internet connection
   and try again, or download the files manually from the repository.

## What NOT to do

- Do not modify any files in the user's project.
- Do not run any skill after updating.
- Do not update `plugin.json` — changes to the plugin manifest are handled
  separately and could affect plugin registration.

## Feedback

Before ending the session, ask: "Were all files updated successfully? If
anything went wrong, I'll log it to `.claude/learnings.md`."
