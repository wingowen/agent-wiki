---
title: Agent 脚手架设计
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [agent, architecture, tool-use, coding-agent, prompt-engineering]
sources:
  - https://www.anthropic.com/engineering/swe-bench-sonnet
---

# Agent 脚手架设计

## 定义

Agent 脚手架（Scaffolding）是指围绕 AI 大语言模型构建的软件框架，负责生成输入模型的[[prompt-engineering (待创建)|提示词]]、解析模型输出以执行操作，以及管理交互循环——即将模型上一次操作的结果纳入下一次提示中。

在 [[swe-bench-verified]] 等编码 Agent 评估中，即使使用相同的底层 AI 模型，Agent 的表现也可能因脚手架的不同而有显著差异。

## 核心设计原则

### 1. 最小化脚手架，保持模型自主性

Anthropic 在为 Claude 3.5 Sonnet 构建 SWE-bench Agent 时的设计理念：

- **尽可能将控制权交给语言模型本身**
- 使用一个 [[prompt-engineering (待创建)|Prompt]] + 两个通用工具（Bash Tool + Edit Tool）
- 持续采样直到模型自行决定完成，或超出其 200k 上下文长度
- 允许模型运用自身判断力决定如何推进问题，而非被硬编码为特定模式或工作流

### 2. 工具设计关键要点

#### Bash Tool（执行命令工具）
- 提供执行任意 bash 命令的能力
- 允许模型运行测试、构建项目、执行脚本

#### Edit Tool（编辑工具）
- **要求绝对路径**: 避免相对路径带来的歧义
- **使用字符串替换而非行号编辑**: 代码行号在模型看来不够直观，字符串替换更可靠
- **在工具描述中预判并防止常见错误**: 减少模型误用的可能性

### 3. Prompt 设计策略

- Prompt 应为模型概述建议的处理方法，但**不过长或过于详细**
- 模型可以自由选择如何在各步骤之间过渡，而非遵循严格的离散转换
- 如果不敏感于 Token 消耗，**显式鼓励模型生成长篇回复**会有所帮助
- 使用 [[prompt-engineering (待创建)|提示词工程]]技术优化模型输出

## 关键技术与模式

### THOUGHT/ACTION/OBSERVATION 模式

虽然未被强制约束，但这一格式清晰展示了 [[agent (待创建)]] 的推理-行动-观察循环：

- **THOUGHT**: 模型描述其思考过程和计划
- **ACTION**: 模型调用工具（如 Edit Tool、Bash Tool）
- **OBSERVATION**: 模型观察工具执行结果

### 自我纠正能力

升级版 Claude 3.5 Sonnet 展现出：
- 进行自我纠正的频率更高
- 尝试多种不同解决方案的能力
- 不易陷入反复犯同样错误的困境

## 运行挑战

使用 SWE-bench 等基准评估编码 Agent 时面临的挑战：

1. **运行时长和高 Token 成本**: 许多成功运行需要数百轮，消耗超过 100k tokens
2. **评分问题**: 环境配置问题或安装补丁被重复应用可能影响准确性
3. **隐藏测试**: 模型无法看到用于评分的测试，可能导致任务实际失败但模型认为成功
4. **多模态限制**: 模型无法查看保存到文件系统或 URL 引用的文件，容易导致[[prompt-engineering (待创建)|幻觉]]

## 关联实体与概念

- [[claude-35-sonnet]] — 展示脚手架设计的模型
- [[swe-bench-verified]] — 评估脚手架设计的基准
- [[agent (待创建)]] — 脚手架服务的对象
- [[claude-code]] — 应用脚手架设计的实际案例
- [[model-context-protocol|MCP]] — 标准化工具协议

## 相关概念

- [[agent-architecture-design]] — Agent 架构设计
- [[context-engineering]] — 上下文工程
- [[prompt-injection]] — 提示词注入
- [[tool-use]] — 工具使用
