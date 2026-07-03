## Why

The `myaider-skill-importer` skill instructs that all skills it imports should include `source` and `updated_at` metadata in their frontmatter for traceability. The importer skill itself is missing this same metadata, creating an inconsistency — it doesn't practice what it preaches.

## What Changes

- Add `version` field to the skill's YAML frontmatter
- Add `updated_at` field to the skill's YAML frontmatter

## Capabilities

### New Capabilities
<!-- none -->

### Modified Capabilities
- `myaider-skill-importer`: Gains version and timestamp metadata in its frontmatter, consistent with the metadata format it applies to imported skills

## Impact

- `myaider-skill-importer/skills/myaider-skill-importer/SKILL.md`: Add `version` and `updated_at` fields to YAML frontmatter
