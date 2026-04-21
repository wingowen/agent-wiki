---
title: Tool Use
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [tool-use, agent, framework]
sources:
---

## Definition

Tool Use（工具使用）是 Agent 系统的核心能力之一，指 Agent 调用外部工具或函数来扩展其能力范围，突破纯语言模型的限制。

## Key Techniques

### Tool Definition

为 LLM 提供工具描述，包括工具名称、参数模式、功能说明。

### Tool Selection

LLM 根据用户请求和可用工具，决定调用哪个工具以及传入什么参数。

### Result Handling

将工具执行结果返回给 LLM，继续推理循环。

## Related

→ [[model-context-protocol|MCP]]（标准化工具协议）
→ [[agent-tool-design|Agent 工具设计原则]]
→ [[tool-search-tool|Tool Search Tool]]（动态工具发现）
→ [[programmatic-tool-calling|Programmatic Tool Calling]]（代码化工具调用）
