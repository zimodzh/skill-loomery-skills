# Specification — the SKILL.md format

> Source: agentskills.io/specification (official, distilled)

## Directory structure

```
skill-name/
├── SKILL.md    # required: metadata + instructions
├── scripts/    # optional: executable code
├── references/ # optional: on-demand documentation
├── assets/     # optional: templates / static resources
└── ...         # any additional files or directories
```

## SKILL.md = YAML frontmatter + Markdown body

### Frontmatter fields

| Field | Required | Constraints |
|---|---|---|
| name | Yes | ≤64 chars; lowercase letters, numbers, and hyphens only (`a-z 0-9 -`); must not start or end with a hyphen; no consecutive hyphens; **must match the parent directory name** |
| description | Yes | 1–1024 chars; states what the skill does and when to use it; include keywords |
| license | No | License name, or a reference to a bundled license file |
| compatibility | No | ≤500 chars; environment requirements (intended product / system packages / network access, etc.) |
| metadata | No | Arbitrary string→string mapping; make key names reasonably unique to avoid conflicts |
| allowed-tools | No | Space-separated string of pre-approved tools (Experimental) |

### Minimal example

```yaml
---
name: skill-name
description: A description of what this skill does and when to use it.
---
```

### Example with optional fields

```yaml
---
name: pdf-processing
description: Extract PDF text, fill forms, merge files. Use when handling PDFs.
license: Apache-2.0
metadata:
  author: example-org
  version: "1.0"
---
```

### The name field, in detail

- 1–64 characters
- May only contain unicode lowercase alphanumeric characters + hyphens
- Must not start or end with a hyphen
- Must not contain consecutive hyphens
- Must match the parent directory name

Valid: `data-analysis`, `code-review`
Invalid: `PDF-Processing` (uppercase), `-pdf` (leading hyphen), `pdf--processing` (consecutive hyphens)

### The description field, in detail

Good (specific, with trigger keywords, states when to use):

```
Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.
```

Poor (too vague):

```
Helps with PDFs.
```

### Body content

The Markdown body after the frontmatter holds the skill instructions. No format restrictions. Recommended sections: step-by-step instructions, examples of inputs and outputs, common edge cases.

> ⚠️ The agent loads this entire file once it activates the skill. Split longer content into referenced files.

## Optional directories

- **scripts/**: executable code. Should be self-contained or clearly document dependencies, give helpful error messages, handle edge cases gracefully.
- **references/**: additional docs read on demand (REFERENCE.md, FORMS.md, domain-specific files). Keep each file focused.
- **assets/**: static resources (templates, images, data files).

## Progressive disclosure

Three levels of loading:

1. **Metadata (~100 tokens)**: name + description loaded at startup for all skills.
2. **Instructions (recommended <5000 tokens)**: the full SKILL.md body loads on activation.
3. **Resources (on demand)**: files in scripts/ / references/ / assets/ load only when required.

Keep SKILL.md under 500 lines; move detail into references/.

## File references

Use relative paths from the skill root:

```markdown
See [the reference guide](references/REFERENCE.md) for details.
Run the extraction script: scripts/extract.py
```

Keep references one level deep; avoid deeply nested chains.

## Validation

```bash
skills-ref validate ./my-skill
```

Checks that your SKILL.md frontmatter is valid and follows naming conventions.
