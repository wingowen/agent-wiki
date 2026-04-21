---
title: Claude Research
created: 2026-04-14
updated: 2026-04-21
type: entity
tags: [agent, research-agent, autonomous-agent, company]
sources: ["https://www.anthropic.com/engineering/multi-agent-research-system"]
---

# Claude Research

## What it is

Claude Research 是 Anthropic 开发的[[multi-agent-system|多 Agent 研究系统]]，被用于 Claude 的 Research 功能。该系统通过多个 AI Agent 协同工作，更高效地探索复杂研究主题。

## Key Facts

- **发布方**: Anthropic Engineering
- **核心架构**: Lead Agent（主导研究员）+ Subagent（子 Agent）层次结构
- **性能提升**: 在内部研究评估中，比单 Agent Claude Opus 4 性能提升 **90.2%**
- **Token 效率**: 仅 Token 消耗就解释了 95% 的性能差异；升级到 Claude Sonnet 4 比将 Token 预算翻倍效果更好
- **并行化**: Subagent 可同时探索问题的不同方面，实现信息压缩和关注点分离

## Architecture

Claude Research 采用层级 [[agent-architecture-design|Agent 架构设计]]:

1. **Lead Agent**: 负责任务规划、Subagent 协调、结果整合
2. **Subagent**: 并行执行搜索和信息收集，通过各自的 [[context-management-short-long-term-memory|上下文窗口]] 独立工作
3. **信息压缩**: Subagent 将最重要的 Token 压缩传递给 Lead Agent

## Key Techniques

- **搜索即压缩**: 从海量语料库中提取洞察的过程
- **关注点分离**: 不同工具、Prompt 和探索轨迹减少路径依赖
- **来源质量启发式规则**: 避免被 SEO 优化的内容误导，选择权威来源

## Relationships

- [[anthropic]] — 开发该系统的公司
- [[multi-agent-system]] — 底层系统架构概念
- [[llm-as-judge (待创建)]] — 用于评估输出质量的方法
- [[prompt-injection]] — 安全考量之一
- [[deepseek]] — 其他 AI 模型提供商
