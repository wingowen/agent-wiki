---
title: OpenClaw
created: 2026-04-15
updated: 2026-04-15
type: entity
tags: [framework, agent, open-source]
sources:
  - "腾讯技术工程 - 详尽地带你从零开始设计实现一个 AI Agent 框架"
---

# OpenClaw

## 什么是 OpenClaw

OpenClaw 是一个在 2026 年初火爆并持续热度的 AI Agent 框架，为 AI Agent 带来了新的想象空间。它被本文作者作为 AI Agent 智能体元年的代表性框架进行讨论。

## 关键事实

- **热度**: 2026 年初火爆，保持高热度
- **定位**: 为 AI Agent 带来新的想象空间
- **影响**: 推动了 2026 年成为 AI Agent 真正商用化的开端
- **相关**: OpenClaw 的底层 Agent Core 被称为 Pi Agent，其 Tools 层包含四个核心工具方法：读文件（Read）、写文件（Write）、编辑文件（Edit）、命令行（Shell）

## 与其他组件的关系

- **Pi Agent**: OpenClaw 的底层 Agent Core 实现
- **Tool 工具层**: 有且仅包含四个工具方法，与本文实现的极简 Agent 框架（shell_exec, file_read, file_write, python_exec）设计理念相似

## 相关概念

- [[agent-architecture-design]]
- [[langgraph-core-principles-react]]
- 工具调用 (Tool Calls) / Function Calling