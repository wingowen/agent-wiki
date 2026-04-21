---
title: 长时间运行 Agent 的 Harness 设计原则
created: 2026-04-14
updated: 2026-04-14
type: concept
tags: [architecture, framework, coding-agent, autonomous-agent, planning]
sources:
  - "https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents"
  - "Notion Page ID: 34267b21-8207-8131-929e-fb39d34023b5"
---

# 长时间运行 Agent 的 Harness 设计原则

## 定义

长时间运行 Agent 的 Harness 设计原则是 Anthropic 在 "Effective Harnesses for Long-Running Agents" 一文中提出的，用于指导 Agent 在多个 [[context-engineering|Context Window]] 间保持持续、可靠工作的设计框架。

## 核心问题

Agent 在长时间运行时面临两个根本挑战：

1. **Context Window 有限** — 复杂任务无法在单个窗口内完成，需要跨会话连续工作
2. **会话离散性** — 每个新会话开始时对之前发生的事情没有记忆

## 关键原则

### 1. 双重 Agent 架构

将工作分为两个专门角色：
- [[initializer-agent (待创建)]] — 设置初始环境和功能清单
- [[coding-agent (待创建)]] — 增量推进，维持 [[clean-state]]

### 2. 增量推进 (Incremental Progress)

要求 Agent 每次只处理一个功能，而非试图"一枪搞定"整个项目。这解决了 Agent 倾向于一次做太多事情的问题。

### 3. Feature List（功能清单）

通过结构化的功能清单（JSON 格式）追踪进度，每个功能包含：
- `description`: 功能描述
- `steps`: 验证步骤
- `passes`: 完成状态（boolean）

### 4. Clean State（干净状态）

每个会话结束时保持环境处于可合并状态，使下一个 Agent 可以直接继续工作。

### 5. 测试驱动验证

使用浏览器自动化等工具进行端到端测试，而非仅靠代码审查。明确提示 Agent 使用测试工具并像人类用户一样操作。

### 6. 快速上手流程 (Getting Up to Speed)

每个新会话开始时，Agent 应执行：
1. 阅读 `claude-progress.txt` 了解进度
2. 阅读 git commit 历史了解最近变更
3. 检查开发服务器状态

## 失败模式与解决方案

| 失败模式 | 解决方案 |
|---------|---------|
| 试图一次性完成所有事 | 增量推进 + Feature List |
| 过早宣布完成 | 功能清单追踪所有功能状态 |
| 环境混乱无法继续 | Clean State + git commit |
| 功能未测试就标记完成 | 浏览器自动化测试验证 |
| Context Window 耗尽 | Compaction（上下文压缩）+ Clean State |

## 与 Agent Loop 的关系

[[agent-loop|Agent Loop]]（Gather Context → Take Action → Verify Work → Repeat）是单个会话内的执行框架，而长时间运行 Harness 设计原则是跨会话、解决会话间状态传递问题的更高层架构原则。

## 相关实体

- [[dual-agent-architecture|Dual Agent Architecture]] — 双重架构模式
- [[claude-agent-sdk|Claude Agent SDK]] — 底层框架
- [[clean-state|Clean State]] — 状态管理目标

## 外部链接

- 原文：https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents