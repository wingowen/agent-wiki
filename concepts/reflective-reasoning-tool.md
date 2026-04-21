---
title: 反思性推理工具
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [tool-use, reasoning-agent, agent, planning, prompt-engineering]
sources:
- https://www.anthropic.com/engineering/claude-think-tool
---

# 反思性推理工具

## 定义

反思性推理工具是一种让 AI Agent 在执行过程中暂停并进行结构化思考的机制。与传统的预生成式思维链不同，反思性工具在响应生成过程中触发，允许模型基于新获得的信息（如工具调用结果）进行即时推理。

## 核心原理

1. **时机差异**：模型在获得新信息后主动触发思考，而非仅在响应开始前
2. **聚焦性推理**：思考内容聚焦于新信息的相关性，而非全面规划
3. **工具整合**：将思考过程作为日志追加，不改变外部状态或获取新信息

## 关键技术

### 工具定义
```json
{
  "name": "think",
  "description": "用于复杂推理或需要缓存记忆的场景",
  "input_schema": {
    "type": "object",
    "properties": {
      "thought": {"type": "string", "description": "需要思考的内容"}
    },
    "required": ["thought"]
  }
}
```

### 策略性提示
有效的提示应包含：
- 推理详细程度的期望
- 复杂指令的分解方法
- 处理常见场景的决策树
- 信息完整性的检查清单

### 系统提示 vs 工具描述
当指令较长或复杂时，将使用指南放在 System Prompt 中比放在工具描述本身更有效，因为它提供更广泛的上下文。

## 适用条件

反思性工具在以下场景有效：
- 长链工具调用
- 顺序决策（每步建立在前一步之上）
- 策略合规性要求高的环境
- 需要根据工具输出调整下一步的场景

## 局限性

以下场景通常不需要反思性工具：
- 单次或并行工具调用
- 简单指令遵循
- 模型默认行为已足够好的情况

## 相关概念

- [[think-tool]] — Anthropic 的具体实现
- [[extended-thinking (待创建)]] — 预生成式思维链，与反思性工具互补
- [[agent-tool-design]] — 设计高效工具的核心方法论
- [[tau-bench]] — 验证此类工具效果的评估基准