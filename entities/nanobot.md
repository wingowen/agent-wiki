---
title: nanobot
created: 2026-04-15
updated: 2026-04-15
type: entity
tags: [agent, framework, autonomous-agent]
sources:
  - NotionPageId: 34367b21-8207-8140-9199-e7fefb5ffb3c
---

# nanobot

## 什么是 nanobot

nanobot 是一个 AI Agent 框架，采用模块化的提示词注入架构，通过文件系统模板和硬编码提示词相结合的方式定义代理行为。该系统支持通过配置文件定制代理的人格、记忆、技能和工具使用约束。

## 关键事实

### 提示词注入方式

nanobot 采用混合提示词注入策略：

**通过文件注入的提示词（高可定制性）**：
- 核心引导文件（BOOTSTRAP_FILES）：AGENTS.md、SOUL.md、USER.md、TOOLS.md
- 代理模板文件：identity.md、platform_policy.md、skills_section.md、evaluator.md 等
- 内存和心跳模板：MEMORY.md、HEARTBEAT.md

**硬编码的提示词（低可定制性）**：
- ContextBuilder._RUNTIME_CONTEXT_TAG：运行时上下文标签
- ContextBuilder._build_runtime_context：构建运行时上下文
- CronTool 工具描述和参数说明

### 提示词注入流程

1. 核心身份：加载 identity.md 模板，包含运行时环境和工作空间信息
2. 引导文件：加载 AGENTS.md、SOUL.md、USER.md、TOOLS.md
3. 记忆上下文：从内存存储中获取记忆上下文
4. 技能信息：加载始终激活的技能和技能摘要
5. 运行时上下文：在用户消息前注入运行时元数据

## 关系

### 相关概念
- [[agent-architecture-design]] — Agent 架构设计
- [[prompt-engineering]] — 提示词工程
- [[context-management-short-long-term-memory]] — 上下文管理（记忆）
- [[tool-use]] — 工具使用

### 相关实体
- [[crontool]] — 定时任务工具
