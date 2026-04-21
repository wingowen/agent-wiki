---
title: 高级工具使用
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [tool-use, agent, architecture, prompt-engineering]
sources:
  - https://www.anthropic.com/engineering/advanced-tool-use
---

## Definition

高级工具使用（Advanced Tool Use）是 Claude 开发者平台在 2025 年 11 月推出的一套 Beta 功能集，旨在解决 AI Agent 在使用大量工具时面临的三大核心挑战：工具发现效率、工具执行效率和工具调用准确性。该功能将工具使用从简单的函数调用推向智能编排阶段。

## Key techniques

### 1. Tool Search Tool — 动态工具发现

**问题**: 传统方式需要将所有工具定义预先加载到上下文，100+ 工具时消耗 50,000+ Token。

**解决方案**: 使用 BM25 搜索算法按需发现工具，通过 `defer_loading=true` 延迟加载非必要工具。

```python
# 工具定义示例
{
  "name": "get_expenses",
  "description": "Get all expenses for a user...",
  "input_schema": {...},
  "defer_loading": true  # 延迟加载标记
}
```

**效果**: 工具库从 50+ 工具减少到 5 工具定义，Token 消耗降低 85%。

### 2. Programmatic Tool Calling — 编程式工具执行

**问题**: 自然语言工具调用每次都需要完整推理过程，中间结果堆积在上下文中。

**解决方案**: Claude 编写 Python 代码编排工具，工具结果由代码处理不进入上下文。

```python
# Claude 编写的编排代码示例
team = await get_team_members("engineering")
levels = list(set(m["level"] for m in team))
budget_results = await asyncio.gather(*[
    get_budget_by_level(level) for level in levels
])
# ... 并行获取所有费用
# 只返回最终汇总结果
```

**效果**: Token 减少 37%，延迟显著降低，准确性提升（GIA: 46.5% → 51.2%）。

### 3. Tool Use Examples — 参数准确性

**问题**: JSON Schema 只定义结构有效性，无法表达使用模式和最佳实践。

**解决方案**: 通过 `input_examples` 提供真实使用示例，展示何时包含可选参数、哪些组合合理等。

```json
{
  "name": "book_flight",
  "input_schema": {...},
  "input_examples": [
    {"origin": "SFO", "destination": "LAX", "trip_class": "economy"},
    {"origin": "NYC", "destination": "LHR", "trip_class": "business", "seat_preference": "aisle"}
  ]
}
```

**最佳实践**:
- 使用真实数据（真实城市名、合理价格）
- 展示最小化、部分和完整规范的多样性
- 每个工具 1-5 个示例
- 专注于 Schema 无法表达的歧义

## Related entities and concepts

- **核心实体**: [[tool-search-tool]]（工具发现）、[[programmatic-tool-calling]]（工具执行）、[[anthropic]]（提供商）
- **技术基础**: [[model-context-protocol]]（工具协议）、[[code-execution-with-mcp]]（代码执行模式）
- **相关概念**: [[context-engineering|上下文工程]]（Token 优化）、[[multi-agent-system|多 Agent 系统]]（复杂工作流）、[[agent-architecture-design|Agent 架构设计]]（编排逻辑）
- **三个功能的关系**: Tool Search Tool 解决"用什么工具"，Programmatic Tool Calling 解决"如何高效执行"，Tool Use Examples 解决"如何正确调用"
