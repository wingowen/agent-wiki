---
title: CommandRouter
created: 2026-04-15
updated: 2026-04-15
type: entity
tags: [architecture, tool-use]
sources:
  - "Notion: 34367b21-8207-81d2-afd3-cd5fe531fbc6"
---

# CommandRouter

## 什么是 CommandRouter

CommandRouter 是 Agent 系统中的命令路由类，负责管理和分发 slash 命令（以 `/` 开头的命令）。它支持多种匹配模式，是命令系统的核心组件。

## 关键事实

### 命令类型

CommandRouter 支持三种类型的命令：

| 类型 | 说明 | 处理方式 |
|------|------|----------|
| priority | 优先级命令 | 在锁之外处理（如 `/stop`, `/restart`） |
| exact | 精确匹配命令 | 在锁内处理 |
| prefix | 前缀匹配命令 | 按最长前缀优先 |
| interceptors | 拦截器 | 作为最后的回退机制 |

### 核心方法

- `priority(cmd, handler)` - 注册优先级命令
- `exact(cmd, handler)` - 注册精确匹配命令
- `prefix(cmd, handler)` - 注册前缀匹配命令
- `interceptor(handler)` - 注册拦截器
- `remove(cmd)` - 注销命令（待实现）

### 现有问题

1. 命令硬编码在 `command/builtin.py` 中
2. 不支持动态注册/注销命令
3. 缺少命令元数据支持（描述、权限等）

## 关系

### 关联实体

- [[agent-architecture-design]] - CommandRouter 是 Agent 架构的一部分
- [[langgraph-core-principles-react]] - 与 ReAct 模式的关系

### 关联概念

- [[agent-interview-qa-overview]] - 面试中可能涉及的命令路由问题
- (待创建) slash-command-hook-design - 自定义 Slash 命令 Hook 设计方案

### 相关组件

- `AgentLoop._dispatch` - 调用 CommandRouter 的入口
- `register_builtin_commands` - 内置命令注册函数
- `command/builtin.py` - 内置命令定义文件
