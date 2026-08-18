# evals/ 目录

<p align="center">
  <samp>
    <strong>中文</strong> ·
    <a href="./README.en.md">English</a>
  </samp>
</p>

description 的触发评测集。用来验证 `skill-loomery` 的 description 是否在正确的 prompt 上触发。

## 文件

- `trigger-queries.json` —— 20 条评测查询：10 条正例（should_trigger = true）+ 10 条负例（should_trigger = false）。

## 评测方法

方法论与 `references/optimizing-descriptions.md` 一致，要点：

1. **跑多次**：模型行为不确定，每条查询跑 ≥3 次，计算**触发率**（skill 被调用的运行占比）。
2. **判定通过**：正例触发率 > 0.5（合理默认阈值）即 pass；负例触发率 < 0.5 即 pass。
3. **train/validation 划分**：优化 description 时，用约 60% 查询指导改动、约 40% 留作验证，避免过拟合；随机打乱并跨迭代保持固定。
4. **记录通过率**：改 description 前后各跑一遍，对比触发率，确认改动泛化。

## 什么时候跑

- 修改 `SKILL.md` 的 description 之前。
- 新增或调整评测查询之后。

## 结构

`trigger-queries.json` 的每一项：

```json
{ "query": "帮我创建一个 skill，用来给团队做代码审查", "should_trigger": true }
```

- `query`：真实用户会说的话。
- `should_trigger`：true = 应该触发 skill-loomery；false = 不该触发。
