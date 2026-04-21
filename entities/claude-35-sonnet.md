---
title: Claude 3.5 Sonnet
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [model, agent, coding-agent, evaluation]
sources:
  - https://www.anthropic.com/engineering/swe-bench-sonnet
  - https://www.anthropic.com/news/3-5-models-and-computer-use
---

# Claude 3.5 Sonnet

## 什么是 Claude 3.5 Sonnet

Claude 3.5 Sonnet 是 Anthropic 开发的升级版大语言模型，在 [[swe-bench-verified]] 软件工程基准测试中达到了 49% 的成绩，超越了之前的 SOTA（State-of-the-Art）模型的 45%。

## 关键事实

- **发布方**: Anthropic
- **发布时间**: 2025年1月（本文发布时）
- **核心技术**: 基于 Claude 3.5 系列模型的升级版本
- **上下文长度**: 200k tokens
- **SWE-bench Verified 成绩**: 49%（当前最高）
- **多模态能力**: 具备出色的视觉和多模态能力，但尚未实现查看保存到文件系统或 URL 引用的文件的功能

## 关键能力

Claude 3.5 Sonnet 在编码 Agent 任务中展现出以下特点：

- **更强的自我纠正能力**: 相较于旧版模型，进行自我纠正的频率更高
- **尝试多种解决方案**: 不易陷入反复犯同样错误的困境
- **韧性（Tenacious）**: 给定足够的时间，通常能找到解决问题的方法
- **长上下文处理**: 支持 200k 上下文长度，可处理数百轮交互

## 关联实体与概念

- [[swe-bench-verified]] — 本模型参与评测的基准
- [[agent (待创建)]] — 由模型 + 工具 + 脚手架代码组成的智能体系统
- [[claude-code]] — Anthropic 官方 CLI 编码 Agent
- [[model-context-protocol|MCP]] — 标准化工具协议

## 相关概念

- [[agent-architecture-design]] — Agent 架构设计
- [[context-engineering]] — 上下文工程
