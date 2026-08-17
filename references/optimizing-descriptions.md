# Optimizing Descriptions — 优化 description 字段

> 来源：agentskills.io/skill-creation/optimizing-descriptions（官方精炼）

目标：让 skill 在**正确的 prompt 上触发**，在错误的 prompt 上不触发。

## 评估触发率的数据集

- **train set**：用来改进 description 的查询集。
- **validation set**：从不参与优化、只用来挑最佳版本。

查询分三类：should-trigger / coincidental（偶然可触发但不该）/ should-not-trigger，标注 pass/fail。

## 迭代循环

1. 从真实用户常见说法收集查询（含拼写、格式变化）。
2. 跑评估脚本测当前 description 的触发率。
3. 基于 pass/fail 改进 description（有时换措辞比微调更有效）。
4. 确保 description 控制在 **1024 字符内**（优化过程会膨胀）。
5. 重复 1–4 直到 train 全过或无改进。
6. 用 **validation pass rate** 选最佳——最佳未必是最后一版（后几版可能过拟合 train）。

**通常 5 轮足够**。不涨 → 问题可能在查询集（太易/太难/标注差）而非 description。

> skill-creator 元技能可端到端自动化此循环。

## 落地

- 更新 SKILL.md frontmatter 的 `description`。
- 检查 <1024 字符。
- 手测几个 prompt 做 sanity check；更严则用 5–10 条全新查询（should + should-not 混合）过评估脚本验证泛化。

## 前后对比示例

❌ Before：

```yaml
description: Process CSV files.
```

✅ After：

```yaml
description: >
  Analyze CSV and tabular data files — compute summary statistics,
  add derived columns, generate charts, and clean messy data. Use this
  skill when the user has a CSV, TSV, or Excel file and wants to
  explore, transform, or visualize the data, even if they don't
  explicitly mention "CSV" or "analysis."
```

要点：更具体地写「做什么」，更宽地写「何时适用」。
