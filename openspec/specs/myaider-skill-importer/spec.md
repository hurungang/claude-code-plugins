# myaider-skill-importer Specification

## Purpose
TBD - created by archiving change add-version-metadata-to-skill-importer. Update Purpose after archive.
## Requirements
### Requirement: Skill Frontmatter Includes Version Metadata

The `myaider-skill-importer` SKILL.md frontmatter MUST include `version` and `updated_at` fields, consistent with the metadata format the skill instructs for imported skills.

#### Scenario: Frontmatter contains version field

- **WHEN** the `myaider-skill-importer` SKILL.md is read
- **THEN** the YAML frontmatter includes a `version` field with a semver string (e.g., `"1.0.0"`)

#### Scenario: Frontmatter contains updated_at field

- **WHEN** the `myaider-skill-importer` SKILL.md is read
- **THEN** the YAML frontmatter includes an `updated_at` field with an ISO 8601 timestamp (e.g., `2026-04-20T00:00:00Z`)

