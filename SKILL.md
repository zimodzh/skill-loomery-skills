---
name: skill-loomery
description: Designs, creates, validates, evaluates, and iteratively improves portable Agent Skills from user goals, real workflows, project artifacts, and documentation. Use when the user asks to 创建、开发、编写、修改、验证、评测 or optimize a Skill, SKILL.md, skill package, skill description, trigger evaluation, or output evaluation, even without saying "Agent Skill".
---

# Skill Loomery

Act as a Skill engineer. Turn concrete expertise and repeatable workflows into
small, portable, testable Agent Skills. Do not solve the user's domain task as
the final deliverable unless doing so is needed to extract or test a workflow.

## Boundaries

Activate for creating, revising, packaging, validating, or evaluating Skills.
Do not activate for merely using an existing Skill, ordinary coding, or a
general explanation of Agent Skills.

## Workflow

1. Inspect the workspace and supplied artifacts before drafting. Preserve
   existing user changes. Read source documentation selectively; do not copy
   browser HTML, navigation, or generic background knowledge into the Skill.
2. Define the contract: job, users, inputs, outputs, side effects, defaults,
   constraints, non-goals, and target clients. Default to the open portable
   format when no client is specified.
3. Gather real expertise. Prefer a completed hands-on workflow, project docs,
   schemas, runbooks, review comments, and failure cases. Extract successful
   steps, corrections, formats, conventions, and gotchas.
4. Set activation boundaries. Write concrete should-trigger and should-not-
   trigger cases before polishing the description.
5. Create or update the Skill package:
   - Make directory name and `name` match the specification.
   - Keep `SKILL.md` concise: metadata plus core workflow only.
   - Put detailed, rarely needed material in `references/` and link it with a
     clear loading condition.
   - Add `scripts/` only for deterministic, repeated work. Use relative paths.
   - Add templates in `assets/` only when an output format benefits from one.
6. Validate before claiming completion:
   - Run `skills-ref validate <skill-directory>` when available.
   - Check frontmatter, directory/name match, description length, links, and
     referenced files manually if the validator is unavailable.
   - Check every bundled script with `--help` and a representative safe input.
7. Evaluate the result:
   - For triggering, use about 20 realistic positive and near-miss negative
     queries. Run each several times when the client permits it.
   - For output quality, start with 2-3 realistic cases containing expected
     results and objective assertions. Compare with and without the Skill when
     isolated runs are available.
   - Record concrete evidence, timing/token cost when available, and human
     feedback. Generalize fixes, then rerun failed cases.
8. Report the package path, scope, files changed, validation result, evaluation
   coverage, known limitations, and any decisions still needing the user.

## Progressive disclosure

Read only reference needed for current stage:

- `references/specification.md` for format and validation rules.
- `references/authoring.md` for scope, detail, procedures, and source material.
- `references/triggering.md` for description and activation tests.
- `references/output-evaluation.md` for eval cases, assertions, and iteration.
- `references/script-design.md` before adding executable scripts.

## Quality rules

- Prefer specific project facts over generic advice.
- Give one sound default; mention alternatives only when they change behavior.
- Match instruction strictness to task fragility.
- Keep reusable procedures, not one-off answers.
- Put non-obvious corrections in the main Skill as gotchas.
- Never invent dependencies, commands, APIs, or test evidence.
- Do not add files, scripts, or evaluation machinery without a concrete need.
