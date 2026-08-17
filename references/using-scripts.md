# Using Scripts — 在 skill 里跑命令 / 打包脚本

> 来源：agentskills.io/skill-creation/using-scripts（官方精炼）

## 一次性命令（无需打包 scripts/）

已有工具能干的，直接在 SKILL.md 引用：

| 工具 | 例子 | 说明 |
|---|---|---|
| uvx | `uvx ruff@0.8.0 check .` | Python 隔离环境，随 uv 附带，缓存激进 |
| pipx | `pipx run 'black==24.10.0' .` | 成熟备选，包管理器可装 |
| npx | `npx eslint@9 --fix .` | 随 npm/Node 附带，按需下载缓存 |
| bunx | `bunx create-vite@6 my-app` | Bun 环境替代 npx |
| deno run | `deno run npm:create-vite@6 my-app` | 从 URL/specifier 跑，需 permission flags |
| go run | `go run golang.org/x/tools/cmd/goimports@v0.28.0 .` | Go 内置 |

要点：**pin 版本**（可复现）、在 SKILL.md 声明前置条件（"Requires Node.js 18+"，运行时级别用 `compatibility` 字段）、命令复杂到易错时改成打包脚本。

## 从 SKILL.md 引用脚本

用**从 skill 根目录出发的相对路径**。先在 SKILL.md 列出可用脚本，再指示运行：

```markdown
## Available scripts
- **`scripts/validate.sh`** — Validates configuration files
- **`scripts/process.py`** — Processes input data

## Workflow
1. Run: `bash scripts/validate.sh "$INPUT_FILE"`
2. Process: `python3 scripts/process.py --input results.json`
```

## 自包含脚本（依赖内联声明，跑一条命令即可）

- **Python（PEP 723）**：`# /// script` + TOML 声明依赖，`uv run scripts/extract.py`。
- **Deno**：`import from "npm:xx@1.0.0"`，天然自包含。
- **Bun**：import 路径 pin 版本 `"cheerio@1.0.0"`，运行时自动装。
- **Ruby**：`bundler/inline` 直接声明 gem。

## 面向 agent 的脚本设计（关键）

agent 通过 stdout/stderr 决定下一步，所以：

1. **绝不交互**（硬要求）：非交互 shell 无法回答 TTY prompt，会挂死。输入全走 flag / 环境变量 / stdin；缺参数给清晰报错。
2. **写 `--help`**：agent 学习接口的第一途径，含描述 / flags / 示例，要简洁。
3. **友好报错**：说清什么错、期望什么、怎么试（`--format must be json/csv/table. Received: xml`）。
4. **结构化输出**：JSON/CSV/TSV 优先（`jq`/`cut`/`awk` 可消费）。数据写 stdout，诊断写 stderr。
5. **幂等**："create if not exists" 优于"create 重复就报错"。
6. **拒绝歧义输入**：用 enum/封闭集，别猜。
7. **`--dry-run`**：破坏性/有状态操作给预览。
8. **有意义退出码**：不同失败类型不同码，写进 `--help`。
9. **安全默认**：破坏性操作要 `--confirm`/`--force`。
10. **可预测输出大小**：harness 常截断 10–30K；大输出默认给摘要，支持 `--offset` 分页，或强制 `--output FILE`（`-` 显式走 stdout）。
