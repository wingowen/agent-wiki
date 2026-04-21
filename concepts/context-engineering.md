---
title: Context Engineering (上下文工程)
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [agent, prompt-engineering, memory, architecture, tool-use]
sources:
  - https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
  - https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
---

## Definition

上下文工程（Context Engineering）是在 [[prompt-injection|提示词工程]] 成为应用 AI 关注焦点之后兴起的新术语。它指的是在 LLM 推理过程中策展和维护最优 Token（信息）集合的策略，包括提示词之外可能进入上下文的所有其他信息。

核心问题：在 LLM 的固有约束下优化这些 Token 的效用，以持续达成期望的结果。有效驾驭 LLM 通常需要"以上下文的方式思考"——也就是说，要考虑 LLM 在任何给定时刻可用的整体状态，以及该状态可能产生哪些潜在行为。

## Key techniques

### 短期上下文管理

1. **上下文压缩（Compaction）**：当对话接近上下文窗口限制时，对其内容进行摘要，然后用摘要重新启动一个新的上下文窗口。最佳实践：首先最大化摘要中的召回率（确保不遗漏重要信息），然后迭代提高精确度（去除不相关细节）。

2. **结构化记忆（Structured Notes）**：Agent 定期将笔记持久化到外部记忆中。与简单的对话摘要不同，结构化笔记是一种有组织的外部记忆系统，Agent 可以在需要时主动写入和读取。

3. **即时上下文检索（Just-in-Time Retrieval）**：通过 glob/grep 等原语允许 Agent 导航环境并即时检索文件，绕过了索引过时和复杂语法树的问题。混合策略（CLAUDE.md 预加载 + 按需检索）被 [[agent-architecture-design|Claude Code]] 采用。

4. **最小可行上下文**：找到尽可能最小的、高信号 Token 集合，以最大化实现期望结果的可能性。

### 上下文衰减（Context Rot）

Context Rot 指的是当上下文窗口中的 Token 数量增长时，模型准确回忆上下文中信息的能力逐渐下降的现象。这类似于人类在处理过多信息时注意力分散的情况。对"大海捞针"式基准测试的研究揭示了这一特征在所有模型中都存在。

### 长期任务管理

1. **Compaction（压缩）**：通过摘要保持长期连贯性。

2. **结构化笔记**：通过外部记忆实现持久记忆。

3. **多 Agent 架构**：将专门的子 Agent 分配给具有干净上下文窗口的聚焦任务。主 Agent 协调高层计划，子 Agent 执行深入的技术工作并返回精简摘要。

## Related entities and concepts

### Entities
- [[deepseek|Anthropic]] — 上下文工程概念的提出者和实践者
- [[claude-code|Claude Code]] — Anthropic 的官方编码 Agent，展示了上下文工程的多种实践
- [[model-context-protocol|Model Context Protocol (MCP)]] — 标准化的工具协议，是上下文工程的关键组件

### Concepts
- [[context-management-short-long-term-memory|上下文管理：短期与长期记忆]] — 上下文工程的核心组成部分
- [[prompt-injection|提示词工程]] — 上下文工程的前身，从编写提示词到管理整个上下文状态的演进
- [[agent-architecture-design|Agent 架构设计]] — 上下文工程是 Agent 架构设计的关键考虑因素
- [[rag-retrieval-augmented-generation|RAG]] — 即时上下文检索是 RAG 实践的核心技术之一
- [[hyde-hypothetical-document-embedding|HyDE]] — 与上下文检索质量相关

### Framework
- [[langgraph-core-principles-react|LangGraph]] — 提供状态管理机制，可用于实现上下文工程策略
- [[openclaw|OpenClaw]] — 支持上下文工程实践的框架
