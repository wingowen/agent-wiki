---
title: AgentHook
created: 2026-04-15
updated: 2026-04-15
type: entity
tags: [architecture, tool-use]
sources:
  - "Notion: 34367b21-8207-81d2-afd3-cd5fe531fbc6"
---

# AgentHook

## 什么是 AgentHook

AgentHook 是 Agent 系统的生命周期 Hook 基类，允许开发者在 Agent 执行的关键节点插入自定义逻辑。当前系统主要关注 agent 执行的生命周期，缺少针对 slash 命令的特定 hook 点。

## 现有 Hook 点

| Hook 方法 | 说明 |
|-----------|------|
| `wants_streaming` | 是否需要流式输出 |
| `before_iteration` | 每次迭代前 |
| `on_stream` | 流式输出时 |
| `on_stream_end` | 流式输出结束时 |
| `before_execute_tools` | 执行工具前 |
| `after_iteration` | 每次迭代后 |
| `finalize_content` | 最终内容处理 |

## 现有问题

1. **缺少命令 Hook**：没有提供针对 slash 命令的特定 hook 点
2. **无法拦截命令**：无法在命令执行前/后进行拦截和处理
3. **不支持动态行为修改**：无法通过 hook 修改命令行为

## 待扩展的 Hook

根据设计方案，计划添加以下命令相关的 Hook：

### before_command

```python
async def before_command(self, ctx: CommandContext) -> OutboundMessage | None:
    """Called before executing a slash command.
    
    Return an OutboundMessage to bypass the command execution.
    """
    return None
```

### after_command

```python
async def after_command(self, ctx: CommandContext, result: OutboundMessage | None) -> OutboundMessage | None:
    """Called after executing a slash command.
    
    Can modify or replace the result.
    """
    return result
```

## 关系

### 关联实体

- [[agent-architecture-design]] - AgentHook 是架构的核心组件
- [[langgraph-core-principles-react]] - 与 ReAct 模式的关系

### 关联概念

- [[context-management-short-long-term-memory]] - 上下文管理相关
- (待创建) slash-command-hook-design - 自定义 Slash 命令 Hook 设计方案
