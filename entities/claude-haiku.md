---
title: Claude Haiku
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [agent, reasoning-agent]
sources:
  - https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues
---

# Claude Haiku

Claude Haiku 是 Anthropic 开发的轻量级 AI 模型，以高速和低成本著称，同时保持了相当的推理能力。

## 版本

- **Claude Haiku 3.5** - 最新版本，在本次故障复盘中，该模型受到了 XLA:TPU 编译器 Bug 的影响

## 在 Anthropic 产品线中的定位

Claude Haiku 与 [[deepseek|Claude Sonnet]] 和 Claude Opus 构成 Anthropic 的三级模型体系：

| 模型 | 定位 | 特点 |
|------|------|------|
| Claude Haiku | 轻量级 | 快速、低成本 |
| Claude Sonnet | 平衡型 | 性能与效率平衡 |
| Claude Opus | 旗舰型 | 最高性能 |

## 相关 Bug

在 2025 年 9 月的故障复盘事件中，Claude Haiku 3.5 是首个被发现受到 approximate top-k XLA:TPU 编译错误影响的模型。该 Bug 导致在 temperature=0 时概率最高的 token 被意外丢弃。

## 关联页面

- [[anthropic|Anthropic]] - 开发公司
- [[deepseek|Claude Sonnet]] - 同系列中端模型
