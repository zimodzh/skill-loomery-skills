# Skill Loomery

<p align="center">
  <samp>
    <a href="./README.md">中文</a> ·
    <strong>English</strong>
  </samp>
</p>

<p align="center">
  <img src="https://img.shields.io/github/repo-size/zimodzh/skill-loomery-skills?style=flat-square" alt="repo size" />
  <img src="https://img.shields.io/github/last-commit/zimodzh/skill-loomery-skills?style=flat-square" alt="last commit" />
  <img src="https://img.shields.io/github/license/zimodzh/skill-loomery-skills?style=flat-square" alt="MIT license" />
  <img src="https://img.shields.io/badge/Agent_Skills-compliant-4D6BFE?style=flat-square" alt="Agent Skills compliant" />
</p>

A **skill for making skills** — the complete methodology of "how to write an effective, reliably-triggering, verifiable Agent Skill", distilled into an open-format skill that agents can load on demand.

## What is this

`skill-loomery` is an [Agent Skills](https://agentskills.io)-format skill (a folder containing a `SKILL.md`). Once loaded, any compatible agent (Claude Code / OpenAI Codex / VS Code Copilot, etc.) gains a standard workflow to:

- **Create** new skills
- **Optimize** a skill's description (trigger accuracy)
- **Evaluate** a skill's output quality (evals)
- **Bundle** reusable scripts

## Why

Stop relying on "flipping through pages + writing by feel". Standardize the act of writing a skill: what fields exist, how they're constrained, how to write well, how to verify it actually works, how to bundle scripts — all distilled into a reproducible pipeline.

This repo is that pipeline, itself built as a compliant skill. **Eating our own dog food**: skill-loomery was made with skill-loomery's own standard.

## What's inside

```
skill-loomery/
├── SKILL.md      # entry: frontmatter + core workflow + hard rules
├── references/   # 6 detailed standards, loaded on demand (progressive disclosure)
├── assets/       # starter skeleton (skill-template)
└── evals/        # description trigger eval set + method
```

references cover: format spec · writing best practices · description trigger optimization · output-quality evaluation · script bundling · minimal-example onboarding.

## Directory structure

```
skill-loomery/
├── SKILL.md                      # entry: frontmatter + core workflow + hard rules
├── scripts/                      # executable scripts (see scripts/README.md)
│   └── .gitkeep                  # placeholder so Git tracks the empty directory
├── references/                   # on-demand reference docs (progressive disclosure; index at references/README.md)
│   ├── quickstart.md              # skill definition + 3 stages + roll-dice example
│   ├── specification.md          # SKILL.md format spec
│   ├── best-practices.md         # writing good skills
│   ├── optimizing-descriptions.md# optimizing description triggering
│   ├── evaluating.md             # evals for output quality
│   ├── using-scripts.md          # running commands / bundling scripts
│   ├── README.md                 # directory index (Chinese)
│   └── README.en.md              # directory index (English)
├── assets/                      # static resources (see assets/README.md)
│   └── skill-template.md         # minimal SKILL.md template
├── evals/                       # description trigger eval set + method
│   ├── trigger-queries.json     # trigger regression eval set
│   ├── README.md                # eval method (Chinese)
│   └── README.en.md             # eval method (English)
├── README.md                     # Chinese
├── README.en.md                  # this file (English)
├── LICENSE                       # MIT
└── .gitignore
```

## Installation

The skill is named `skill-loomery`; the Agent Skills spec requires the folder name to match. The repository is named `skill-loomery-skills`. Clone straight into the target folder name:

```bash
git clone https://github.com/zimodzh/skill-loomery-skills.git <your-skills-dir>/skill-loomery
```

Replace `<your-skills-dir>` with the skills directory of your agent. The official docs (Quickstart) give VS Code as the example: `.agents/skills/` under the project. Agent Skills is an open format — the same skill works in other compatible agents (Claude Code, OpenAI Codex, etc.); drop it into that agent's skills directory.

Verify: ask an agent "create a skill that …" or "optimize my skill's description" — the skill should trigger.

## Examples

The minimal complete example is in `references/quickstart.md` — the official `roll-dice` skill (a sub-20-line `SKILL.md` with name + description + executable body). The starter skeleton is `assets/skill-template.md`; copy it to `<name>/SKILL.md` and edit.

## Trigger evals

`evals/trigger-queries.json` is the regression eval set for the description (positive + negative examples). Run the eval and record the pass rate before changing the description; the method (multi-run trigger rate, train/validation split, avoiding overfitting) is in `evals/README.md`, consistent with `references/optimizing-descriptions.md`.

## Source & version

Distilled from the [agentskills.io](https://agentskills.io) official docs, covering all seven pages: Overview, Specification, Quickstart, Best practices, Optimizing descriptions, Evaluating, Using scripts. Where the skill and the official docs disagree, **the official docs win**. Discrepancies welcome via issue or PR.

## Scope

Covers the complete methodology and format spec for **creating, optimizing, evaluating, and bundling Agent Skills**. Does not cover installation/deployment or runtime details of any specific agent — those belong to each agent's own docs.

## Maintenance & contributing

- Before updating any `references/`, check the corresponding official page and cite the source.
- Respect Agent Skills constraints: kebab-case name matching the directory; description ≤ 1024 chars; progressive disclosure in the body.
- PRs welcome: fixes, more examples, a larger eval set, other languages.

## License

[MIT](./LICENSE)
