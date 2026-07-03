## Context

The `myaider-skill-importer` SKILL.md currently has frontmatter with `name`, `description`, and `compatibility` fields. It lacks `version` and `updated_at`, which it mandates for all skills it imports.

## Goals / Non-Goals

**Goals:**
- Add `version` and `updated_at` to the skill's own frontmatter

**Non-Goals:**
- Changing the skill's behavior or workflow
- Adding automated version-bumping tooling

## Decisions

### Decision 1: Use the same metadata format the skill prescribes for imports

The skill instructs imported skills to use `version` (semver) and `updated_at` (ISO 8601). We use the exact same format here for consistency. Initial version will be `"1.0.0"` and `updated_at` will be today's date (`2026-04-20T00:00:00Z`).
