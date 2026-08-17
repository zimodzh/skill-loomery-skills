# Best Practices — writing good skills

> Source: agentskills.io/skill-creation/best-practices (official, distilled)

## Core principle: start from real expertise, don't let the LLM invent

The most common pitfall is asking an LLM to generate a skill without domain-specific context, relying only on its general training knowledge. The result is vague, generic procedures ("handle errors appropriately") instead of the specific API patterns, edge cases, and project conventions that make a skill valuable.

**Three sources:**

1. **Extract from a hands-on task**: complete a real task in conversation with an agent, providing context, corrections, and preferences, then extract the reusable pattern. Pay attention to:
   - Steps that worked — the sequence of actions that led to success
   - Corrections you made — places where you steered the agent ("use library X instead of Y", "check for edge case Z")
   - Input/output formats — what the data looked like going in and out
   - Context you provided — project-specific facts the agent didn't already know
2. **Synthesize from existing project artifacts**: feed internal docs / runbooks / style guides, API specs / schemas / configs, code review comments, version control history (especially patches/fixes), real failure cases → ask an LLM to synthesize.
3. **Refine with real execution**: run the skill against real tasks, feed **all** results (not just failures) back. Read execution traces, not just final outputs.

## Spend context wisely

Once a skill activates, its full SKILL.md body loads into context alongside everything else. Every token competes for attention.

- **Add what the agent lacks, omit what it knows**: project conventions, domain procedures, non-obvious edge cases, specific tools/APIs. Don't explain what a PDF is.
- For each piece of content ask: **"Would the agent get this wrong without this instruction?"** If no, cut it.

## Design coherent units

- Too narrow → multiple skills must load for one task (overhead, conflicting instructions).
- Too broad → hard to activate precisely.
- Example: "query a database and format the results" is one coherent unit; adding "database administration" is too much.

## Aim for moderate detail

Overly comprehensive skills hurt — the agent struggles to extract what matters. Concise, stepwise guidance with a working example outperforms exhaustive documentation.

## Structure large skills with progressive disclosure

Keep SKILL.md under 500 lines / 5,000 tokens — just the core instructions needed on every run. Move detail to separate files. **Tell the agent when to load each file** ("Read references/api-errors.md if the API returns a non-200 status code"), not a generic "see references/".

## Calibrate control (match specificity to fragility)

- **Give freedom** when multiple approaches are valid: explaining *why* beats rigid directives.
- **Be prescriptive** when operations are fragile, consistency matters, or a specific sequence must be followed (e.g. database migration: "run exactly this sequence, do not modify the command").
- Most skills mix both; calibrate each part independently.

### Provide defaults, not menus
When multiple tools could work, pick a default and mention alternatives briefly — don't present them as equal options.

### Favor procedures over declarations
Teach the agent how to approach a *class* of problems, not what to produce for one instance.

- Bad: `JOIN orders to customers on customer_id, WHERE region='EMEA', SUM amount` (only works for this exact task)
- Good: read schema → join via `_id` convention → apply WHERE from the request → aggregate → format as markdown table (generalizes)

## Patterns for effective instructions

### Gotchas sections (highest value)
Environment-specific facts that defy reasonable assumptions — concrete corrections, not generic advice:

```markdown
## Gotchas
- The users table uses soft deletes; queries must include WHERE deleted_at IS NULL
- The user ID is user_id in the DB, uid in the auth service, and accountId in the billing API — all the same value
- /health returns 200 as long as the web server runs, even if the DB is down; use /ready
```

**Every time you correct the agent's mistake, add that correction to the gotchas section** — the most direct iterative improvement.

### Templates for output format
When a specific output shape is required, provide a template (agents pattern-match concrete structures better). Short templates inline; long/conditional ones in assets/, referenced on demand.

### Checklists for multi-step workflows
Use `- [ ]` checklists to help the agent track progress and avoid skipping steps, especially with dependencies or validation gates.

### Validation loops
Do the work → run a validator (script / reference / self-check) → fix → re-run, until it passes. A reference doc can serve as the "validator".

### plan-validate-execute
For batch/destructive operations: produce a structured intermediate plan → validate against a source of truth → only then execute.

### Bundling reusable scripts
If the agent repeatedly reinvents the same logic (charting, parsing, validating), write it once, test it, bundle into scripts/ (see using-scripts.md).
