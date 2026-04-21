---
title: DeepSeek
created: 2026-04-15
updated: 2026-04-15
type: entity
tags: [company, llm, open-source]
sources:
  - "腾讯技术工程 - 详尽地带你从零开始设计实现一个 AI Agent 框架"
---

# DeepSeek

## 什么是 DeepSeek

DeepSeek 是一个 AI 模型提供商，其 deepseek-chat 模型被用于本文的极简 Agent 框架实现。选择 DeepSeek 的主要考量因素是：模型支持 Tool Calls，并且完全兼容 OpenAI 的 SDK。

## 关键事实

- **模型**: deepseek-chat（用于 Agent 的 LLM Provider）
- **特性**: 支持 Tool Calls / Function Calling
- **兼容性**: 完全兼容 OpenAI SDK（标准化设计）
- **API**: 使用标准化的 OpenAI SDK 进行 LLM Call
- **官网**: https://platform.deepseek.com
- **API Keys 获取**: https://platform.deepseek.com/api_keys

## 在 Agent 框架中的角色

在本文实现的极简 Agent 框架中，DeepSeek 作为核心的 LLM Provider，承担：
- 理解用户请求
- 推理与规划
- 生成 Tool Call 请求
- 格式化响应

## 相关概念

- [[agent-architecture-design]]
- Tool Calls / Function Calling（OpenAI 标准）
- LLM Provider
- [[context-management-short-long-term-memory]]（Context Engineering 上下文工程）