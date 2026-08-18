---
name: skill-loomery
description: 创建、优化、评估、打包 Agent Skills 的标准流程。当需要新建 skill、改进 description 或指令、搭建 evals、或打包可复用脚本时使用。Create and refine Agent Skills. A standard workflow for authoring, optimizing, evaluating, and bundling Agent Skills (SKILL.md format). Use when creating a new skill, improving a skill's description or instructions, setting up evals, or bundling reusable scripts.
license: MIT
compatibility: "Any skills-compatible agent (Claude Code, OpenAI Codex, VS Code Copilot, DSH, etc.). No special system dependencies; optional tools: git, uv, npm."
metadata:
  author: skill-loomery
  version: "1.0.0"
  repo: skill-loomery-skills
---

# Skill Loomery

A standard workflow for turning expertise into a reusable, reliably-triggering, verifiable **Agent Skill**. Follow this when the user wants to **create a new skill, improve an existing skill's description or instructions, set up evals, or bundle scripts**.

## Core workflow (in order)

1. **Extract real expertise** (see references/best-practices.md, "Start from real expertise")
   Do not let an LLM invent from general knowledge. Distill from real tasks, project artifacts, and execution traces.

2. **Scope the unit**: one skill should encapsulate one coherent unit of work — too narrow forces multiple skills to load; too broad is hard to trigger precisely.

3. **Scaffold**: `<name>/` directory + `SKILL.md` (required) + optional `scripts/`, `references/`, `assets/`. Copy `assets/skill-template.md` to `<name>/SKILL.md` as a starting skeleton, then edit it.

4. **Write frontmatter** (see references/specification.md)
   - `name`: ≤64 chars, lowercase/numbers/hyphens only, no leading/trailing/consecutive hyphen, **must match the parent directory name**.
   - `description`: ≤1024 chars, states both what it does and when to use it, with trigger keywords.

5. **Write the body** (see references/best-practices.md)
   Step-by-step instructions + a gotchas list + templates + checklists + validation loops.

6. **Optimize the description** (see references/optimizing-descriptions.md)
   Write imperatively, focus on user intent; iterate with trigger-rate evals over a train/validation split (~20 queries, ≥3 runs each) to avoid overfitting, until it triggers reliably.

7. **Evaluate quality** (see references/evaluating.md)
   Evals over a with-skill vs without-skill baseline + assertion grading + benchmark aggregation (delta cost/benefit) + human review, forming a full iteration loop.

8. **Bundle scripts** (see references/using-scripts.md)
   When code is needed, follow the one-off command / self-contained script / agentic script design rules.

9. **Validate (mandatory)**: run `skills-ref validate ./<skill-name>` immediately after writing frontmatter (see references/specification.md, "Validation"). A YAML syntax error — e.g. an unquoted `: ` — makes the loader silently skip the skill, so never ship without validating.

## Hard rules (always)

- Keep SKILL.md **under 500 lines / 5,000 tokens**; move detail into `references/` for on-demand loading (progressive disclosure).
- **Write only what the agent doesn't already know**; cut anything it does (e.g. "what a PDF is").
- Reference files with **relative paths from the skill root**, at most one level deep.
- Calibrate instruction strictness to task fragility: give freedom + reasoning for tolerant tasks; give hard commands for fragile/high-risk ones.
- Agent-facing scripts: **never interactive**, have `--help`, structured output, idempotent, `--dry-run`.
- In YAML frontmatter, **always quote any value containing `: ` (colon + space)** — an unquoted `key: value` inside a scalar breaks parsing and the skill is silently skipped.

## What a compliant skill looks like

```
skill-name/
├── SKILL.md        # required: frontmatter + instructions
├── scripts/        # optional: executable code
├── references/     # optional: on-demand docs
├── assets/         # optional: templates / static resources
└── evals/          # optional: evals.json + test files
```

A minimal working skill is a single `SKILL.md` file — see the complete roll-dice example in references/quickstart.md, and the starter skeleton at `assets/skill-template.md`.

## When to load which reference

| Scenario | Load |
|---|---|
| What a skill is / minimal example | references/quickstart.md |
| Field format / constraints | references/specification.md |
| How to write well | references/best-practices.md |
| Description triggers unreliably | references/optimizing-descriptions.md |
| How to verify quality | references/evaluating.md |
| Whether to run code / bundle scripts | references/using-scripts.md |
