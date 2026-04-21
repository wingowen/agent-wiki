---
title: Programmatic Tool Calling
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [tool-use, agent, code-execution, beta-feature]
sources:
  - https://www.anthropic.com/engineering/advanced-tool-use
---

## What it is

Programmatic Tool Calling（PTC）是 Claude 开发者平台推出的 Beta 功能（类型标识：`code_execution_20250825`），允许 Claude 通过编写 Python 代码来编排和执行工具，而非通过单独的自然语言推理过程逐个调用工具。该功能将工具执行从"对话式"转变为"编程式"，显著提升效率并减少 Token 消耗。

## Key facts

- **核心机制**: Claude 生成 Python 代码在沙箱环境中执行，工具结果由代码处理而非返回模型上下文
- **Token 节省**: 复杂研究任务平均减少 37%（从 43,588 降至 27,297 Token）
- **延迟优化**: 中间结果不进入上下文，200KB 原始数据可压缩至 1KB 结果
- **并行执行**: 支持 `asyncio.gather()` 等模式并行调用多个工具
- **安全机制**: 通过 `allowed_callers` 字段限制哪些执行环境可以调用特定工具
- **准确性提升**: GIA 基准测试从 46.5% 提升至 51.2%；内部知识检索从 25.6% 提升至 28.5%

## 工作原理

1. 将 `code_execution` 工具添加到工具列表，并设置 `allowed_callers` 选择可编程执行的工具
2. Claude 编写 Python 编排代码，通过 `await` 调用工具函数
3. 工具执行在 API 层面处理，工具结果由脚本接收，不进入模型上下文
4. 只有最终输出返回给 Claude

## Relationships

- **提供商**: [[anthropic]]（Claude 开发者平台团队）
- **相关功能**: [[tool-search-tool]]（工具发现）、[[tool-use-examples (待创建)]]（参数准确性）
- **技术基础**: [[code-execution-with-mcp]]（代码执行模式）
- **相关概念**: [[context-engineering|上下文工程]]（Token 管理）、[[model-context-protocol]]（MCP 协议）
- **应用场景**: 批量数据处理、跨工具工作流编排、复杂条件逻辑
