# scripts/ 目录

<p align="center">
  <samp>
    <strong>中文</strong> ·
    <a href="./README.en.md">English</a>
  </samp>
</p>

可执行脚本目录。本目录当前为空，仅以 `.gitkeep` 占位。

## 按规范，这是什么

Agent Skills 规范（详见上层 `references/specification.md`）约定，skill 目录可含四个标准子目录，其中 `scripts/` 的定位是：

> **scripts/**：可执行代码。agent 可以运行它。脚本应当**自包含或清晰声明依赖**、**给出友好的错误提示**、**优雅处理边界情况**。支持的语言取决于具体 agent 实现，常见的有 Python、Bash、JavaScript。

也就是说，只有当这个 skill 的**执行过程需要跑代码**时，`scripts/` 才会被用到——把经过测试的工具脚本打包进去，让 agent 在激活 skill 后能直接执行。典型例子：

- 数据处理 skill 自带 `scripts/process.py`
- 代码检查 skill 自带 `scripts/lint.sh`
- 表单处理 skill 自带 `scripts/validate_fields.py`

## 为什么 skill-loomery 的 `scripts/` 现在是空的

因为 **skill-loomery 是一个「方法论」skill**：它只教 agent 如何思考、如何组织、如何验证一个 skill，全程不需要运行任何代码。它没有、当前也不需要任何可执行脚本。因此该目录为空——这是**有意的、符合设计**的，而不是遗漏。

## `.gitkeep` 是干什么的

git 不跟踪空目录。为了让你 clone（或别人拉取）之后，`scripts/` 这个目录依然存在于版本库里、结构完整，我们放了一个占位文件 `.gitkeep`。它本身没有任何功能，唯一的作用就是"占住这个目录的坑"。

## 将来什么情况下会往里面放东西

当 skill-loomery 日后需要"可执行"的能力时，才会添加脚本。预计可能的方向（目前都只是设想，尚未实现）：

- `validate-skill.sh` —— 自动校验 SKILL.md 的 frontmatter 是否符合规范
- `run-evals.sh` —— 自动跑 `evals/` 里的测试用例并汇总结果

添加任何一个脚本后，目录就会自动出现内容，届时 `.gitkeep` 即可删除。

## 哪些脚本该放这里

关于"何时该把逻辑写成脚本打包、脚本该怎么设计才适合 agent 调用"，见上层 `references/using-scripts.md`。要点摘录：

- **不交互**（硬要求）：agent 在非交互 shell 里运行，脚本不能阻塞等输入。
- **写 `--help`**：让 agent 能读懂接口。
- **结构化输出**：优先 JSON / CSV / TSV。
- **幂等、`--dry-run`、有意义退出码**。
