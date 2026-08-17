# assets/ directory

<p align="center">
  <samp>
    <a href="./README.md">中文</a> ·
    <strong>English</strong>
  </samp>
</p>

Directory for static resources: templates, images, data files, and other static material a skill needs.

## What this is, per the spec

The Agent Skills specification (see the parent `references/specification.md`) defines four standard subdirectories a skill may contain. `assets/` is:

> **assets/** — static resources. Contains templates (document templates, configuration templates), images (diagrams, examples), and data files (lookup tables, schemas).

Unlike `scripts/` (executable code) and `references/` (readable docs), `assets/` holds things that are **not executed and don't directly direct the workflow, but are used during it**.

## Current contents

- `skill-template.md` — a minimal SKILL.md template. When creating a new skill, copy this file to `<skill-name>/SKILL.md` and edit it.

## What belongs here

- **Templates**: document templates, configuration templates, output-format templates (see the "Templates for output format" section in `references/best-practices.md`).
- **Images**: diagrams, illustrations, example screenshots.
- **Data files**: lookup tables, schemas, seed data.

Short templates can be inlined directly into `SKILL.md`; longer templates, or ones needed only in specific cases, go here and are referenced on demand from `SKILL.md` — this is exactly the intent of progressive disclosure: load only when needed, without occupying resident context.
