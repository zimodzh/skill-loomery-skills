# Specification — SKILL.md 格式规范

> 来源：agentskills.io/specification（官方规范精炼）

## 目录结构

```
skill-name/
├── SKILL.md    # 必填：metadata + 指令
├── scripts/    # 可选：可执行代码
├── references/ # 可选：按需加载的文档
├── assets/     # 可选：模板/静态资源
└── ...         # 任意其他文件/目录
```

## SKILL.md = YAML frontmatter + Markdown 正文

### frontmatter 字段

| 字段 | 必填 | 约束 |
|---|---|---|
| name | 是 | ≤64 字符；仅小写字母数字与连字符 `a-z 0-9 -`；不以 `-` 开头或结尾；不含连续 `--`；**必须等于父目录名** |
| description | 是 | 1–1024 字符；写「做什么」+「何时用」；含关键词 |
| license | 否 | 许可证名，或捆绑 license 文件的引用 |
| compatibility | 否 | ≤500 字符；环境要求（目标产品 / 系统包 / 网络等） |
| metadata | 否 | string→string 任意映射，键名尽量唯一防冲突 |
| allowed-tools | 否 | 空格分隔的预授权工具串（Experimental） |

### 最小示例

```yaml
---
name: skill-name
description: A description of what this skill does and when to use it.
---
```

### 带可选字段示例

```yaml
---
name: pdf-processing
description: Extract PDF text, fill forms, merge files. Use when handling PDFs.
license: Apache-2.0
metadata:
  author: example-org
  version: "1.0"
---
```

### name 详规

- 1–64 字符
- 仅 unicode 小写字母数字 + 连字符
- 不以连字符开头/结尾
- 无连续连字符
- 等于父目录名

合法：`data-analysis`、`code-review`
非法：`PDF-Processing`（大写）、`-pdf`（开头连字符）、`pdf--processing`（连续连字符）

### description 详规

好（具体 + 触发词 + 写清何时用）：

```
Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.
```

差（太笼统）：

```
Helps with PDFs.
```

### 正文（Markdown body）

无格式限制。推荐小节：分步指令、输入输出示例、常见边界情况。

> ⚠️ agent 激活后**一次性加载整个正文**，长内容应拆到 references/。

## 可选目录约定

- **scripts/**：可执行代码。应自包含或写明依赖、给友好错误、优雅处理边界。
- **references/**：按需读取的文档（REFERENCE.md、FORMS.md、领域文件）。保持每个文件聚焦。
- **assets/**：静态资源（模板、图片、数据文件）。

## 渐进披露（progressive disclosure）

三层加载：

1. **Metadata（~100 token）**：name + description 在启动时全量加载。
2. **Instructions（建议 <5000 token）**：激活后加载 SKILL.md 正文。
3. **Resources（按需）**：scripts/references/assets 里的文件只在需要时加载。

保持 SKILL.md <500 行，细节移 references/。

## 文件引用

用**从 skill 根目录出发的相对路径**：

```markdown
See [the reference guide](references/REFERENCE.md) for details.
Run the extraction script: scripts/extract.py
```

引用只深一层，避免深嵌套链。

## 校验

```bash
skills-ref validate ./my-skill
```

检查 frontmatter 合法性与命名规范。
