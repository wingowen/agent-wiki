---
title: web_search
created: 2026-04-15
updated: 2026-04-21
type: entity
tags: [tool-use, agent]
sources:
  - Notion ID: 34367b21-8207-81fe-b72e-d5cd215313a6
---

# web_search

## 什么是 web_search

web_search 是 Agent 系统中常用的网络搜索工具，用于获取实时信息和最新动态。在文章示例中，用户询问"金庸和好友最近一周的新动向"时，模型尝试使用 web_search 工具来获取信息。

## 关键事实

1. **工具调用失败场景**：当 web_search 之前调用失败时，模型仍会继续尝试使用该工具，而没有考虑其他可用技能
2. **缺乏成功历史感知**：工具的过往成功/失败信息没有有效传递给模型，导致重复尝试失败方案
3. **需要与技能提示系统配合**：web_search 的正确使用需要技能提示（Skill Prompt）中包含足够的上下文信息
4. **失败不应阻断其他工具选择**：系统应具备智能工具选择能力，在某工具失败后考虑备用方案

## 关联关系

- **相关概念**: [[context-management-short-long-term-memory]] — 会话历史中的失败记录影响工具选择
- **相关组件**: [[command-router]] — 负责工具/命令的路由分发
- **所属系统**: Agent 工具调用系统
- **相关概念**: [[session-context-skill-selection]] — Session Context 如何影响工具选择
