# Trigger Evaluation

Use this reference to make a Skill activate for the right requests.

## Description design

The agent normally sees only `name` and `description` during discovery. The
description must carry the activation decision:

1. State what the Skill does.
2. State when to use it with imperative or direct wording.
3. Match user intent, not internal implementation.
4. Include indirect ways users may express the need.
5. Stay specific enough to avoid adjacent-task false positives.

Keep it below the 1,024-character limit. A description should not claim work
the Skill cannot perform.

## Query set

Create about 20 realistic queries:

- 8-10 should-trigger queries.
- 8-10 should-not-trigger queries.
- Include formal, casual, abbreviated, typo-containing, terse, and detailed wording.
- Include prompts that mention the domain indirectly.
- Use near misses that share keywords but need a different Skill.
- Include both simple and multi-step requests.

Example shape:

```json
[
  {"query": "Create a Skill for our release checklist", "should_trigger": true},
  {"query": "Run the existing release Skill", "should_trigger": false}
]
```

## Measurement

Run each query multiple times when possible because activation is nondeterministic.
A useful initial threshold is 0.5 trigger rate:

- Positive query passes when rate is at least the threshold.
- Negative query passes when rate is below the threshold.

Split data into a fixed train set of about 60% and validation set of about 40%.
Use train failures to revise the description; use validation only to select the
best generalizing version. Do not add literal failed-query keywords as a
patch. Generalize the missing intent or boundary instead.

Usually stop after about five useful iterations or when validation stops
improving.
