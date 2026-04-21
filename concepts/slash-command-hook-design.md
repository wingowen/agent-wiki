---
title: 自定义 Slash 命令 Hook 设计方案
created: 2026-04-15
updated: 2026-04-15
type: concept
tags: [agent, architecture, tool-use]
sources:
  - "Notion: 34367b21-8207-81d2-afd3-cd5fe531fbc6"
---

# 自定义 Slash 命令 Hook 设计方案

## 定义

自定义 Slash 命令 Hook 设计方案是一个为 Agent 系统添加灵活命令扩展机制的设计方案。该方案旨在解决当前项目中 slash 命令硬编码、无法扩展的问题，允许用户通过配置或插件的方式添加自定义命令，并通过 hook 机制自定义命令行为。

## 背景问题

### 当前系统的三个主要问题

1. **缺乏自定义 Slash 命令的扩展机制**
   - 当前的 slash 命令硬编码在 `command/builtin.py` 中
   - 用户无法通过配置或插件的方式添加自定义命令

2. **Hook 系统不支持 Slash 命令**
   - 当前的 [[agent-hook]] 系统主要关注 agent 执行的生命周期
   - 没有提供针对 slash 命令的特定 hook 点

3. **命令路由硬编码**
   - 命令路由逻辑在 [[command-router]] 初始化时硬编码注册
   - 没有提供动态注册的机制

## 当前系统分析

### 命令路由系统

[[command-router]] 支持四种命令类型：

| 类型 | 说明 | 示例 |
|------|------|------|
| priority | 优先级命令，锁外处理 | `/stop`, `/restart` |
| exact | 精确匹配，锁内处理 | `/help` |
| prefix | 前缀匹配，最长优先 | `/search` |
| interceptors | 拦截器，回退机制 | - |

### Hook 系统生命周期

[[agent-hook]] 提供以下生命周期事件：

- `wants_streaming` → `before_iteration` → `on_stream` → `on_stream_end` → `before_execute_tools` → `after_iteration` → `finalize_content`

### 执行流程

1. 消息进入 `AgentLoop._dispatch` 方法
2. 检查是否为优先级命令，若是则直接处理
3. 否则创建任务执行 `_process_message` 方法
4. 在 `_process_message` 中检查是否为 slash 命令
   - 若是：通过 `commands.dispatch` 处理
   - 否则：进行正常的 agent 处理流程

## 解决方案

### 3.1 自定义 Slash 命令扩展机制

#### 命令注册 API

在 [[agent-architecture-design]] 中扩展 `AgentLoop` 类，添加 `register_command` 方法：

```python
class AgentLoop:
    def register_command(self, cmd: str, handler: Handler, type: str = "exact"):
        """Register a custom slash command.
        
        Args:
            cmd: Command string (e.g. "/mycommand")
            handler: Command handler function
            type: Command type ("priority", "exact", "prefix")
        """
        if type == "priority":
            self.commands.priority(cmd, handler)
        elif type == "exact":
            self.commands.exact(cmd, handler)
        elif type == "prefix":
            self.commands.prefix(cmd, handler)
```

#### 命令执行 Hook

扩展 [[agent-hook]] 类，添加针对 slash 命令的 hook 点：

```python
class AgentHook:
    async def before_command(self, ctx: CommandContext) -> OutboundMessage | None:
        """Called before executing a slash command.
        Return an OutboundMessage to bypass the command execution."""
        return None
    
    async def after_command(self, ctx: CommandContext, result: OutboundMessage | None) -> OutboundMessage | None:
        """Called after executing a slash command.
        Can modify or replace the result."""
        return result
```

### 3.2 实现建议

#### 命令注册机制

1. **扩展 AgentLoop 类**：添加 `register_command` 和 `unregister_command` 方法
2. **修改 CommandRouter 类**：添加 `remove` 方法支持命令注销
3. **添加命令元数据**：支持命令描述、权限等元数据

## 测试策略

### 功能测试

- 测试自定义命令注册和执行
- 测试命令 hook 的调用
- 测试命令拦截器的功能

## 预期效果

1. **提高系统的可扩展性**：允许用户通过多种方式添加自定义 slash 命令
2. **增强系统的灵活性**：通过 hook 机制允许用户自定义命令的行为
3. **简化命令管理**：通过配置和插件系统简化命令的注册和管理
4. **保持向后兼容性**：确保现有命令和系统不受影响

## 关系

### 关联实体

- [[command-router]] - 命令路由系统
- [[agent-hook]] - Hook 系统
- [[agent-architecture-design]] - Agent 架构设计

### 关联概念

- [[langgraph-core-principles-react]] - 与 LangGraph/ReAct 的关系
- [[context-management-short-long-term-memory]] - 上下文管理
