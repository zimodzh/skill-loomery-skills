# assets/ 目录

<p align="center">
  <samp>
    <strong>中文</strong> ·
    <a href="./README.en.md">English</a>
  </samp>
</p>

静态资源目录。存放模板、图片、数据文件等 skill 需要的静态素材。

## 按规范，这是什么

Agent Skills 规范（详见上层 `references/specification.md`）约定，skill 目录可含四个标准子目录，其中 `assets/` 的定位是：

> **assets/**：静态资源。包含模板（文档模板、配置模板）、图片（图表、示例）、数据文件（查找表、schema）。

与 `scripts/`（可执行代码）、`references/`（可读文档）不同，`assets/` 放的是**不执行、不直接指导流程、但流程中会用到的东西**。

## 当前内容

- `skill-template.md` —— 最小可用的 SKILL.md 模板。制作新 skill 时，复制此文件到 `<skill-name>/SKILL.md` 再编辑。

## 什么样的东西放这里

- **模板**：文档模板、配置模板、输出格式模板（见 `references/best-practices.md` 的「模板（output format）」一节）。
- **图片**：图表、示意图、示例图。
- **数据文件**：查找表、schema、种子数据。

短模板可以直接内联进 `SKILL.md`；长模板、或只在特定场景才用到的模板，放本目录并由 `SKILL.md` 按需引用——这正是渐进披露（progressive disclosure）的设计意图：只有在需要时才加载，不占用常驻上下文。
