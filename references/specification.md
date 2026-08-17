# Agent Skills Specification

Use this reference when creating or validating a Skill package.

## Package layout

Minimum package:

```text
skill-name/
└── SKILL.md
```

Optional directories:

```text
skill-name/
├── scripts/       # executable helpers
├── references/    # detailed material loaded on demand
└── assets/        # templates and static resources
```

## Frontmatter

`SKILL.md` starts with YAML frontmatter, followed by Markdown instructions.

| Field | Required | Rules |
| --- | --- | --- |
| `name` | yes | 1-64 characters; lowercase letters, numbers, and hyphens; no leading, trailing, or consecutive hyphens; must match parent directory |
| `description` | yes | 1-1024 characters; state capability and activation context; include useful keywords |
| `license` | no | License name or bundled license reference |
| `compatibility` | no | 1-500 characters; only when environment requirements exist |
| `metadata` | no | String-to-string mapping |
| `allowed-tools` | no | Space-separated pre-approved tools; experimental and client-dependent |

Example:

```markdown
---
name: example-skill
description: Does a concrete task. Use when the user needs that task or mentions its relevant files and terms.
license: MIT
---
```

## Body and references

The body has no special format restrictions. Prefer step-by-step procedures,
examples, edge cases, and validation gates. Keep the main file under 500 lines
and about 5,000 tokens; under 100 lines is a useful target for a focused Skill.

Reference files must use relative paths from the Skill root. Keep references one
level deep from `SKILL.md`, and say when each file should be read. Avoid a chain
where one reference points to another.

## Validation

Run the reference validator when available:

```bash
skills-ref validate ./my-skill
```

Also verify that every referenced file exists, every script path is correct, and
the package does not rely on undocumented working-directory assumptions.
