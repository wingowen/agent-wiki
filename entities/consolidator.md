---
title: Consolidator
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [agent, memory, architecture]
sources:
  - NotionPageId: 34267b21-8207-81a2-b887-ec73bb83404c
---

# Consolidator

## 什么是 Consolidator

Consolidator 是 nanobot 框架中的轻量级记忆压缩组件，负责将会话消息转化为可管理的历史总结。它是双层记忆系统中**短期记忆向长期记忆流动**的第一道处理环节。

Consolidator 的核心职责是**轻量化压缩**——不进行深度分析，只提取关键信息并生成简洁的历史记录，为后续 Dream 组件的分析工作提供原材料。

## 关键事实

### 处理流程

1. **接收会话消息**：用户与 nanobot 交互产生的对话内容
2. **轻量级压缩**：提取关键信息，生成简洁的总结
3. **追加到 history.jsonl**：以追加方式写入历史记录文件

### 输出格式

Consolidator 将压缩后的历史总结追加到 `history.jsonl` 文件，采用 JSONL 格式（每行一个 JSON 对象），便于后续 Dream 读取和分析。

### 设计原则

- **轻量优先**：不进行深度推理，只做信息提取和压缩
- **追加模式**：不修改已有内容，只追加新记录
- **为 Dream 准备**：输出格式经过优化，便于 Dream 进行差异分析

### 在记忆流中的位置

```
会话消息 → Consolidator → history.jsonl → Dream → 长期记忆文件
```

Consolidator 位于短期记忆（会话消息）和 Dream 之间，起到承上启下的作用。

## 关系

### 相关实体
- [[nanobot]] — Consolidator 所在的 Agent 框架
- [[dream]] — 下游组件，负责分析 history.jsonl 并更新长期记忆
- [[memory-store (待创建)]] — 配合组件，负责记忆文件的读写操作

### 相关概念
- [[context-management-short-long-term-memory]] — 上下文管理（短期与长期记忆）的总体框架
- [[agent-loop]] — Agent 执行循环中的记忆管理环节