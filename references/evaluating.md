# Evaluating — 评估 skill 输出质量

> 来源：agentskills.io/skill-creation/evaluating-skills（官方精炼）

"跑了一次觉得行" ≠ 可靠。用结构化 eval 回答：跨不同 prompt / 边界 / 比没有 skill 更好吗？

## 设计测试用例

每例三部分：
- **prompt**：真实用户会说的话。
- **expected_output**：成功长啥样的（人读）描述。
- **files（可选）**：需要的输入文件。

存到 `evals/evals.json`：

```json
{
  "skill_name": "csv-analyzer",
  "evals": [
    {
      "id": 1,
      "prompt": "I have a CSV of monthly sales data in data/sales_2025.csv. Can you find the top 3 months by revenue and make a bar chart?",
      "expected_output": "A bar chart image showing the top 3 months by revenue, with labeled axes and values.",
      "files": ["evals/files/sales_2025.csv"]
    }
  ]
}
```

写 prompt 要点：先 2–3 例起步、措辞多样化（随意 vs 精确）、至少一个边界用例、用真实语境（文件路径/列名）。

## 跑 eval

核心：每例跑**两次**（with skill vs without skill 基线）。

工作区结构（`<skill>-workspace/iteration-N/eval-*/{with_skill,without_skill}/`），机器产出的 `grading.json` / `timing.json` / `benchmark.json` 由 agent/脚本/你生成，只有 `evals.json` 手写。

每次跑要**干净上下文**（无残留状态）——用 subagent 天然隔离，否则开独立会话。

改良已有 skill：先 `cp -r` 快照旧版作基线，跑 `old_skill/`。

## 记录 timing（token / 耗时）

```json
{ "total_tokens": 84852, "duration_ms": 23332 }
```

（Claude Code subagent 完成通知里带这两个值，及时保存。）

## 写 assertions

可验证断言，先看第一轮输出再补。好断言：

- "输出文件是合法 JSON" —— 可程序验证
- "柱状图有带标签的坐标轴" —— 具体可观察
- "提到三个具体月份" —— 明确

坏断言：主观（"输出 helpful"）。

## 分级与迭代

用 assertions 自动判 pass/fail，汇总 `benchmark.json`。读失败 trace 定位改进点，改 skill 后重跑。核心即「基线对比 + 断言 + 迭代」。
