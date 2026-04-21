---
title: Agent 工具设计原则
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [tool-use, agent, prompt-engineering, evaluation, architecture]
sources:
  - https://www.anthropic.com/engineering/writing-tools-for-agents
---

## Definition | 定义

Agent 工具设计原则（Tool Design Principles for Agents）是指为 AI Agent 设计高效工具的核心方法论。与传统确定性软件的 API 设计不同，Agent 工具需要面向非确定性系统设计，考虑 Agent 的认知特点和行为模式。

核心观点：**工具不是传统意义上的 API 或函数，而是面向非确定性 Agent 设计的新型接口**。这一认知转变是理解 Agent 工具设计的基础。

## Key Techniques | 关键技巧

### 1. 评估驱动的开发方法

采用迭代式、评估驱动的方法来改进工具：

1. **构建原型** → **建立评估体系** → **使用 Agent 自动优化** → **迭代改进**
2. 类似软件开发中的 TDD（测试驱动开发）理念
3. 通过 [[claude-code|Claude Code]] 等 Agent 自动提升工具性能

### 2. 工具整合原则（Tool Consolidation）

与其提供大量细粒度工具，不如将相关功能合并为少量高层次工具：

| 不推荐 | 推荐 |
|--------|------|
| `create_event` + `find_available_time` | `schedule_event` |
| `read_logs` | `search_logs` |
| `get_customer_by_id` + `list_transactions` + `list_notes` | `get_customer_context` |

**好处**：减少上下文消耗、降低 Agent 选择错误工具的概率

### 3. 工具命名空间（Tool Namespacing）

为工具添加清晰的功能边界前缀，帮助 Agent 在大量工具中选择正确的工具：

- **按服务命名**：`asana_search`、`jira_search`
- **按资源命名**：`asana_projects_search`、`asana_users_search`

命名空间的选择（前缀 vs 后缀）对评估性能有"非平凡影响"，建议通过 A/B 测试确定最佳方案。

### 4. 返回有意义的上下文

工具应只向 Agent 返回高信号信息：

- **优先**：语义化的自然语言名称（`name`、`image_url`、`file_type`）
- **避免**：低级技术标识符（`uuid`、`256px_image_url`、`mime_type`）

将任意 UUID 解析为可读的语义名称可显著减少幻觉，提高检索精度。

### 5. 响应格式控制（Response Format）

通过 `response_format` 参数让 Agent 控制返回信息的详细程度：

```
enum ResponseFormat {
  DETAILED = "detailed",  // 包含所有 ID 和技术标识符
  CONCISE = "concise"      // 只返回主题内容
}
```

在灵活性和 Token 效率之间取得平衡。

### 6. Token 效率优化

- 实现分页、范围选择、过滤和截断
- 设置合理的默认参数值（如 Claude Code 默认限制 25,000 Token）
- 截断响应时提供有用引导

### 7. 错误响应工程

对错误响应进行提示工程，清晰传达可操作的改进建议：

| 不推荐 | 推荐 |
|--------|------|
| 不透明的错误代码 | 具体且可操作的建议 |
| 堆栈跟踪 | 正确格式的示例输入 |

## Related Concepts

- [[tool-use|工具使用]] — 核心概念
- [[prompt-engineering|提示工程]] — 工具描述的提示工程
- [[context-engineering|上下文工程]] — 上下文管理
- [[multi-agent-system|多 Agent 系统]] — 工具在多 Agent 协作中的应用
- [[evaluation (待创建)|评估体系]] — 评估驱动的开发方法

## Related Entities

- [[model-context-protocol|Model Context Protocol (MCP)]] — 标准化工具协议
- [[claude-code|Claude Code]] — 用于自动优化工具的 Agent
- [[ken-aizawa|Ken Aizawa]] — 文章作者
- [[swe-bench-verified|SWE-bench Verified]] — 评估基准
