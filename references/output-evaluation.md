# Output Evaluation

Use this reference after a Skill can be discovered.

## Test case format

Start with 2-3 realistic cases. Store them in `evals/evals.json` inside the
Skill under test:

```json
{
  "skill_name": "example-skill",
  "evals": [
    {
      "id": 1,
      "prompt": "A realistic user request",
      "expected_output": "Observable description of success",
      "files": [],
      "assertions": [
        "A concrete, verifiable condition is true"
      ]
    }
  ]
}
```

Vary wording, detail, formality, and complexity. Include at least one boundary
case such as malformed input, missing information, or an ambiguous request.

## Baseline and workspace

Run each case with the Skill and without it, or against a snapshot of the prior
version. Use clean contexts. Keep outputs, timing, token counts, and grades in a
separate workspace with one directory per iteration. Do not mix generated eval
outputs into the Skill's source files.

## Assertions and grading

Assertions must be observable and not brittle. Good assertions check valid JSON,
file existence, row counts, required sections, or labeled output. Avoid vague
claims such as "the result is good" and exact wording requirements unless
wording itself is the contract.

Grade each assertion as PASS or FAIL with concrete evidence. Use code for
mechanical checks and human or model review for holistic qualities. Review the
assertions too: remove assertions that always pass in both configurations or
are impossible to verify.

## Iteration

Use three signals together:

- Failed assertions identify missing or unclear instructions.
- Human feedback identifies quality problems not covered by assertions.
- Execution traces reveal ignored instructions and wasted work.

Generalize fixes, keep the Skill lean, rerun all cases in a new iteration, and
stop when results are stable or further changes have no meaningful benefit.
