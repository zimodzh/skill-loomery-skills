# skill-loomery

<p align="center">  <samp>
    <a href="./README.md">中文</a> ·
    <strong>English</strong>
  </samp>
</p>

Weave user goals, project material, and real workflows into reusable,
validated Agent Skills.

## Capabilities

- Extract Skills from requirements, documentation, codebases, runbooks, and hands-on tasks.
- Generate specification-compliant `SKILL.md` files and package layouts.
- Design trigger descriptions, scope boundaries, and default workflows.
- Organize `references/`, `scripts/`, and `assets/` only when useful.
- Validate packages with `skills-ref validate`.
- Design trigger evals, output evals, and with/without-Skill comparisons.
- Iterate from failed assertions, execution traces, and human feedback.

## Layout

```text
skill-loomery/
├── SKILL.md
├── references/
│   ├── specification.md
│   ├── authoring.md
│   ├── triggering.md
│   ├── output-evaluation.md
│   └── script-design.md
├── assets/
│   ├── README.md
│   ├── README.en.md
│   └── evals-template.json
└── evals/
    ├── README.md
    ├── README.en.md
    └── evals.json
```

## Usage

Place this directory in a compatible Agent Skills directory and ask for work
such as:

```text
Create a Skill from this release runbook. Preserve step order and add validation.
```

Validate this Skill:

```bash
skills-ref validate ./skill-loomery
```

If `skills-ref` is unavailable, follow the manual checks in
`references/specification.md`.

## Principles

- Start from real source material, then generalize.
- Keep the main `SKILL.md` short and load details on demand.
- Choose a clear default instead of presenting unnecessary menus.
- Prefer concrete facts, boundaries, and gotchas over generic advice.
- Do not force a Skill into existence without a real reusable need.
- Do not add scripts or dependencies without repeated concrete value.

## Sources

- [Agent Skills Specification](https://agentskills.io/specification)
- [Best practices for skill creators](https://agentskills.io/skill-creation/best-practices)
- [Optimizing skill descriptions](https://agentskills.io/skill-creation/optimizing-descriptions)
- [Evaluating skill output quality](https://agentskills.io/skill-creation/evaluating-skills)
- [Using scripts in skills](https://agentskills.io/skill-creation/using-scripts)

## License

[MIT License](./LICENSE)
