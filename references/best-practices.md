# Best Practices — 写好 Skill 的实践

> 来源：agentskills.io/skill-creation/best-practices（官方最佳实践精炼）

## 核心心法：从真实经验出发，别让 LLM 空想

最常见坑：直接让 LLM 生成 skill，不喂领域上下文 → 产出空话（"handle errors appropriately"）。有效 skill 扎根真实经验。

**三条来源：**

1. **从亲手做过的任务提取**：在对话里真实完成一个任务（给上下文 / 纠正 / 偏好），再抽出可复用模式。关注：
   - 奏效的步骤序列
   - 你纠正过的地方（"用 X 库别用 Y"、"注意边界 Z"）
   - 输入输出格式
   - 你补充过的项目背景
2. **从现有项目 artifact 合成**：喂内部文档 / runbook / style guide、API 规范 / schema / 配置、code review 评论、版本历史（尤其 patch / fix）、真实故障与解决 → 让 LLM 合成。
3. **用真实执行打磨**：拿真实任务跑 skill，把**所有**结果（不只失败）喂回去重写。读执行 trace 而非只看最终输出。

## 省上下文（token）

激活后 SKILL.md 全部进 context，每个 token 都在抢注意力。

- **只加 agent 不知道的**：项目约定、领域流程、非显然边界、具体工具 API。不用解释「PDF 是什么」。
- 每条内容自问：**「没有这条，agent 会不会做错？」** 不会 → 删。

## 设计内聚单元

- 太窄 → 单个任务要叠加载多个 skill；太宽 → 难精确触发。
- 例：「查库 + 格式化结果」是一个内聚单元；「再加库管理」就塞太多了。

## 适度详情，别求全

过度全面反而有害——agent 难提取重点、可能被不适用的指令带偏。简洁分步 + 一个工作示例 > 穷举文档。

## 大 skill 用渐进披露拆分

SKILL.md 只留每次都要的核心指令，其余移 references/。**关键：告诉 agent 何时加载哪个文件**（"API 返回非 200 时读 references/api-errors.md"），而不是笼统写"见 references/"。

## 校准控制强度（match specificity to fragility）

- **给自由度**（多种做法都 OK、任务容错）：讲「为什么」比死命令更有效。
- **死命令**（脆弱、要求一致、指定顺序）：如数据库迁移——"精确按此序列执行，不许改命令"。
- 大多数 skill 是混合的，逐部分校准。

### 给默认值，别给菜单
多个工具可选时：选一个默认 + 简述备选，别平铺成菜单。

### 教过程，别给单一答案
skill 应教**一类问题怎么做**，而非某个具体实例的结果。

- 差：`orders join customers on customer_id, WHERE region='EMEA', SUM amount`（只对该任务有用）
- 好：读 schema → 按 `_id` 约定 join → 应用 WHERE → 聚合 → 输出 markdown 表（通用方法）

## 高频指令模式（按需取用）

### Gotchas 踩坑清单（最高价值）
环境特有、反直觉的事实——不是泛泛建议，而是"不告诉它就做错"的具体纠正：

```markdown
## Gotchas
- users 表软删除，查询必须带 WHERE deleted_at IS NULL
- 用户 ID 在 DB 叫 user_id、auth 叫 uid、账单 API 叫 accountId，指向同一个值
- /health 只要 web server 在跑就 200，DB 挂了也 200；查 /ready
```

**agent 每次犯错被你纠正，就把纠正加进 gotchas**（最直接的迭代方式）。

### 模板（output format）
要特定输出格式 → 给模板（agent 对具体结构 pattern-match 更稳）。短模板内联，长模板放 assets/ 按需引用。

### 检查清单（checklist）
多步骤 + 有依赖/校验门 → 用 `- [ ]` 清单帮 agent 跟踪不跳步。

### 验证循环（validation loops）
做 → 跑校验器（脚本/参考清单/自查）→ 修 → 重跑，直到通过。参考文档也能当"校验器"。

### plan-validate-execute
批量/破坏性操作：先产出结构化中间计划 → 对照真源校验 → 才执行。

### 打包可复用脚本
agent 每次重复造同样逻辑（画图/解析格式/校验输出）→ 写一次、测试好、打包进 scripts/（详见 using-scripts.md）。
