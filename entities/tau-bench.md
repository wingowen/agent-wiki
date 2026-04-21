---
title: τ-Bench
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [benchmark, evaluation, agent, tool-use]
sources:
- https://arxiv.org/abs/2406.12045
- https://www.anthropic.com/engineering/claude-think-tool
---

# τ-Bench

## 什么是 τ-Bench

τ-Bench（Tau-Bench）是由 Sierra Research 开发的 Agent 评估基准，专注于测试 AI 模型在真实客户服务场景中的工具使用能力。该基准模拟实际的客服场景，要求 Agent 遵守复杂的策略和处理多步骤问题。

## 设计特点

τ-Bench 包含多个领域，其中航空领域因其策略复杂度高而成为测试模型推理能力的理想场景。模型需要：
- 遵守详细的取消和改签政策
- 处理多轮对话上下文
- 在多步骤决策中保持策略一致性

## 评估指标

τ-Bench 使用 pass@k 指标衡量性能：
- **pass@1**: 首次尝试成功率
- **pass@2+**: 多次尝试成功率

## 相关研究

Anthropic 在其 Think 工具研究中使用 τ-Bench 验证性能提升：
- 基线配置 pass@1: 0.332
- Think + 提示词配置 pass@1: 0.584
- 相对提升约 76%

## 关系

- [[think-tool]] — 在 τ-Bench 上验证的工具
- [[agent-tool-design]] — 工具设计方法论
- [[claude-code]] — 被测模型
- [[claude-35-sonnet]] — 性能提升显著的模型