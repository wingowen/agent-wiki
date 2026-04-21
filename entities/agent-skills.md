---
title: Agent Skills（Anthropic 实现）
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [agent, tool-use, architecture, framework]
sources:
  - https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
  - https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview
---

## What it is

Agent Skills 是 Anthropic 在 2025 年 10 月发布的一种为 AI Agent 装备专业化能力的技术框架。通过将指令、脚本和资源打包在文件夹中，Agent 可以动态发现和加载 Skills 以在特定任务上表现出更好的专业性。

核心思想是：不需要为每个用例构建定制化的碎片化 Agent，而是通过捕获和共享程序性知识来用可组合的能力专业化 Agent。

## Key facts

### 结构组成

- **SKILL.md 文件**：每个 Skill 目录中必须包含的文件，以 YAML frontmatter 开头（包含 name 和 description）
- **目录结构**：Skill 是一个包含 SKILL.md 文件的目录，可包含额外资源文件
- **代码执行**：Skill 可以包含预编写的脚本（如 Python），Claude 可自行决定执行

### 三层渐进式披露（Progressive Disclosure）

1. **第一层 - YAML 元数据**（name + description）：Agent 启动时预加载，让 Claude 知道何时使用每个 Skill
2. **第二层 - SKILL.md 正文**：当 Claude 认为 Skill 与当前任务相关时，加载完整内容
3. **第三层及以后 - 额外文件**：Claude 可根据需要选择性导航和发现

### Context Window 交互

当用户消息触发 Skill 时：
1. 初始状态：Context Window 包含核心系统提示词和每个已安装 Skill 的元数据
2. Claude 调用 Bash 工具读取 Skill 文件
3. Claude 选择性读取 Skill 附带的额外文件
4. Claude 已加载相关指令，继续处理任务

### 安全特性

- Skills 通过指令和代码为 Claude 提供新能力
- ⚠️ 只从可信来源安装 Skills
- 使用前彻底审计可疑 Skill
- 注意连接到外部网络源的指令

### 平台支持

- Claude.ai
- Claude Code
- Claude Agent SDK
- Claude Developer Platform

### 未来方向

- Agent 自主创建、编辑和评估 Skills
- 将自己的行为模式编码为可复用能力

## Relationships

- **父级概念**：[[skills-agent-capability-reuse|Skills（Agent 技能复用模式）]]（更通用的技能复用模式）
- **实现平台**：[[claude-code|Claude Code]]（使用 Skills 的 Agent 实现）
- **相关架构**：[[context-engineering|上下文工程]]（Skills 是上下文工程的重要实践）
- **代码执行**：[[model-context-protocol|Model Context Protocol]]（Skills 可包含 MCP 代码执行）
- **核心原则**：[[progressive-disclosure|Progressive Disclosure（渐进式披露）]]（Skill 设计核心原则）
- **安全注意**：[[prompt-injection|Prompt Injection]]（恶意 Skill 风险相关）
