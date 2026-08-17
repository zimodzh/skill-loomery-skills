# references/ directory

<p align="center">
  <samp>
    <a href="./README.md">中文</a> ·
    <strong>English</strong>
  </samp>
</p>

On-demand reference documentation. This is the body of skill-loomery's knowledge — SKILL.md keeps only the core workflow and hard rules; the detail lives here, loaded on demand.

## Index

| File | What it covers | When to read it |
|---|---|---|
| `quickstart.md` | The definition of an Agent Skill + the Discovery→Activation→Execution model + the roll-dice minimal example | Quickly understand what a skill is and what it looks like |
| `specification.md` | The full SKILL.md format spec (frontmatter fields, constraints, directory conventions, progressive disclosure, validation) | Look up field format / constraints |
| `best-practices.md` | Writing good skills (real expertise, context economy, coherent units, control calibration, instruction patterns) | How to write well |
| `optimizing-descriptions.md` | Optimizing description triggering (trigger model, writing principles, eval queries, train/validation, the loop) | When the description triggers unreliably |
| `evaluating.md` | Assessing output quality (test cases, grading, aggregation, pattern analysis, human review, iteration) | Verifying the skill actually works |
| `using-scripts.md` | Running commands / bundling scripts (one-off commands, self-contained scripts, agentic script design) | Whether to run code / how to bundle scripts |

## Suggested reading order

- **First contact**: `quickstart.md` — build the overall picture.
- **Starting to write**: `specification.md` (format) + `best-practices.md` (writing).
- **Tuning after writing**: `optimizing-descriptions.md` (triggering) + `evaluating.md` (quality).
- **Needing code**: `using-scripts.md`.

This order is the expansion of steps 4–8 of SKILL.md's "Core workflow".
