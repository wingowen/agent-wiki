---
title: Agent Loop（Agent 循环）
created: 2026-04-14
updated: 2026-04-21
type: concept
tags: [agent, architecture, planning, autonomous-agent]
sources:
  - "https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk"
  - "Notion Page ID: 34267b21-8207-8141-8638-c436b190d370"
---

# Agent Loop（Agent 循环）

## 定义

Agent Loop 是 [[claude-agent-sdk|Claude Agent SDK]] 提出的核心设计框架，描述了自主 Agent 执行任务的基本循环：

**Gather Context → Take Action → Verify Work → Repeat**

该框架简洁而实用，可作为设计任何 Agent 系统的参考模板。

## 循环阶段

### 1. Gather Context（收集上下文）

Agent 需要获取完成任务所需的信息和资源：

- 搜索和读取文件
- 获取用户输入和历史对话
- 调用外部 API 获取数据
- 理解任务目标和约束条件

### 2. Take Action（执行操作）

基于收集的上下文，Agent 采取具体行动：

- **编写代码**：生成可执行代码是 Claude Agent SDK 的核心能力
- **调用工具**：通过工具与外部世界交互
- **Bash 命令**：使用终端执行系统命令
- **文件操作**：创建、编辑、删除文件

### 3. Verify Work（验证工作）

Agent 需要检查和改进自身输出，确保可靠性：

- **定义规则**：提供明确定义的规则进行反馈（如 Code Linting）
- **视觉反馈**：截图或渲染用于视觉任务验证
- **LLM as a Judge**：让另一个语言模型评判输出

### 4. Repeat（重复迭代）

循环迭代直到任务完成：

- 在错误复合之前发现错误
- 在偏离时自我纠正
- 在迭代中不断改进

## 关键设计理念

Claude Agent SDK 的关键设计理念是"**给 Claude 一台电脑**"——通过终端访问让 Agent 获得与人类程序员相同的工作环境。这使得 Agent 在非编码任务上同样有效，可以：

- 读取 CSV 文件、搜索网页
- 构建可视化、解读指标
- 连接内部数据源和跨应用跟踪上下文

## 验证工作的重要性

能够检查和改进自身输出的 Agent 本质上更可靠：

> 它们在错误复合之前就能发现错误，在偏离时自我纠正，并在迭代中不断改进。

验证方法按优先级排序：
1. **基于规则的验证**（最高优先级，如 TypeScript + Lint）
2. **视觉反馈**（适用于 UI 生成或测试）
3. **LLM as a Judge**（延迟代价较高，但某些场景有价值）

## 相关实体和概念

- [[claude-agent-sdk|Claude Agent SDK]] - 提出该框架的 SDK
- [[claude-code|Claude Code]] - SDK 的起源产品
- [[model-context-protocol|MCP]] - 标准化集成协议
- [[agent-tool-design|Agent 工具设计原则]]
- [[agent-architecture-design|Agent 架构设计]]
- [[agentic-coding|Agentic Coding]]
