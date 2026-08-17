# Script Design

Use this reference before adding executable helpers to a Skill.

## Decide whether a script is needed

Use a one-off existing command when it is short and reliable. Bundle a script
when the same deterministic logic is repeatedly reinvented, the command has
fragile argument handling, or validation needs explicit behavior. Do not add a
script for speculative future use.

Pin tool versions when reproducibility matters. State prerequisites in the
Skill's `compatibility` field or instructions. Keep script references relative
to the Skill root.

## Agent-facing interface

- Never require interactive prompts, TTY input, passwords, or confirmation menus.
- Accept values through flags, environment variables, or stdin.
- Provide concise `--help` output with purpose, options, defaults, and examples.
- Explain errors with the problem, received value, expected value, and next step.
- Write structured data to stdout and progress or diagnostics to stderr.
- Use meaningful non-zero exit codes and document them when useful.

## Safety and reliability

- Make safe operations idempotent where possible.
- Add `--dry-run` for destructive or stateful operations when practical.
- Require explicit confirmation for risky changes.
- Reject ambiguous input instead of guessing.
- Keep default output bounded; support output files or pagination for large data.
- Test the script's help, normal path, invalid input, and safe boundary case.
