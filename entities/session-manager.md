---
title: SessionManager
created: 2026-04-15
updated: 2026-04-21
type: entity
tags: [agent, memory, architecture, tool-use]
sources:
  - Notion ID: 34367b21-8207-81fe-b72e-d5cd215313a6
---

# SessionManager

## 什么是 SessionManager

SessionManager 是 Agent 系统中负责管理会话历史（Session History）的核心组件。它维护用户与 Agent 之间对话的上下文信息，包括历史消息、工具调用记录、执行结果等。

## 关键事实

1. **会话历史持久化**：SessionManager 会将工具调用的结果（包括失败记录）保存到会话历史中
2. **历史影响模型决策**：模型在生成回复时会参考会话历史，导致可能重复尝试已失败的方法
3. **失败记录传播问题**：当某个工具（如 web_search）调用失败后，失败信息会被记录并影响后续决策
4. **缺乏智能过滤机制**：系统没有对失败的工具调用进行智能标记或过滤，导致模型持续尝试失败的工具

## 关联关系

- **相关概念**: [[context-management-short-long-term-memory]] — SessionManager 管理的会话历史是上下文管理的重要组成部分
- **相关问题**: [[agent-architecture-design]] — SessionManager 是 Agent 架构中的记忆管理层
- **工具调用**: SessionManager 的设计直接影响 [[command-router]] 等工具路由系统的表现
- **所属领域**: Agent 系统中的 [[memory (待创建)]] 记忆管理子模块
