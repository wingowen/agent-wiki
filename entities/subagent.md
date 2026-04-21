---
title: Subagent
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [agent, coding-agent, framework, tool-use]
sources:
  - https://www.anthropic.com/engineering/claude-code-best-practices
---

## What it is

Subagent（子代理）是 Claude Code 中的一种机制，允许主 Agent 将特定任务委派给在独立上下文中运行的子 Agent。每个 Subagent 有自己独立的上下文窗口和允许工具列表，完成后只将相关信息返回给主 Agent。

## Key facts

1. **上下文隔离**：Subagent 在独立上下文中运行，不会污染主对话的上下文窗口

2. **信息汇总**：Subagent 完成后只将相关信息返回给主 Agent，保持主 Agent 的上下文整洁

3. **主要用途**：
   - **调查和研究**：当需要阅读大量文件或搜索代码库时，使用 subagent 避免上下文污染
   - **验证**：让独立的 subagent 验证实现是否正确，避免实现过拟合到测试用例
   - **并行工作**：对于可以同时进行的独立任务，可以使用多个 subagent 并行处理

4. **上下文窗口管理**：上下文窗口是最根本的约束，subagent 是管理这一资源的强大工具

## Relationships

- **相关工具**：[[claude-code|Claude Code]]
- **相关概念**：[[agentic-coding|Agentic Coding]]
- **类似机制**：在其他多 Agent 系统中也有类似的委派机制