---
title: Model Context Protocol (MCP)
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [mcp, tool-use, standard, protocol]
sources:
  - https://modelcontextprotocol.io/docs/getting-started/intro
  - https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
---

## What it is

Model Context Protocol（MCP）是一个标准化的协议，旨在为 AI Agent 提供一种统一的方式来调用外部工具和数据源。它是上下文工程中的关键组件，使得 Agent 能够有效地与各种外部系统交互。

## Key facts

1. **标准化工具调用**：MCP 提供了一种标准化的方式来定义和调用工具，使得 Agent 可以灵活地与不同的外部系统集成。

2. **上下文管理的关键组件**：在构建多轮推理和更长的时间跨度上运行的强大 Agent 时，MCP 是整个上下文状态（系统指令、工具、外部数据、消息历史等）的重要组成部分。

3. **与 Prompt Engineering 的关系**：从 [[prompt-injection|提示词工程]] 到上下文工程的演进中，MCP 代表了工具集成层面的标准化努力。

## Relationships

- **相关概念**：[[context-management-short-long-term-memory|上下文管理]]、prompt-engineering
- **应用场景**：用于增强 Agent 的工具调用能力，实现外部数据访问
- **相关实体**：[[deepseek|Anthropic]]（Claude MCP 实现）、[[openclaw|OpenClaw]]（支持 MCP 的框架）
