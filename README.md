# skill-loomery

<p align="center">  <samp>
    <strong>中文</strong> ·
    <a href="./README.en.md">English</a>
  </samp>
</p>

把需求、项目资料和真实工作流程，编织成可复用、可验证的 Agent Skill。

## 能做什么

- 从需求、文档、代码库、运行手册和实际任务中提炼 Skill。
- 生成符合 Agent Skills 规范的 `SKILL.md` 和目录结构。
- 设计 Skill 的触发描述、适用边界和默认流程。
- 按需组织 `references/`、`scripts/` 和 `assets/`。
- 使用 `skills-ref validate` 验证格式。
- 设计触发测试、输出测试和 with/without Skill 对比。
- 根据失败断言、执行记录和人工反馈迭代 Skill。

## 目录

```text
skill-loomery/
├── SKILL.md
├── references/
│   ├── specification.md
│   ├── authoring.md
│   ├── triggering.md
│   ├── output-evaluation.md
│   └── script-design.md
├── assets/
│   ├── README.md
│   ├── README.en.md
│   └── evals-template.json
└── evals/
    ├── README.md
    ├── README.en.md
    └── evals.json
```

## 使用

将本目录放入兼容 Agent Skills 的目录，然后提出类似请求：

```text
根据这份发布手册创建一个 Skill，要求保留步骤顺序并带验证流程。
```

验证本 Skill：

```bash
skills-ref validate ./skill-loomery
```

如果本地没有 `skills-ref`，按 `references/specification.md` 手动检查。

## 设计原则

- 先看真实资料，再写通用流程。
- 主 `SKILL.md` 保持短小，详细内容按需加载。
- 默认方案优先，不堆砌无关选项。
- 具体事实、边界和踩坑优先于泛泛而谈的建议。
- 没有真实需求或可复用经验时，不强行创建 Skill。
- 不为重复逻辑之外的场景添加脚本或依赖。

## 规范来源

- [Agent Skills Specification](https://agentskills.io/specification)
- [Best practices for skill creators](https://agentskills.io/skill-creation/best-practices)
- [Optimizing skill descriptions](https://agentskills.io/skill-creation/optimizing-descriptions)
- [Evaluating skill output quality](https://agentskills.io/skill-creation/evaluating-skills)
- [Using scripts in skills](https://agentskills.io/skill-creation/using-scripts)

## 许可证

[MIT License](./LICENSE)
