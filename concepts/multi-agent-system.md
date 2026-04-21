---
title: Multi-Agent System
created: 2026-04-14
updated: 2026-04-21
type: concept
tags: [agent, architecture, research-agent, autonomous-agent]
sources: ["https://www.anthropic.com/engineering/multi-agent-research-system"]
---

# Multi-Agent System | 多 Agent 系统

## Definition

多 Agent 系统（Multi-Agent System）是由多个 AI Agent 协同工作的系统架构。每个 Agent 可以独立执行任务、使用工具并做出决策，多个 Agent 之间通过协调机制共同完成复杂目标。

## Key Techniques

### 1. 信息压缩（Compression）

搜索的本质是压缩——从海量语料库中提取洞察。Subagent 通过在自己的[[context-management-short-long-term-memory|上下文窗口]]中并行操作来促进压缩：

- 同时探索问题的不同方面
- 将最重要的 Token 压缩传递给 Lead Agent
- 实现关注点分离（Separation of Concerns）

### 2. 层级架构

Claude Research 采用 Lead Agent + Subagent 模式：

| 角色 | 职责 |
|------|------|
| Lead Agent | 任务规划、Subagent 协调、结果整合 |
| Subagent | 并行搜索、信息收集、独立探索 |

### 3. 评估方法

**LLM-as-Judge**: 使用大语言模型作为评估者，根据评分标准评判输出质量：

| 评估维度 | 说明 |
|----------|------|
| 事实准确性 | 声明是否与来源一致 |
| 引用准确性 | 引用的来源是否与声明匹配 |
| 完整性 | 是否覆盖了所有要求的方面 |
| 来源质量 | 是否使用了高质量的主要来源 |
| 工具效率 | 是否以合理的次数使用了正确的工具 |

### 4. Prompt 工程原则

1. **定义 Agent 角色和职责边界**
2. **具体说明工具及其正确使用方式**
3. **明确信息传递格式**
4. **让 Agent 自我改进**（元认知）
5. **先广后深**（探索-利用平衡）
6. **设置检查点**
7. **动态调整**搜索方法
8. **识别深度 vs 广度的平衡**

### 5. 生产可靠性挑战

- **有状态 Agent**: 错误会累积，系统必须持久执行代码并处理沿途的错误
- **涌现行为（Emergent Behavior）**: 整体表现出的行为无法通过单独分析其组成部分来预测
- **调试复杂性**: Agent 在不同运行之间是非确定性的，需要完整的生产追踪（Production Tracing）
- **部署策略**: 使用[[agent-architecture-design|彩虹部署]]避免中断正在运行的 Agent

## Performance Insights

关键性能数据（来自 Claude Research）：

- 多 Agent 系统比单 Agent 性能提升 **90.2%**
- Token 消耗解释了 **95%** 的性能差异
- 升级模型版本比增加 Token 预算更有效

## Related Concepts

- [[agent-architecture-design]] — Agent 架构设计原则
- [[context-management-short-long-term-memory]] — 上下文窗口管理
- [[langgraph-core-principles-react]] — 类似的图结构编排
- [[prompt-injection]] — 安全考量
- [[llm-as-judge (待创建)]] (待创建) — LLM 评估方法
- [[emergent-behavior (待创建)]] (待创建) — 涌现行为

## Related Entities

- [[claude-research]] — 具体的 Multi-Agent System 实现
- [[anthropic]] — 开发该技术的公司
