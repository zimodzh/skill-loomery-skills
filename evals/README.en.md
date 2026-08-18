# evals/ directory

<p align="center">
  <samp>
    <a href="./README.md">中文</a> ·
    <strong>English</strong>
  </samp>
</p>

The trigger eval set for the description. It verifies that `skill-loomery`'s description triggers on the right prompts.

## Files

- `trigger-queries.json` — 20 eval queries: 10 positive (should_trigger = true) + 10 negative (should_trigger = false).

## Method

Consistent with `references/optimizing-descriptions.md`. Key points:

1. **Run multiple times**: model behavior is nondeterministic — run each query ≥3 times and compute the **trigger rate** (fraction of runs where the skill was invoked).
2. **Pass condition**: a positive passes if trigger rate > 0.5 (a reasonable default); a negative passes if trigger rate < 0.5.
3. **train/validation split**: when optimizing the description, use ~60% of queries to guide changes and hold out ~40% for validation, to avoid overfitting; shuffle and keep the split fixed across iterations.
4. **Record pass rates**: run before and after each description change and compare, to confirm the change generalizes.

## When to run

- Before changing the `description` in `SKILL.md`.
- After adding or adjusting eval queries.

## Structure

Each entry in `trigger-queries.json`:

```json
{ "query": "help me create a skill for code review", "should_trigger": true }
```

- `query`: something a real user would actually type.
- `should_trigger`: true = should trigger skill-loomery; false = should not.
