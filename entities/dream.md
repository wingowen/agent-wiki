---
title: Dream
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [agent, memory, architecture]
sources:
  - NotionPageId: 34267b21-8207-81a2-b887-ec73bb83404c
---

# Dream

## 什么是 Dream

Dream 是 nanobot 框架中的核心组件，负责管理长期记忆文件的更新和维护。它通过两阶段处理流程（分析 + 编辑）实现记忆的智能整合，确保 Agent 的长期记忆保持准确、连贯和最新。

Dream 的设计理念是**手术式编辑**——精确、高效、最小化侵入性地更新记忆文件，避免对已有记忆结构的破坏性修改。

## 关键事实

### 两阶段处理流程

**第一阶段：分析差异**
- 读取 history.jsonl 和长期记忆文件（MEMORY.md、SOUL.md、USER.md）
- 分析历史信息与现有记忆的差异
- 识别需要更新的记忆内容
- 生成结构化的分析报告

**第二阶段：手术式编辑**
- 基于分析报告生成编辑指令
- 执行精确的文件编辑操作
- 保持记忆的连贯性和结构完整性

### 配置项

```yaml
memory:
  dream:
    enabled: true      # 是否启用 Dream
    interval: 3600     # 自动触发间隔（秒）
    max_history_lines: 1000  # 分析的历史行数
```

### 触发方式

- **自动触发**：根据配置的 interval 定期执行
- **手动触发**：通过 `/dream` 命令手动触发 Dream 处理

### 模板文件

Dream 使用两个核心模板文件：
- **dream_phase1.md** - 分析阶段模板，引导 LLM 分析历史信息与现有记忆的差异，识别需要更新的关键信息，生成结构化分析报告
- **dream_phase2.md** - 编辑阶段模板，基于分析报告生成编辑指令，指导 LLM 执行手术式编辑，确保编辑后的记忆保持结构清晰

## 关系

### 相关实体
- [[nanobot]] — Dream 所在的 Agent 框架
- [[consolidator]] — 前置组件，负责将会话消息压缩为 history.jsonl
- [[memory-store (待创建)]] — 配合组件，负责记忆文件的读写操作
- [[git-store (待创建)]] — 版本控制组件，自动提交记忆变更，记录记忆演化历史

### 相关概念
- [[context-management-short-long-term-memory]] — 上下文管理（短期与长期记忆）的总体框架
- [[prompt-engineering]] — Dream 使用 LLM 进行分析和编辑，涉及提示词工程