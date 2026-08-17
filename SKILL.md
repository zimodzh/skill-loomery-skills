---
name: skill-loomery
description: Create and refine Agent Skills. A bilingual (中文为主) standard workflow for authoring, optimizing, evaluating, and bundling Agent Skills (SKILL.md format). Use when creating a new skill, improving a skill's description or instructions, setting up evals, or bundling reusable scripts. 创建、优化、评估、打包 Agent Skills 的标准流程——当需要编写/改进 SKILL.md、优化 description、搭建 evals、或打包脚本时使用。
license: MIT
compatibility: Any skills-compatible agent (Claude Code, OpenAI Codex, VS Code Copilot, DSH, etc.). No special system dependencies; optional tools: git, uv, npm.
metadata:
  author: skill-loomery
  version: "1.0.0"
  repo: skill-loomery-skills
---

# Skill Loomery

一套把「经验」固化成一个可复用、可触发、可验证的 **Agent Skill** 的标准流程。当用户要 **创建新 skill、改进已有 skill 的 description/指令、搭建 evals、或打包脚本** 时，按本流程执行。

## 核心流程（按顺序执行）

1. **提取真实经验**（见 references/best-practices.md「从真实经验出发」）
   不要凭 LLM 通用知识空想。从真实任务、项目 artifact、执行 trace 中提炼，而非让 LLM 凭空生成。

2. **圈定范围**：一个 skill 封装一个内聚的「工作单元」——太窄导致要叠加载多个、太宽难精确触发。

3. **搭骨架**：`<name>/` 目录 + `SKILL.md`（必填）+ 可选 `scripts/` `references/` `assets/`。

4. **写 frontmatter**（见 references/specification.md）
   - `name`：≤64 字符，仅小写字母/数字/连字符，不首尾连字符、无连续 `--`，**必须等于父目录名**。
   - `description`：≤1024 字符，同时写清「做什么」+「何时用」，含触发关键词。

5. **写正文**（见 references/best-practices.md）
   分步指令 + gotchas 踩坑清单 + 模板 + 检查清单 + 验证循环。

6. **优化 description**（见 references/optimizing-descriptions.md）
   用 train/validation 触发率评估迭代，直到正确触发。

7. **评估质量**（见 references/evaluating.md）
   用 evals 做 with_skill vs without_skill 基线对比 + 断言。

8. **打包脚本**（见 references/using-scripts.md）
   需要跑代码时，遵循「一次性命令 / 自包含脚本 / 面向 agent 的脚本设计」规范。

9. **校验**：`skills-ref validate ./<skill-name>`（见 references/specification.md「校验」）。

## 铁律（永远遵守）

- SKILL.md **< 500 行、< 5000 token**；细节拆到 `references/` 按需加载（渐进披露）。
- **只写 agent 自己不知道的**；它知道的（如「PDF 是什么」）删掉。
- 文件引用用**从 skill 根目录出发的相对路径**，且只下钻一层。
- 按任务脆弱度校准指令严格度：普通任务给自由度 + 讲理由；脆弱/高风险任务给死命令。
- 面向 agent 的脚本：**不交互**、有 `--help`、结构化输出、幂等、有 `--dry-run`。

## 规范 skill 长这样

```
skill-name/
├── SKILL.md        # 必填：frontmatter + 指令
├── scripts/        # 可选：可执行代码
├── references/     # 可选：按需加载的文档
├── assets/         # 可选：模板/静态资源
└── evals/          # 可选：evals.json + 测试文件
```

最小可用 skill 就一个 `SKILL.md` 文件（见 references/specification.md 的示例）。

## 何时加载哪个参考

| 场景 | 加载 |
|---|---|
| 字段怎么写 / 约束 | references/specification.md |
| 怎么写得好 | references/best-practices.md |
| description 触发不准 | references/optimizing-descriptions.md |
| 怎么验证质量 | references/evaluating.md |
| 要不要跑代码 / 打包脚本 | references/using-scripts.md |
