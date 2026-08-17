# Optimizing Descriptions — improving the description field

> Source: agentskills.io/skill-creation/optimizing-descriptions (official, distilled)

A skill only helps if it gets activated. The `description` field is the primary mechanism agents use to decide whether to load a skill. Under-specified → won't trigger when it should; over-broad → triggers when it shouldn't.

## 1. How triggering works

Agents use progressive disclosure: at startup they load only each skill's name and description; when a task matches, they read the full SKILL.md. This means **the description carries the entire burden of triggering**.

An important nuance: **agents typically only consult skills for tasks beyond what they can handle alone**. A simple one-step request ("read this PDF") may not trigger even a perfectly matching description, because the agent can do it with basic tools. Specialized knowledge — an unfamiliar API, a domain-specific workflow, an uncommon format — is where a well-written description makes the difference.

## 2. Principles for an effective description

- **Use imperative phrasing**: write "Use this skill when…", not "This skill does…". The agent is deciding whether to act.
- **Focus on user intent, not implementation**: describe what the user wants to achieve, not internal mechanics.
- **Err on the side of pushy**: explicitly list applicable contexts, including when the user doesn't name the domain directly ("even if they don't explicitly mention 'CSV' or 'analysis'").
- **Keep it concise**: a few sentences to a short paragraph. The spec enforces a hard 1024-character limit.

## 3. Designing trigger eval queries

A set of realistic user prompts, labeled should-trigger / should-not-trigger.

`eval_queries.json`:

```json
[
  { "query": "I've got a spreadsheet in ~/data/q4_results.xlsx with revenue in col C and expenses in col D — can you add a profit margin column and highlight anything under 10%?", "should_trigger": true },
  { "query": "whats the quickest way to convert this json file to yaml", "should_trigger": false }
]
```

Aim for about **20 queries: 8–10 should-trigger + 8–10 shouldn't**.

### Should-trigger queries

Vary along several axes:

- **Phrasing**: formal, casual, typos, abbreviations.
- **Explicitness**: some name the domain ("analyze this CSV"), others describe the need without naming it ("my boss wants a chart from this data file").
- **Detail**: terse prompts alongside context-heavy ones (paths, column names, backstory).
- **Complexity**: single-step alongside multi-step workflows, to test whether the agent discerns relevance when the task is buried in a larger chain.

The most useful should-trigger queries are those where the skill helps but the connection isn't obvious from the query alone.

### Should-not-trigger queries

The most valuable negatives are **near-misses** — sharing keywords/concepts but needing something different. They test *precision*, not just *breadth*.

For a CSV analysis skill, **weak negatives** (nearly useless):

- "Write a fibonacci function" — obviously irrelevant.
- "What's the weather today?" — no keyword overlap, too easy.

**Strong negatives**:

- "I need to update the formulas in my Excel budget spreadsheet" — shares "spreadsheet"/"data", but needs Excel editing, not CSV analysis.
- "can you write a python script that reads a csv and uploads each row to our postgres database" — involves CSV, but the task is ETL, not analysis.

### Realism

Real prompts carry context generic queries lack: file paths (`~/Downloads/report_final_v2.xlsx`), personal context ("my manager asked me to…"), specific details (column names, data values), casual language, abbreviations, occasional typos.

## 4. Testing whether a description triggers

Run each query with the skill installed and observe whether the agent invokes it. Make sure the skill is registered and discoverable (varies by client). Most clients provide observability — execution logs, tool-call histories, verbose output → check which skills were consulted.

A query **passes** iff:

- `should_trigger == true` and the skill was invoked; or
- `should_trigger == false` and the skill was not invoked.

### Run multiple times (trigger rate)

Model behavior is nondeterministic — run each query multiple times (**3 is a reasonable start**) and compute the **trigger rate**: the fraction of runs where the skill was invoked.

- should-trigger passes if trigger rate is above a threshold (**0.5 is a reasonable default**).
- should-not-trigger passes if trigger rate is below that threshold.

20 queries × 3 runs = 60 invocations — worth scripting. Skeleton (replace the claude invocation and detection logic with your client's):

```bash
#!/bin/bash
QUERIES_FILE="${1:?Usage: $0 <queries.json>}"
SKILL_NAME="my-skill"
RUNS=3

check_triggered () {
  local query="$1"
  claude -p "$query" --output-format json 2>/dev/null \
    | jq -e --arg skill "$SKILL_NAME" \
        'any(.messages[].content[]; .type == "tool_use" and .name == "Skill" and .input.skill == $skill)' \
    >/dev/null 2>&1
}

count=$(jq length "$QUERIES_FILE")
for i in $(seq 0 $((count - 1))); do
  query=$(jq -r ".[$i].query" "$QUERIES_FILE")
  should_trigger=$(jq -r ".[$i].should_trigger" "$QUERIES_FILE")
  triggers=0
  for run in $(seq 1 $RUNS); do
    check_triggered "$query" && triggers=$((triggers + 1))
  done
  jq -n --arg query "$query" --argjson should_trigger "$should_trigger" \
      --argjson triggers "$triggers" --argjson runs "$RUNS" \
      '{query: $query, should_trigger: $should_trigger, triggers: $triggers, runs: $runs, trigger_rate: ($triggers / $runs)}'
done | jq -s '.'
```

If supported, you can stop a run early once the outcome is clear — saving time and cost.

## 5. Avoiding overfitting with train/validation splits

Optimizing against *all* queries risks a description that works for those specific phrasings but fails on new ones.

- **Train set (~60%)**: used to identify failures and guide improvements.
- **Validation set (~40%)**: held out, only to check whether improvements generalize.

Both sets should contain a proportional mix of should/shouldn't. Shuffle randomly and **keep the split fixed across iterations** for apples-to-apples comparison. Split into `train_queries.json` and `validation_queries.json` and run separately.

## 6. The optimization loop

1. Evaluate the current description on both train and validation sets.
2. Identify train failures: which should-trigger didn't trigger? which shouldn't did?
3. **Only use train failures to guide changes** — keep validation results out of the process, whether you edit yourself or prompt an LLM.
4. Revise toward generalization:
   - should-trigger failing → too narrow; broaden scope or add context about when it's useful.
   - should-not false-triggering → too broad; add specificity about what it *doesn't* do, or clarify the boundary with adjacent capabilities.
5. **Avoid adding specific keywords from failed queries** — that's overfitting. Find the general category they represent and address that.
6. If stuck after several iterations, try a **structurally different framing** rather than incremental tweaks.
7. **Check the description stays under 1024 characters** — it tends to grow.
8. Repeat until all train queries pass or no meaningful improvement.

**Select the best iteration by validation pass rate.** The best may not be the last one — earlier iterations may generalize better than later overfit ones.

**Five iterations is usually enough.** If performance stalls, the problem may be the queries (too easy/hard/poorly labeled), not the description.

> The skill-creator Skill automates this end-to-end.

## 7. Applying the result

- Update `description` in the SKILL.md frontmatter.
- Verify it's under 1024 characters.
- Sanity-check with a few manual prompts; for rigor, run 5–10 fresh queries (should + shouldn't mix) never seen in optimization.

Before / after:

```yaml
# Before
description: Process CSV files.

# After
description: >
  Analyze CSV and tabular data files — compute summary statistics,
  add derived columns, generate charts, and clean messy data. Use this
  skill when the user has a CSV, TSV, or Excel file and wants to
  explore, transform, or visualize the data, even if they don't
  explicitly mention "CSV" or "analysis."
```

The improved description is more specific about *what it does* and broader about *when it applies*.

## Next steps

Once triggering is reliable, evaluate output quality — see evaluating.md.
