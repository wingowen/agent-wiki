---
title: CLAUDE.md
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [agent, coding-agent, prompt-engineering, framework]
sources:
  - https://www.anthropic.com/engineering/claude-code-best-practices
---

## What it is

CLAUDE.md 是 Claude Code 在每次对话启动时自动加载到上下文中的特殊配置文件。它是 Claude Code 上下文工程策略的核心组件，让用户可以在项目级别预设指导信息，使 Agent 每次都能获取一致的项目上下文。

## Key facts

1. **自动加载**：Claude 在启动对话时会自动将 CLAUDE.md 文件内容加载到上下文中

2. **配置内容**：
   - 常用的 bash 命令
   - 核心文件和工具函数
   - 代码风格指南
   - 测试说明
   - 仓库规范（如分支命名、merge vs. rebase 等）
   - 开发者环境设置（如 pyenv 使用、编译器版本等）
   - 项目特有的意外行为或警告

3. **CLAUDE.local.md**：可以使用 CLAUDE.local.md 覆盖全局配置，实现本地个性化设置

4. **与 .claude 目录的关系**：在项目根目录创建 .claude 目录，可放置 CLAUDE.md 和 CLAUDE.local.md

## Relationships

- **工具**：[[claude-code|Claude Code]]
- **相关概念**：[[context-engineering|上下文工程]]、[[agentic-coding|Agentic Coding]]