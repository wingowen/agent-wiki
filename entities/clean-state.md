---
title: Clean State (干净状态)
created: 2026-04-14
updated: 2026-04-14
type: entity
tags: [architecture, memory, planning, coding-agent]
sources:
  - "https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents"
  - "Notion Page ID: 34267b21-8207-8131-929e-fb39d34023b5"
---

# Clean State (干净状态)

## 什么是干净状态

Clean State（干净状态）指代码处于适合合并到主分支的状态：没有重大 bug，代码有序且文档齐全。在 [[dual-agent-architecture]] 的语境下，它意味着每个 [[coding-agent (待创建)]] 会话结束时，工作目录处于可以让下一个 Agent 直接继续工作的状态，而不需要"收拾烂摊子"。

## 核心要求

实现 Clean State 的关键实践：

1. **功能完整而非部分实现** — 每个开始的功能都要完成到可测试状态
2. **描述性 git commit** — 记录每次会话的进展，供后续 Agent 通过 git 历史了解项目状态
3. **更新 claude-progress.txt** — 结构化记录功能完成情况（`passes: true/false`）
4. **测试驱动验证** — 使用浏览器自动化等工具像最终用户一样验证功能，而非仅靠代码审查

## 为什么重要

没有 Clean State 会导致：
- 后续 Agent 需要花费大量时间理解之前的状态
- 上下文窗口被浪费在重新阅读和推断上
- 错误累积，最终难以合并

## 与 Context Window 的关系

Clean State 是减少每个会话所需 Context Token 的关键：
- 后续 Agent 通过 `claude-progress.txt` + git 历史快速了解状态
- 不需要重新阅读整个代码库或对话历史
- 每个会话开始时只需知道"当前进展到哪里"和"下一步做什么"

## 相关概念

- [[dual-agent-architecture]] — 使用 Clean State 的架构模式
- [[initializer-agent (待创建)]] — 负责建立初始 Clean State
- [[coding-agent (待创建)]] — 负责维持 Clean State
- [[context-engineering|Context Engineering]] — Clean State 是上下文管理的一部分

## 类比

就像工程师下班前把工位收拾整齐，第二天来的人可以直接开始新工作，而不用先清理前任留下的混乱。