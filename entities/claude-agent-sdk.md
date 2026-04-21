---
title: Claude Agent SDK
created: 2026-04-14
updated: 2026-04-21
type: entity
tags: [framework, tool-use, coding-agent, autonomous-agent]
sources:
  - "https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk"
  - "Notion Page ID: 34267b21-8207-8141-8638-c436b190d370"
---

# Claude Agent SDK

## 什么是 Claude Agent SDK

Claude Agent SDK 是 Anthropic 提供的一套用于构建 Agent 的工具集，原名为 Claude Code SDK。它源自 Anthropic 内部开发者生产力工具 Claude Code，如今已发展为驱动 Anthropic 几乎所有主要 Agent 循环的核心框架。

SDK 的核心设计理念是"给 Claude 一台电脑"——通过终端访问让 Claude 获得与人类程序员相同的工作环境，包括文件查找、代码编写与编辑、lint、运行、调试等能力。

## 关键事实

- **原名**：Claude Code SDK，后重命名为 Claude Agent SDK 以反映更广阔的愿景
- **设计原则**：让 Claude 使用与程序员相同的工具
- **核心能力**：文件操作、bash 命令执行、代码生成与调试、MCP 集成
- **适用场景**：金融 Agent、个人助理 Agent、客户支持 Agent、深度研究 Agent
- **验证机制**：支持基于规则验证、视觉反馈、LLM as a Judge 等多种验证方式

## 关键概念

Claude Agent SDK 的核心框架是 [[agent-loop|Agent Loop]]：

1. **Gather Context** - 收集上下文（搜索文件、读取数据、获取用户输入）
2. **Take Action** - 执行操作（编写代码、调用工具、执行命令）
3. **Verify Work** - 验证工作（规则检查、视觉验证、LLM 评判）
4. **Repeat** - 重复迭代直到完成目标

## 工具设计原则

SDK 建议工具设计遵循以下原则：

- **主要操作**：工具应该是 Agent 采取的主要操作
- **上下文效率**：工具设计应最大化上下文效率
- **可组合性**：代码是精确的、可组合的、无限可复用的，是 Agent 理想输出格式

## 与 MCP 的关系

Claude Agent SDK 支持 [[model-context-protocol|MCP (Model Context Protocol)]]，提供与外部服务的标准化集成，自动处理认证和 API 调用，可连接 Slack、GitHub、Google Drive、Asana 等工具。

## 相关链接

- 官方文档：https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk
- [[claude-code|Claude Code]] - SDK 的起源产品
- [[anthropic|Anthropic]] - 开发商
- [[agent-loop|Agent Loop]] - 核心设计框架
- [[agent-tool-design|Agent 工具设计原则]]
