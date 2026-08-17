# Using Scripts — running commands and bundling scripts

> Source: agentskills.io/skill-creation/using-scripts (official, distilled)

## One-off commands (no scripts/ needed)

When an existing package already does the job, reference it directly in SKILL.md:

| Tool | Example | Notes |
|---|---|---|
| uvx | `uvx ruff@0.8.0 check .` | Isolated Python envs, ships with uv, aggressive caching |
| pipx | `pipx run 'black==24.10.0' .` | Mature alternative, installable via OS package managers |
| npx | `npx eslint@9 --fix .` | Ships with npm/Node, downloads on demand |
| bunx | `bunx create-vite@6 my-app` | Bun's equivalent of npx |
| deno run | `deno run npm:create-vite@6 my-app` | Runs from URLs/specifiers, needs permission flags |
| go run | `go run golang.org/x/tools/cmd/goimports@v0.28.0 .` | Built into Go |

Key points: **pin versions** for reproducibility; state prerequisites in SKILL.md ("Requires Node.js 18+", runtime-level via the `compatibility` field); move complex commands into tested scripts.

## Referencing scripts from SKILL.md

Use relative paths from the skill root. List available scripts, then instruct the agent to run them:

```markdown
## Available scripts
- **`scripts/validate.sh`** — Validates configuration files
- **`scripts/process.py`** — Processes input data

## Workflow
1. Run: `bash scripts/validate.sh "$INPUT_FILE"`
2. Process: `python3 scripts/process.py --input results.json`
```

## Self-contained scripts (inline dependency declarations)

Bundle a script that declares its own dependencies inline — run with a single command, no separate manifest/install step.

- **Python (PEP 723)**: `# /// script` + TOML dependency block; `uv run scripts/extract.py`.
- **Deno**: `import from "npm:xx@1.0.0"` — self-contained by default.
- **Bun**: `import from "cheerio@1.0.0"` — auto-installs at runtime.
- **Ruby**: `bundler/inline` declares gems directly.

## Designing scripts for agentic use (critical)

The agent reads stdout/stderr to decide what to do next, so:

1. **Never interactive** (hard requirement): agents run in non-interactive shells — a script that blocks on a TTY prompt hangs forever. Take all input via flags / env vars / stdin; give a clear error on missing arguments.
2. **Write `--help`**: the primary way an agent learns the interface — brief description, flags, usage examples.
3. **Helpful error messages**: say what went wrong, what was expected, what to try (`--format must be json/csv/table. Received: xml`).
4. **Structured output**: prefer JSON/CSV/TSV (consumable by `jq`/`cut`/`awk`). Data to stdout, diagnostics to stderr.
5. **Idempotent**: agents may retry — "create if not exists" beats "create and fail on duplicate".
6. **Reject ambiguous input**: use enums / closed sets, don't guess.
7. **`--dry-run`**: for destructive/stateful operations.
8. **Meaningful exit codes**: distinct codes per failure type, documented in `--help`.
9. **Safe defaults**: destructive operations require `--confirm`/`--force`.
10. **Predictable output size**: harnesses truncate ~10–30K; default to a summary, support `--offset` pagination, or require `--output FILE` (`-` to opt into stdout).
