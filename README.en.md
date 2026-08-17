# Skill Loomery

**Language / 语言**：[简体中文](README.md) · [English](#)

A **skill for making skills** — the complete methodology of "how to write an effective, reliably-triggering, verifiable Agent Skill", distilled into an open-format skill that agents can load on demand.

> Standards first, with quiet self-respect. Open-sourcing is just giving good work a home — never an excuse to compromise my own standards.

## What is this

`skill-loomery` is an [Agent Skills](https://agentskills.io)-format skill (a folder containing a `SKILL.md`). Once loaded, any compatible agent (Claude Code / OpenAI Codex / VS Code Copilot / DSH, etc.) gains a standard workflow to:

- **Create** new skills
- **Optimize** a skill's description (trigger accuracy)
- **Evaluate** a skill's output quality (evals)
- **Bundle** reusable scripts

## Why

Stop relying on "flipping through pages + writing by feel". Standardize the act of writing a skill: what fields exist, how they're constrained, how to write well, how to verify it actually works, how to bundle scripts — all distilled into a reproducible pipeline.

This repo is that pipeline, itself built as a compliant skill. **Eating our own dog food**: skill-loomery was made with skill-loomery's own standard.

## Directory structure

```
skill-loomery/
├── SKILL.md                      # entry: frontmatter + core workflow + hard rules
├── references/                   # on-demand reference docs (progressive disclosure)
│   ├── specification.md          # SKILL.md format spec
│   ├── best-practices.md         # writing good skills
│   ├── optimizing-descriptions.md# optimizing description triggering
│   ├── evaluating.md             # evals for output quality
│   └── using-scripts.md          # running commands / bundling scripts
├── assets/
│   └── skill-template.md         # minimal SKILL.md template
├── README.md                     # Chinese
├── README.en.md                  # this file (English)
└── LICENSE                       # MIT
```

## Quick start

1. Put this repo into your skills directory (VS Code default `.agents/skills/`, DSH default `~/.agents/skills/`).
2. Tell an agent: "create a skill that …", or "optimize my skill's description".
3. The agent triggers skill-loomery and follows the `SKILL.md` workflow.

## Specification

The canonical format is the [Agent Skills Specification](https://agentskills.io/specification); `references/specification.md` is a distillation.

## License

[MIT](./LICENSE)
