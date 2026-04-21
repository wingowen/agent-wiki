---
title: 提示词注入
created: 2026-04-15
updated: 2026-04-15
type: concept
tags: [prompt-engineering, architecture]
sources:
  - NotionPageId: 34367b21-8207-8140-9199-e7fefb5ffb3c
---

# 提示词注入

## 定义

提示词注入（Prompt Injection）是一种通过向 AI Agent 注入预设提示词来定义代理行为、人格、记忆和工具使用约束的技术。它是 AI Agent 架构中的核心组件，决定了代理如何感知、思考和行动。

在 nanobot 系统中，提示词注入采用混合策略：结合文件系统模板（高可定制性）和硬编码提示词（高稳定性）。

## 关键技术与机制

### 1. 文件注入方式

通过 `nanobot/templates/` 目录下的模板文件注入提示词：

**核心引导文件（BOOTSTRAP_FILES）**：
| 文件 | 作用 |
|------|------|
| AGENTS.md | 代理指令，包含提醒和心跳任务的使用指南 |
| SOUL.md | 定义代理的人格、价值观和沟通风格 |
| USER.md | 用户配置文件，包含用户基本信息和偏好设置 |
| TOOLS.md | 工具使用说明和约束 |

**代理模板文件**：
- identity.md — 核心身份信息，包含运行时环境和工作空间信息
- platform_policy.md — 平台特定的政策和指导
- skills_section.md — 技能部分的模板
- evaluator.md — 评估器模板
- subagent_system.md — 子代理系统模板
- untrusted_content.md — 不可信内容处理指南

**记忆和心跳模板**：
- MEMORY.md — 长期记忆模板
- HEARTBEAT.md — 心跳任务模板

### 2. 硬编码方式

在代码中直接定义提示词，优点是稳定性和一致性：

| 位置 | 内容 | 作用 |
|------|------|------|
| ContextBuilder._RUNTIME_CONTEXT_TAG | [Runtime Context — metadata only, not instructions] | 运行时上下文标签 |
| ContextBuilder._build_runtime_context | 构建包含当前时间、通道和聊天ID的运行时上下文 | 提供运行时元数据 |
| CronTool 描述 | Tool to schedule reminders and recurring tasks. | cron 工具描述 |

### 3. 注入流程

1. 核心身份：加载 identity.md 模板
2. 引导文件：加载 AGENTS.md、SOUL.md、USER.md、TOOLS.md
3. 记忆上下文：从内存存储中获取
4. 技能信息：加载始终激活的技能
5. 运行时上下文：注入运行时元数据

## 相关技术与概念对比

| 类型 | 数量 | 可定制性 | 稳定性 |
|------|------|----------|--------|
| 文件注入 | 16+ | 高 | 中 |
| 硬编码 | 5+ | 低 | 高 |

## 相关实体与概念

### 相关实体
- [[nanobot]] — 采用提示词注入的 AI Agent 框架
- [[crontool]] — 通过硬编码提示词注入的工具

### 相关概念
- [[agent-architecture-design]] — Agent 架构设计
- [[prompt-engineering]] — 提示词工程
- [[context-management-short-long-term-memory]] — 上下文管理
- [[tool-use]] — 工具使用
