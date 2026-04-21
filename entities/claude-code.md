---
title: Claude Code
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [agent, coding-agent, tool-use, framework]
sources:
  - https://docs.anthropic.com/en/docs/claude-code
  - https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
---

## What it is

Claude Code 是由 [[deepseek|Anthropic]] 官方开发的命令行工具，是一个专为 AI Agent 设计的编码辅助工具。它是 Anthropic 在构建 [[agent-architecture-design|Agent]] 实践中提出的核心参考实现，也是本文讨论的多种上下文工程技术的实践典范。

## Key facts

Claude Code 在上下文工程方面的核心实践：

1. **最小可行上下文策略**：采用混合策略，CLAUDE.md 文件被预先加载到上下文中，而 glob 和 grep 等原语允许 Agent 导航环境并即时检索文件，有效地绕过了索引过时和复杂语法树的问题。

2. **Compaction（压缩）实践**：模型会保留架构决策、未解决的 Bug 和实现细节，同时丢弃冗余的 tool 输出。Agent 以压缩的上下文加上最近访问的五个文件继续工作。

3. **结构化笔记维护**：Claude Code 会创建待办事项列表，类似地，自定义 Agent 可以维护 NOTES.md 文件来跟踪进度和发现。

4. **实时上下文检索**：通过 glob/grep 等原语实现按需检索，避免预加载过多文件导致上下文污染。

## Relationships

- **创作者**：[[deepseek|Anthropic]]
- **相关概念**：[[context-management-short-long-term-memory|上下文管理]]、[[agent-architecture-design|Agent 架构设计]]
- **类似工具**：[[openclaw|OpenClaw]]（其他 AI Agent 框架）
- **技术基础**：基于 Claude 模型，支持 Tool Use
