# Skill Loomery

**语言 / Language**：[简体中文](#) · [English](README.en.md)

一个「**制作 Skill 的 Skill**」——把"如何写一个高效、可触发、可验证的 Agent Skill"的完整方法论，固化成可被智能体按需加载的开放格式技能。

> 尊严自持，标准先行。开源只是顺手，给好东西一个家——但永远不会因为迁就别人而改变自己的标准。

## 这是什么

`skill-loomery` 是一个 [Agent Skills](https://agentskills.io) 格式的技能（一个含 `SKILL.md` 的文件夹）。加载它之后，任何兼容的 agent（Claude Code / OpenAI Codex / VS Code Copilot / DSH 等）都会获得一套标准流程，用来：

- **创建**新的 skill
- **优化** skill 的 description（触发准确性）
- **评估** skill 的输出质量（evals）
- **打包**可复用脚本

## 为什么做这个

不再靠"翻一堆网页 + 凭感觉写"。把写 skill 这件事标准化：**有什么字段、怎么约束、怎么写得好、怎么验证它真的有效、怎么打包脚本**——全部沉淀成一条可复现的流水线。

这个仓库就是把这条流水线本身，做成一个符合规范的 skill。**狗粮自食**：skill-loomery 就是用 skill-loomery 的规范打造的。

## 目录结构

```
skill-loomery/
├── SKILL.md                      # skill 入口：frontmatter + 核心流程 + 铁律
├── references/                   # 按需加载的参考文档（渐进披露）
│   ├── specification.md          # SKILL.md 格式规范
│   ├── best-practices.md         # 写好 skill 的实践
│   ├── optimizing-descriptions.md# 优化 description 触发
│   ├── evaluating.md             # evals 评估输出质量
│   └── using-scripts.md          # 跑命令 / 打包脚本
├── assets/
│   └── skill-template.md         # 最小 SKILL.md 模板
├── README.md                     # 本文件（中文）
├── README.en.md                  # 英文版
└── LICENSE                       # MIT
```

## 快速开始

1. 把本仓库放到你的 skill 目录（VS Code 默认 `.agents/skills/`，DSH 默认 `~/.agents/skills/`）。
2. 在 agent 里说：「帮我创建一个 skill，用来……」，或「优化我这个 skill 的 description」。
3. agent 会触发 skill-loomery，按 `SKILL.md` 的流程执行。

## 规范依据

完整格式规范以 [Agent Skills Specification](https://agentskills.io/specification) 为准，本仓库的 `references/specification.md` 是其精炼。

## License

[MIT](./LICENSE)
