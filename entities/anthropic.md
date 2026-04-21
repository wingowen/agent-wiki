---
title: Anthropic
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [company, agent, reasoning-agent]
sources:
  - https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues
  - https://www.anthropic.com
---

# Anthropic

Anthropic 是一家专注于人工智能安全性和有益性的 AI 研究公司与实验室，由前 OpenAI 研究人员于 2021 年创立。公司总部位于美国，致力于开发可解释、可控且安全的 AI 系统。

## 核心产品

Anthropic 开发了多代 Claude 模型，包括：

- **Claude Haiku 3.5** - 轻量级快速模型
- **Claude Sonnet 4** - 中等规模平衡模型
- **Claude Opus 4** - 高性能旗舰模型
- **Claude Opus 3** - 前一代旗舰模型

## Claude Code

Claude Code 是 Anthropic 官方开发的命令行编码 Agent 工具，展示了上下文工程等多种 Agent 技术的实践应用。

## 基础设施部署

Anthropic 通过自有 API、Amazon Bedrock 和 Google Cloud Vertex AI 向数百万用户提供 Claude 服务。Claude 部署在多个硬件平台上：

- AWS Trainium
- NVIDIA GPU
- Google TPU

## 2025年9月故障复盘

2025年9月17日，Anthropic 工程团队发布了《三个近期问题的故障复盘》技术报告，披露了三个导致 Claude 响应间歇性降级的基础设施 Bug：

1. **Context Window 路由错误** - 8月5日引入，短上下文请求被错误路由到 1M token Context Window 服务器
2. **输出损坏** - 8月25日部署的错误配置导致 TPU 服务器生成乱码
3. **Approximate Top-k XLA:TPU 编译错误** - 混合精度算术问题导致 token 采样错误

## 相关链接

- 官网：https://www.anthropic.com
- 工程博客：https://www.anthropic.com/engineering

## 关联页面

- [[claude-code|Claude Code]] - Anthropic 官方 CLI 编码 Agent
- [[claude-research]] - Anthropic 多 Agent 研究系统
- [[deepseek]] - 另一家 AI 模型提供商
