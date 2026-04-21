---
title: Tool Search Tool
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [tool-use, agent, beta-feature]
sources:
  - https://www.anthropic.com/engineering/advanced-tool-use
---

## What it is

Tool Search Tool 是 Claude 开发者平台推出的 Beta 功能（类型标识：`tool_search_tool_regex_20251119`），用于实现动态工具发现。该功能允许 Claude 在需要时从大量工具库中按需搜索合适的工具，而无需预先将所有工具定义加载到上下文窗口中。

## Key facts

- **功能类型**: Beta 功能，需要添加 `betas=["advanced-tool-use-2025-11-20"]` 头部
- **搜索算法**: 使用 BM25（Best Matching 25）算法进行相关性搜索
- **Token 节省**: 在 100+ 工具场景下可节省约 85% 的 Token
- **延迟加载**: 通过 `defer_loading=true` 标记的工具默认不加载，仅在需要时搜索和加载
- **搜索范围**: 覆盖工具名称、描述和参数定义
- **与 Prompt Caching 兼容**: 延迟加载的工具不会出现在初始提示中，可与缓存机制协同工作

## Relationships

- **提供商**: [[anthropic]]（Claude 开发者平台团队）
- **相关功能**: [[programmatic-tool-calling]]（编程式工具调用）、[[tool-use-examples (待创建)]]（工具使用示例）
- **相关概念**: [[model-context-protocol]]（MCP 协议）、[[context-engineering|上下文工程]]（Token 优化理念）
- **生态位置**: Agent 工具使用流程中的"发现"环节
