# Skill Authoring Guidance

Use this reference while drafting or revising instructions.

## Start with real expertise

Generic LLM knowledge produces generic Skills. Prefer:

- A completed hands-on task and the user's corrections.
- Internal docs, runbooks, schemas, API specifications, and style guides.
- Review comments, issue history, patches, fixes, incidents, and recoveries.

Extract what actually worked, what was corrected, input/output formats, project
conventions, failure modes, and non-obvious edge cases.

## Spend context carefully

Include facts the agent would likely get wrong without help. Omit definitions,
background, and general advice the agent already knows. Keep each Skill a
coherent unit of work; do not combine unrelated domains.

Use moderate detail. A short ordered procedure with a working example usually
beats exhaustive documentation. Move rare details to a reference file and tell
the agent exactly when to load it.

## Calibrate control

- Give freedom where several approaches are safe and equivalent.
- Be prescriptive where order, consistency, or safety matters.
- Choose one default instead of presenting a menu of equal options.
- Explain why a fragile step exists when that improves correct adaptation.
- Prefer reusable procedures over one-off answers.

## High-value patterns

### Gotchas

Record concrete facts that defeat reasonable assumptions. Examples include
different names for the same identifier across systems, soft-delete rules, a
health endpoint that is not a readiness check, or a command that requires a
specific order.

### Templates

Use a Markdown or data template when output structure matters. Concrete
templates are more reliable than prose descriptions of formatting.

### Checklists

Use checklists for multi-step work with dependencies or validation gates. Keep
steps observable and actionable.

### Validation loops

Use the loop: do work, run validator, inspect failures, fix, rerun. Do not
declare success based on intent or an unverified output.

### Plan-validate-execute

For batch or destructive work, create a structured intermediate plan, validate
it against the source of truth, then execute only after validation passes.

## Scope test

Ask whether each instruction changes behavior. If removing it would not change
the result, omit it or move it out of the main Skill. If the task is already
handled well without the Skill, the Skill may not need to exist.
