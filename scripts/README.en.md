# scripts/ directory

<p align="center">
  <samp>
    <a href="./README.md">中文</a> ·
    <strong>English</strong>
  </samp>
</p>

Directory for executable scripts. Currently empty, held in place by `.gitkeep`.

## What this is, per the spec

The Agent Skills specification (see the parent `references/specification.md`) defines four standard subdirectories a skill may contain. `scripts/` is:

> **scripts/** — executable code that agents can run. Scripts should be **self-contained or clearly document dependencies**, **give helpful error messages**, and **handle edge cases gracefully**. Supported languages depend on the agent implementation; common options include Python, Bash, and JavaScript.

In other words, `scripts/` is used only when a skill's **execution needs to run code** — bundling tested tool scripts so the agent can execute them once the skill activates. Typical examples:

- A data-processing skill bundles `scripts/process.py`
- A code-review skill bundles `scripts/lint.sh`
- A form-filling skill bundles `scripts/validate_fields.py`

## Why skill-loomery's `scripts/` is currently empty

Because **skill-loomery is a methodology skill**: it only teaches the agent how to think, organize, and verify a skill, and never needs to run code throughout that process. It has — and currently needs — no executable scripts. So the directory is empty **by design**, not by omission.

## What the `.gitkeep` is for

Git does not track empty directories. `.gitkeep` is a placeholder that exists solely so the `scripts/` directory survives cloning and stays part of the repository structure. It has no functional role — it just "holds the directory's place".

## What would go in here in the future

Only when skill-loomery gains "executable" capabilities would scripts be added. Plausible directions (all speculative, none implemented yet):

- `validate-skill.sh` — automatically validate a SKILL.md's frontmatter against the spec
- `run-evals.sh` — automatically run the test cases in `evals/` and aggregate results

Once any script is added, the directory gains real content and `.gitkeep` can be removed.

## Which scripts belong here

For guidance on when to write logic as a bundled script and how scripts should be designed for agentic use, see the parent `references/using-scripts.md`. Key points:

- **Never interactive** (hard requirement): agents run in non-interactive shells, so a script must not block on input.
- **Write `--help`**: so the agent can understand the interface.
- **Structured output**: prefer JSON / CSV / TSV.
- **Idempotent, `--dry-run`, meaningful exit codes**.
