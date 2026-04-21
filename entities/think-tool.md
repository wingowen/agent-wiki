---
title: Think 工具
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [tool-use, agent, reasoning-agent, prompt-engineering]
sources:
- https://www.anthropic.com/engineering/claude-think-tool
---

# Think 工具

## 什么是 Think 工具

Think 工具是 Anthropic 为 Claude 引入的一种特殊工具，它允许 Claude 在复杂任务执行过程中停下来进行结构化思考。与 [[extended-thinking (待创建)]] 不同，Think 工具在响应生成**之后**触发，而非之前。

Think 工具的核心用途是让 Claude 在处理外部信息（如工具调用结果）时反思是否已收集足够信息来继续推进。这在长链工具调用和多步骤对话场景中特别有效。

## 工具定义示例

```json
{
  "name": "think",
  "description": "Use the tool to think about something. It will not obtain new information or change the database, but just append the thought to the log. Use it when complex reasoning or some cache memory is needed.",
  "input_schema": {
    "type": "object",
    "properties": {
      "thought": {
        "type": "string",
        "description": "A thought to think about."
      }
    },
    "required": ["thought"]
  }
}
```

## 与 Extended Thinking 的区别

| 特性 | Extended Thinking | Think 工具 |
|------|-------------------|------------|
| 触发时机 | 响应生成**之前** | 响应生成**过程中** |
| 思考内容 | 深入迭代计划 | 聚焦新获得的信息 |
| 适用场景 | 编程、数学、非顺序工具调用 | 长链工具调用、顺序决策 |

## 最佳实践

1. **领域特定的策略性提示**：在 System Prompt 中提供关于何时使用 Think 工具的清晰指令和示例
2. **复杂指导放系统提示**：当指令较长时，将 Think 工具说明放在 System Prompt 而非工具描述中效果更好
3. **配合提示工程**：提供针对特定用例的决策树和详细程度期望

## 适用场景

- 复杂工具调用和长链工具调用
- 分析工具输出并决定下一步
- 在规则繁多的环境中导航详细指南
- 顺序决策（每步建立在前一步之上）

## 不适用场景

- 非顺序工具调用（单次或并行调用）
- 简单指令遵循
- Claude 默认行为已足够好的情况

## 性能提升

基于 τ-Bench 实验，Think 工具配合提示工程可显著提升 Claude 3.7 Sonnet 在复杂任务上的性能，航空领域 pass@1 相对基线提升约 76%。

## 关系

- [[extended-thinking (待创建)]] — 相关但不同的思考能力
- [[tau-bench]] — 评估基准
- [[agent-tool-design]] — 工具设计方法论
- [[claude-code]] — 实现的载体