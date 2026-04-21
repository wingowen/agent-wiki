---
title: SWE-bench Verified
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [benchmark, evaluation, coding-agent]
sources:
  - https://www.swebench.com/
  - https://openai.com/index/introducing-swe-bench-verified/
  - https://www.anthropic.com/engineering/swe-bench-sonnet
---

# SWE-bench Verified

## 什么是 SWE-bench Verified

SWE-bench 是一个用于评估 AI 模型完成真实世界软件工程任务能力的基准测试。具体而言，它测试模型能否解决来自热门开源 Python 仓库的 GitHub Issue。

SWE-bench Verified 是 SWE-bench 的一个经过人工审核的子集，包含 500 个问题，确保每个问题都是可解决的，因此提供了对编码 [[agent (待创建)]] 性能最清晰的衡量标准。

## 关键事实

- **发布方**: Princeton 与 OpenAI 等机构联合推出
- **问题数量**: 500 个（Verified 子集）
- **评测方式**: AI 模型在配置好的 Python 环境中解决 GitHub Issue
- **评分标准**: 解决方案与关闭原始 GitHub Issue 的 Pull Request 中的真实单元测试进行对比
- **当前最高成绩**: 49%（Claude 3.5 Sonnet）
- **饱和度**: 远未饱和，仍有较大提升空间

## 为什么 SWE-bench 重要

SWE-bench 因以下原因日益流行：

1. **真实性**: 使用来自真实项目的实际工程任务，而非竞赛或面试风格的问题
2. **未饱和**: 目前还没有任何模型突破 50% 完成率，仍有巨大提升空间
3. **整体评估**: 衡量的是整个「Agent」而非孤立的模型，开源开发者和初创公司在优化脚手架方面取得了巨大成功

## Agent 评估特点

在 SWE-bench 语境下，「Agent」指的是 AI 模型与其周围软件脚手架（Scaffolding）的组合：

- 脚手架负责生成输入模型的 [[prompt-engineering (待创建)|提示词]]
- 解析模型输出以执行操作
- 管理交互循环——将模型上一次操作的结果纳入下一次提示中

即使使用相同的底层 AI 模型，Agent 在 SWE-bench 上的表现也可能因脚手架的不同而有显著差异。

## 关联实体与概念

- [[claude-35-sonnet]] — 达到 49% 成绩的模型
- [[agent (待创建)]] — Agent = AI 模型 + 工具 + 脚手架代码
- [[claude-code]] — Anthropic 官方 CLI 编码 Agent
- [[model-context-protocol|MCP]] — 标准化工具协议

## 相关概念

- [[agent-architecture-design]] — Agent 架构设计
- [[evaluation (待创建)]] — 评估
- [[benchmark (待创建)]] — 基准测试
