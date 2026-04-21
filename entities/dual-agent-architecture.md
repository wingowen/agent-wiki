---
title: Dual Agent Architecture (双重 Agent 架构)
created: 2026-04-14
updated: 2026-04-14
type: entity
tags: [architecture, coding-agent, framework, agent]
sources:
  - "https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents"
  - "Notion Page ID: 34267b21-8207-8131-929e-fb39d34023b5"
---

# Dual Agent Architecture (双重 Agent 架构)

## 什么是双重 Agent 架构

双重 Agent 架构是 Anthropic 为解决长时间运行 Agent 在多个 [[context-engineering|Context Window]] 间维持连续性而提出的设计模式。该架构将 Agent 的工作分为两个专门角色：[[initializer-agent (待创建)]] 负责初始化环境，[[coding-agent (待创建)]] 负责增量推进任务。

## 核心组件

### Initializer Agent（初始化 Agent）

首次会话运行，专门负责：
- 创建 `init.sh` 环境初始化脚本
- 生成 `claude-progress.txt` 进度日志文件
- 建立 Feature List（功能清单）—— 扩展用户需求为200+可测试的功能项
- 创建初始 git commit 记录添加了哪些文件

### Coding Agent（编码 Agent）

后续每个会话运行，职责为：
- 每次只处理一个功能（增量推进）
- 通过更新 Feature List 中的 `passes` 字段标记功能完成状态
- 保持环境处于 [[clean-state]]
- 将进展提交到 git（附描述性 commit 消息）

## 工作流程

```
[用户需求] 
     ↓
[Initializer Agent]
  - 创建 init.sh
  - 创建 claude-progress.txt (含 Feature List)
  - 建立初始 git commit
     ↓
[Coding Agent Session 1]
  - 阅读进度日志
  - 实现功能 1 → passes: true
  - git commit
     ↓
[Coding Agent Session 2]
  - 阅读进度日志 + git 历史
  - 实现功能 2 → passes: true
  - git commit
     ↓
[重复直到所有功能完成]
```

## 为什么需要双重架构

单一 Agent 的两个失败模式：
1. **试图一次性完成所有事** — 耗尽 Context Window，留下未完成且无文档的状态
2. **过早宣布完成** — 看到部分进展就认为项目完成

双重架构通过角色分离和明确的进度追踪解决这两个问题。

## 相关概念

- [[claude-agent-sdk|Claude Agent SDK]] — 该架构的底层框架
- [[context-engineering|Context Engineering]] — Context Window 管理
- [[clean-state]] — 每个会话结束时应保持的状态
- [[agent-loop|Agent Loop]] — SDK 的核心执行循环

## 外部链接

- 原文：https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents