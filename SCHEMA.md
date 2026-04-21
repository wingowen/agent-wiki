# Wiki Schema

## Domain
AI Agent 研究 — 构建、评估和优化 AI Agent 的技术、框架和最佳实践

## Conventions
- 文件名: 小写、连字符、无空格 (如 `transformer-architecture.md`)
- 每个 wiki 页面以 YAML frontmatter 开头 (见下方)
- 使用 `[[wikilinks]]` 连接页面 (每页至少 2 个出站链接)
- 更新页面时必须更新 `updated` 日期
- 每个新页面必须添加到 `index.md` 正确章节
- 每个操作必须追加到 `log.md`

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
---
```

## Tag Taxonomy
定义此领域 10-20 个顶级标签。使用新标签前先在此添加。

- **Agent 类型**: agent, reasoning-agent, coding-agent, research-agent, autonomous-agent
- **框架/工具**: framework, tool-use, mcp, prompt-engineering, memory
- **技术**: architecture, planning, rl, fine-tuning, distillation
- **评估**: evaluation, benchmark, safety, alignment
- **人物/组织**: person, company, lab, open-source
- **Meta**: comparison, timeline, controversy, prediction, summary

## Page Thresholds
- **创建页面**: 一个实体/概念在 2+ 来源中提及 OR 在一个来源中处于核心地位
- **添加到现有页面**: 来源提到的内容已覆盖
- **不创建页面**: 一次性提及、微小细节、或超出领域的内容
- **拆分页面**: 超过 ~200 行时拆分
- **归档页面**: 内容完全被取代时移动到 `_archive/`

## Entity Pages
每个 notable entity 一页，包括:
- 概述/是什么
- 关键事实和日期
- 与其他 entity 的关系 ([[wikilinks]])
- 来源引用

## Concept Pages
每个概念/主题一页，包括:
- 定义/解释
- 当前知识状态
- 开放问题或争议
- 相关概念 ([[wikilinks]])

## Comparison Pages
并排分析，包括:
- 比较的内容和原因
- 比较维度 (表格格式优先)
- 结论或综合
- 来源

## Update Policy
新信息与现有内容冲突时:
1. 检查日期 — 新来源通常取代旧来源
2. 如果确实矛盾，记录双方立场和日期来源
3. 在 frontmatter 标记矛盾: `contradictions: [page-name]`
4. 在 lint 报告中标记供用户审核
