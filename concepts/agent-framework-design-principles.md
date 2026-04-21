---
title: AI Agent 框架设计原则
created: 2026-04-15
updated: 2026-04-15
type: concept
tags: [architecture, framework, agent, tool-use, prompt-engineering]
sources:
  - "腾讯技术工程 - 详尽地带你从零开始设计实现一个 AI Agent 框架"
---

# AI Agent 框架设计原则

## 定义

AI Agent 框架是实现 AI 智能体的软件系统基础架构。AI 智能体是使用 AI 来实现目标并代表用户完成任务的软件系统，其表现出推理、规划和记忆能力，具有自主性，能够自主学习、适应和做出决定。

核心公式：**Agent = Reasoning + Acting**

## 关键设计原则

### 1. Agent Loop（智能体循环）

Agent 框架的核心是 Agent Loop，执行流程：

1. **初始上下文**（系统提示词 + 用户请求）
2. **Agent Loop 开始**
3. Agent 读取上下文 → 思考 → 决定行动
4. 执行工具/行动 → 获得结果
5. 结果追加到上下文
6. 循环继续或结束

安全设置：设置迭代安全上限（如 MAX_TURNS=200）

### 2. 三大要素设计

#### LLM Call（语言模型调用）

- 使用标准化 OpenAI SDK
- 同步非流式调用（为保证代码可读性）
- 支持 Tool Calls 的模型（如 [[deepseek]] deepseek-chat）

#### Tools Call（工具调用）

遵循 OpenAI Function Calling 标准格式（OpenAI Tools API schema）：

- **Tool 实现**: 极简工具集，操作对象包含文件、Shell 和 Python 代码执行
- **4 个核心工具**:
  - `shell_exec`: 执行 Shell 命令并返回输出
  - `file_read`: 读取文件内容
  - `file_write`: 写入文件内容（自动创建目录）
  - `python_exec`: 在子进程中执行 Python 代码并返回输出
- **Tools 注册**: 手动维护字典映射 `name → (function, OpenAI function schema)`

#### Context Engineering（上下文工程）

- **System Prompt**: 极简系统提示词，告知 LLM 可用工具和 ReAct 思考方式
- **用户 Session 管理**: 使用 messages 列表方式（OpenAI chat 格式），累积系统提示词、用户消息、助手响应和工具结果

### 3. ReAct 模式

ReAct = Reasoning + Acting，在 System Prompt 中明确告知 LLM 使用 ReAct 思考方式，是实现 Agent 推理与行动融合的关键模式。

## 极简设计理念

为什么要极简？
- 方便论述清楚 Agent 的关键点
- 代码库越简单，上下文越清晰（信息噪声越少），Agent 则越智能
- 框架提供基础工具，上下文工程提升智能

## 相关实体

- [[openclaw|OpenClaw]] - 底层 Agent Core (Pi Agent) 使用类似设计
- [[deepseek|DeepSeek]] - LLM Provider
- OpenAI SDK - 标准化工具体系
- Pi Agent - OpenClaw 的底层实现

## 相关概念

- [[agent-architecture-design]]
- [[langgraph-core-principles-react]]
- [[context-management-short-long-term-memory]]
- [[rag-retrieval-augmented-generation]]
- Tool Calls / Function Calling
- System Prompt