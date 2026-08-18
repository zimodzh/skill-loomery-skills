# Skill Loomery

<p align="center">
  <samp>
    <strong>中文</strong> ·
    <a href="./README.en.md">English</a>
  </samp>
</p>

<p align="center">
  <img src="https://img.shields.io/github/repo-size/zimodzh/skill-loomery-skills?style=flat-square" alt="repo size" />
  <img src="https://img.shields.io/github/last-commit/zimodzh/skill-loomery-skills?style=flat-square" alt="last commit" />
  <img src="https://img.shields.io/github/license/zimodzh/skill-loomery-skills?style=flat-square" alt="MIT license" />
  <img src="https://img.shields.io/badge/Agent_Skills-compliant-4D6BFE?style=flat-square" alt="Agent Skills compliant" />
</p>

一个「**制作 Skill 的 Skill**」——把"如何写一个高效、可触发、可验证的 Agent Skill"的完整方法论，固化成可被智能体按需加载的开放格式技能。

## 这是什么

`skill-loomery` 是一个 [Agent Skills](https://agentskills.io) 格式的技能（一个含 `SKILL.md` 的文件夹）。加载它之后，任何兼容的 agent（Claude Code / OpenAI Codex / VS Code Copilot 等）都会获得一套标准流程，用来：

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
├── SKILL.md                        # skill 入口：frontmatter + 核心流程 + 铁律
├── scripts/                        # 可执行脚本（说明见 scripts/README.md）
│   └── .gitkeep                    # 占位文件，使空目录被 git 跟踪
├── references/                     # 按需加载的参考文档（渐进披露，索引见 references/README.md）
│   ├── quickstart.md                # skill 定义 + 三阶段 + roll-dice 最小示例
│   ├── specification.md            # SKILL.md 格式规范
│   ├── best-practices.md           # 写好 skill 的实践
│   ├── optimizing-descriptions.md  # 优化 description 触发
│   ├── evaluating.md               # evals 评估输出质量
│   ├── using-scripts.md            # 跑命令 / 打包脚本
│   ├── README.md                   # 本目录索引（中文）
│   └── README.en.md                # 本目录索引（英文）
├── assets/                         # 静态资源（说明见 assets/README.md）
│   └── skill-template.md           # 最小 SKILL.md 模板
├── evals/                          # description 触发评测集与评测方法
│   ├── trigger-queries.json        # 触发回归评测集
│   ├── README.md                   # 评测方法（中文）
│   └── README.en.md                # 评测方法（英文）
├── README.md                       # 本文件（中文）
├── README.en.md                    # 英文版
├── LICENSE                         # MIT
└── .gitignore
```

## 安装

技能名为 `skill-loomery`，Agent Skills 规范要求所在文件夹同名；本仓库名为 `skill-loomery-skills`。克隆时直接指定目标文件夹名即可一步到位：

```bash
git clone https://github.com/zimodzh/skill-loomery-skills.git <你的 skill 目录>/skill-loomery
```

`<你的 skill 目录>` 换成你所用的 agent 的技能目录。官方文档（Quickstart）给出的示例是 VS Code：项目下的 `.agents/skills/`。Agent Skills 是开放格式，同一技能在其它兼容 agent（Claude Code、OpenAI Codex 等）中也能工作——放到对应 agent 的技能目录即可。

验证：向 agent 提问"帮我创建一个 skill"或"优化我这个 skill 的 description"，技能应被触发。

## 示例

最小完整示例见 `references/quickstart.md`——官方文档的 `roll-dice` 技能（一个含 name + description + 可执行正文的 20 行内 `SKILL.md`）。起步骨架见 `assets/skill-template.md`，复制为 `<name>/SKILL.md` 再编辑即可。

## 触发评测

`evals/trigger-queries.json` 是 description 的回归评测集（正例 + 负例）。修改 description 前请先跑评测并记录通过率；方法论（含多次运行触发率、train/validation 划分、防过拟合）见 `evals/README.md`，与 `references/optimizing-descriptions.md` 一致。

## 来源与版本对应

内容蒸馏自 [agentskills.io](https://agentskills.io) 官方文档，覆盖其中全部七篇：Overview、Specification、Quickstart、Best practices、Optimizing descriptions、Evaluating、Using scripts。技能内容与官方文档不一致时，**以官方文档为准**。发现偏差欢迎提 issue 或 PR。

## 范围边界

覆盖**创建、优化、评估、打包 Agent Skill 的完整方法论与格式规范**。不覆盖某个具体 agent 的安装部署与运行环境细节——那些由各 agent 自己的文档负责。

## 维护与贡献

- 更新任何 `references/` 前，先核对官方文档对应页面，并注明来源。
- 遵守 Agent Skills 约束：name 为 kebab-case 且与目录一致；description ≤ 1024 字符；正文渐进式披露。
- 欢迎 PR：修正、更多示例、扩充评测集、其它语言版本。

## License

[MIT](./LICENSE)
