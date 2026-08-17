# references/ 目录

<p align="center">
  <samp>
    <strong>中文</strong> ·
    <a href="./README.en.md">English</a>
  </samp>
</p>

按需加载的参考文档目录。这里是 skill-loomery 的知识库主体——SKILL.md 只保留核心流程和铁律，细节都在这里，agent 按需查阅。

## 目录索引

| 文件 | 讲什么 | 何时读 |
|---|---|---|
| `quickstart.md` | Agent Skill 的定义 + Discovery→Activation→Execution 三阶段 + roll-dice 最小完整示例 | 想快速理解"skill 是什么、长什么样" |
| `specification.md` | SKILL.md 的完整格式规范（frontmatter 字段、约束、目录约定、渐进披露、校验） | 查字段怎么写、约束是什么 |
| `best-practices.md` | 写好 skill 的实践（真实经验、省上下文、内聚单元、控制强度、指令模式） | 怎么把 skill 写得好 |
| `optimizing-descriptions.md` | 优化 description 触发（触发机制、写作原则、eval 查询、train/validation、优化循环） | description 触发不准 |
| `evaluating.md` | 评估输出质量（测试用例、评分、汇总、模式分析、人工审核、迭代） | 验证 skill 是否真的有效 |
| `using-scripts.md` | 跑命令 / 打包脚本（一次性命令、自包含脚本、面向 agent 的脚本设计） | 要不要跑代码、怎么打包脚本 |

## 阅读顺序建议

- **第一次接触**：先读 `quickstart.md`，建立整体认知。
- **开始动手写**：`specification.md`（格式约束）+ `best-practices.md`（写法）。
- **写完调优**：`optimizing-descriptions.md`（触发）+ `evaluating.md`（质量）。
- **需要代码**：`using-scripts.md`。

这个顺序就是 SKILL.md「核心流程」第 4–8 步的展开。
