# Evaluating — assessing skill output quality

> Source: agentskills.io/skill-creation/evaluating-skills (official, distilled; all mechanisms preserved)

"It worked once" ≠ reliable. Structured evals answer: does it hold across varied prompts? In edge cases? Better than no skill at all?

## 1. Designing test cases

Each test case has three parts:

- **prompt**: a realistic user message — what someone would actually type.
- **expected_output**: a human-readable description of what success looks like.
- **files (optional)**: input files the skill needs.

Store in `evals/evals.json` inside the skill directory:

```json
{
  "skill_name": "csv-analyzer",
  "evals": [
    {
      "id": 1,
      "prompt": "I have a CSV of monthly sales data in data/sales_2025.csv. Can you find the top 3 months by revenue and make a bar chart?",
      "expected_output": "A bar chart image showing the top 3 months by revenue, with labeled axes and values.",
      "files": ["evals/files/sales_2025.csv"]
    },
    {
      "id": 2,
      "prompt": "there's a csv in my downloads called customers.csv, some rows have missing emails — can you clean it up and tell me how many were missing?",
      "expected_output": "A cleaned CSV with missing emails handled, plus a count of how many were missing.",
      "files": ["evals/files/customers.csv"]
    }
  ]
}
```

Tips for writing prompts:

- **Start with 2–3 cases.** Don't over-invest before the first round; expand later.
- **Vary the prompts**: phrasings, detail level, formality. Casual ("hey can you clean up this csv") alongside precise ("Parse the CSV at data/input.csv, drop rows where column B is null, and write the result to data/output.csv").
- **Cover edge cases**: at least one boundary condition — malformed input, unusual request, ambiguous instructions.
- **Use realistic context**: file paths, column names, personal context. "process this data" is too vague.

Don't define pass/fail checks yet — just prompts and expected outputs. Add detailed checks (assertions) after the first run.

## 2. Running evals

Run each test case **twice**: once with the skill, once without (or with a previous version) → a baseline.

### Workspace structure

Organize eval results in a workspace directory alongside the skill. Each pass gets `iteration-N/`; within it, each test case gets an eval directory with `with_skill/` and `without_skill/` subdirectories:

```
csv-analyzer/
└── evals/
    └── evals.json

csv-analyzer-workspace/
└── iteration-1/
    ├── eval-top-months-chart/
    │   ├── with_skill/
    │   │   ├── outputs/       # files produced by the run
    │   │   ├── timing.json    # tokens and duration
    │   │   └── grading.json   # assertion results
    │   └── without_skill/
    │       ├── outputs/
    │       ├── timing.json
    │       └── grading.json
    ├── eval-clean-missing-emails/
    ...
    └── benchmark.json         # aggregated statistics
```

**Only `evals.json` is authored by hand**; `grading.json` / `timing.json` / `benchmark.json` are produced during the process.

### Spawning runs

Each run must start with a **clean context** — no leftover state — so the agent follows only the SKILL.md. Subagent-capable environments (Claude Code) isolate naturally; otherwise use a separate session per run.

Provide for each run: skill path (or none for baseline), test prompt, input files, output directory. Example with-skill instruction:

```
Execute this task:
- Skill path: /path/to/csv-analyzer
- Task: I have a CSV of monthly sales data in data/sales_2025.csv.
  Can you find the top 3 months by revenue and make a bar chart?
- Input files: evals/files/sales_2025.csv
- Save outputs to: csv-analyzer-workspace/iteration-1/eval-top-months-chart/with_skill/outputs/
```

Baseline: same prompt, no skill path, save to `without_skill/outputs/`.

**Improving an existing skill**: snapshot the old version first (`cp -r <skill-path> <workspace>/skill-snapshot/`), point the baseline there, save to `old_skill/outputs/`.

### Capturing timing data

Compare time/tokens against baseline — a skill that triples tokens is a different trade-off than one that's both better and cheaper. Record after each run:

```json
{ "total_tokens": 84852, "duration_ms": 23332 }
```

In Claude Code, the subagent completion notification carries these; **save immediately** (not persisted elsewhere).

## 3. Writing assertions

Verifiable statements about what the output should contain/achieve. Add after the first round.

**Good assertions:**

- "The output file is valid JSON" — programmatically verifiable.
- "The bar chart has labeled axes" — specific and observable.
- "The report includes at least 3 recommendations" — countable.

**Weak assertions:**

- "The output is good" — too vague to grade.
- "The output uses exactly the phrase 'Total Revenue: $X'" — too brittle.

Not everything needs an assertion. Writing style, visual design, "feels right" are better caught in **human review**. Reserve assertions for objectively checkable things.

Add assertions to each test case:

```json
{
  "files": ["evals/files/sales_2025.csv"],
  "assertions": [
    "The output includes a bar chart image file",
    "The chart shows exactly 3 months",
    "Both axes are labeled",
    "The chart title or caption mentions revenue"
  ]
}
```

## 4. Grading outputs

Grade = judge each assertion against the actual outputs, recording PASS/FAIL with **specific evidence** (quote/reference the output, not an opinion).

Simplest approach: give outputs + assertions to an LLM to evaluate. For assertions checkable by code (valid JSON, row count, file dimensions), use a **verification script** — more reliable than LLM judgment and reusable.

`grading.json`:

```json
{
  "assertion_results": [
    { "text": "The output includes a bar chart image file", "passed": true, "evidence": "Found chart.png (45KB) in outputs directory" },
    { "text": "The chart shows exactly 3 months", "passed": true, "evidence": "Chart displays bars for March, July, and November" },
    { "text": "Both axes are labeled", "passed": false, "evidence": "Y-axis is labeled 'Revenue ($)' but X-axis has no label" },
    { "text": "The chart title or caption mentions revenue", "passed": true, "evidence": "Chart title reads 'Top 3 Months by Revenue'" }
  ],
  "summary": { "passed": 3, "failed": 1, "total": 4, "pass_rate": 0.75 }
}
```

### Grading principles

- **Require concrete evidence for a PASS.** No benefit of the doubt. If an assertion says "includes a summary" but the output has a "Summary" section with one vague sentence, that's a **FAIL** — label present, substance absent.
- **Review the assertions themselves**, not just results. Notice when assertions are too easy (always pass), too hard (always fail), or unverifiable. Fix them next iteration.

### Blind comparison

For comparing two skill versions, present both outputs to an LLM judge **without revealing which is which**. The judge scores holistic qualities (organization, formatting, usability, polish) free of bias. This complements assertion grading: both outputs may **pass all assertions** yet differ significantly in overall quality.

## 5. Aggregating results

Once every run is graded, compute per-configuration summary statistics into `benchmark.json`:

```json
{
  "run_summary": {
    "with_skill": {
      "pass_rate": { "mean": 0.83, "stddev": 0.06 },
      "time_seconds": { "mean": 45.0, "stddev": 12.0 },
      "tokens": { "mean": 3800, "stddev": 400 }
    },
    "without_skill": {
      "pass_rate": { "mean": 0.33, "stddev": 0.10 },
      "time_seconds": { "mean": 32.0, "stddev": 8.0 },
      "tokens": { "mean": 2100, "stddev": 300 }
    },
    "delta": { "pass_rate": 0.50, "time_seconds": 13.0, "tokens": 1700 }
  }
}
```

**The delta tells you what the skill costs and what it buys.** +13 seconds for +50 percentage points of pass rate is probably worth it; doubling tokens for +2 points may not be.

**stddev is only meaningful with multiple runs per eval.** In early iterations (2–3 cases, single runs), focus on raw pass counts and the delta.

## 6. Analyzing patterns

Aggregate statistics hide important patterns. After computing benchmarks:

- **Remove assertions that always pass in both configurations.** They tell you nothing; they inflate the with-skill pass rate without reflecting real value.
- **Investigate assertions that always fail in both.** Either the assertion is broken, the test case is too hard, or it checks the wrong thing. Fix before next iteration.
- **Study assertions that pass with the skill but fail without.** That's where the skill adds value. Understand which instructions/scripts made the difference.
- **Tighten instructions when results are inconsistent across runs.** High stddev means flaky evals or ambiguous instructions; add examples or specificity.
- **Check time/token outliers.** A 3× slower eval → read its execution transcript to find the bottleneck.

## 7. Reviewing with a human

Assertion grading + pattern analysis only check what you thought to assert. A human catches the unanticipated, the "technically correct but misses the point", and things hard to express as pass/fail. **For each test case, review the actual outputs alongside the grades.**

Record specific feedback per case into `feedback.json`:

```json
{
  "eval-top-months-chart": "The chart is missing axis labels and the months are in alphabetical order instead of chronological.",
  "eval-clean-missing-emails": ""
}
```

"Missing axis labels" is actionable; "looks bad" is not. Empty feedback = that case passed review. Focus iteration improvements on cases with specific complaints.

## 8. Iterating on the skill

After grading + review, three signal sources:

- **Failed assertions** → specific gaps: a missing step, an unclear instruction, an unhandled case.
- **Human feedback** → broader quality issues: wrong approach, poor structure, technically-correct-but-unhelpful.
- **Execution transcripts** → *why* things went wrong: ambiguous instructions, wasted steps.

Most effective: **give all three + the current SKILL.md to an LLM and ask it to propose changes**. Include these guidelines when prompting:

- **Generalize from feedback.** Fix underlying issues broadly, not narrow patches for specific examples.
- **Keep the skill lean.** Fewer, better instructions beat exhaustive rules. If transcripts show waste, remove instructions; if pass rates plateau, the skill may be over-constrained — try removing instructions.
- **Explain the why.** Reasoning-based instructions ("Do X because Y tends to cause Z") beat rigid directives.
- **Bundle repeated work.** If every run rewrites a similar helper script, bundle it into scripts/ (see using-scripts.md).

### The loop

1. Give eval signals + current SKILL.md to an LLM to propose improvements.
2. Review and apply changes.
3. Rerun all cases in a new `iteration-<N+1>/` directory.
4. Grade and aggregate.
5. Review with a human. Repeat.

### Stop conditions

Stop when you're satisfied, feedback is consistently empty, or no meaningful improvement between iterations.

> The skill-creator Skill automates much of this: running evals, grading, aggregating, presenting for review.
