---
title: Agentic Coding
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [agent, coding-agent, framework, prompt-engineering]
sources:
  - https://www.anthropic.com/engineering/claude-code-best-practices
---

## Definition

Agentic Coding（代理编程）是一种利用 AI Agent 执行编程任务的实践方法。与传统辅助编程不同，Agentic Coding 赋予 AI Agent 更大的自主权，使其能够自主探索代码库、规划实现方案、执行多步骤任务并提交最终成果。Claude Code 作为这一领域的代表性工具，展示了如何将大型语言模型深度集成到软件开发工作流中。

## Key Techniques

### 1. 探索 → 规划 → 编码 → 提交 工作流

Anthropic 最推荐的核心工作流：

1. **探索（Explore）**：让 Agent 先理解代码库结构和问题背景
2. **规划（Plan）**：在编写代码之前，先让 Agent 制定实现计划
3. **编码（Code）**：Agent 根据计划执行代码编写
4. **提交（Commit）**：验证后提交变更

这种模式与人类工程师的最佳实践一致，强调"先理解问题，再解决问题"。

### 2. 测试驱动开发（TDD）

Claude Code 支持的 TDD 工作流：

1. 让 Agent 编写测试用例（基于预期输入/输出）
2. 运行测试确认失败
3. 提交测试
4. 让 Agent 实现代码直到测试通过
5. 提交代码

Agent 在有明确目标（如测试）可以迭代时表现最佳。

### 3. 迭代式开发

Claude 的输出在迭代后会显著改善。典型模式：

- **视觉迭代**：给 Agent 设计截图 → 实现 → 截图对比 → 迭代
- **测试迭代**：编写测试 → 运行 → 调整代码 → 再运行

通常 2-3 次迭代后质量会大幅提升。

### 4. CLAUDE.md 配置

创建项目级配置文件，包含：
- 常用 bash 命令
- 核心文件和工具函数
- 代码风格指南
- 测试说明
- 仓库规范
- 开发者环境设置
- 项目特有的警告信息

该文件在每次对话启动时自动加载到上下文中。

## Related Entities and Concepts

- **工具**：[[claude-code|Claude Code]]
- **公司**：[[deepseek|Anthropic]]
- **相关概念**：[[context-engineering|上下文工程]]、[[agent-architecture-design|Agent 架构设计]]
- **相关技术**：[[model-context-protocol|Model Context Protocol (MCP)]]