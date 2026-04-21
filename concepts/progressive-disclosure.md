---
title: Progressive Disclosure（渐进式披露）
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [agent, architecture, memory, context-engineering]
sources:
  - https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
---

## Definition

Progressive Disclosure（渐进式披露）是一种信息设计原则，源自人类接口设计领域，核心思想是：只在用户需要时才展示详细信息，避免信息过载。

在 Agent Skills 架构中，Progressive Disclosure 体现在三层信息结构：
1. **第一层**：YAML 元数据（name + description）——Agent 启动时预加载
2. **第二层**：SKILL.md 正文——当 Claude 认为 Skill 与当前任务相关时加载
3. **第三层及以后**：Skill 目录中的额外文件——Claude 可根据需要选择性导航

## Key techniques

### 1. 元数据预加载机制

Agent 启动时，系统提示词包含每个已安装 Skill 的 name 和 description，使 Claude 能够在早期判断应该使用哪些 Skill，而无需加载完整的 Skill 内容。

### 2. 按需加载策略

只有当 Claude 认为某个 Skill 与当前任务相关时，才调用工具（如 Bash）读取 SKILL.md 的完整内容。这种惰性加载避免了 Context Window 被不必要的信息填满。

### 3. 选择性文件导航

Skill 目录中的额外文件（如 forms.md、scripts 等）只有在 Claude 明确需要时才被读取。这使得可以打包到 Skill 中的上下文量实际上是无限的。

### 4. 代码执行解耦

当 Skill 包含预编写脚本时（如 Python PDF 处理脚本），Claude 可以直接执行脚本而无需将脚本代码或相关数据加载到上下文中。代码执行结果以确定性的方式返回。

## 为什么重要

- **Token 效率**：避免将 Skill 的全部内容读入 Context Window
- **信息聚焦**：Agent 只在需要时获取相关信息，减少干扰
- **无限上下文**：理论上可以打包无限量的上下文到 Skill 中
- **可组合性**：不同的 Skill 可以独立开发、测试和分发

## Related entities and concepts

- **核心实现**：[[agent-skills|Agent Skills（Anthropic 实现）]]
- **相关概念**：[[skills-agent-capability-reuse|Skills（Agent 技能复用模式）]]
- **上下文工程**：[[context-engineering|上下文工程]]（Progressive Disclosure 是上下文工程的核心技术）
- **记忆架构**：[[context-management-short-long-term-memory|上下文管理：短期与长期记忆]]（ Progressive Disclosure 也是记忆管理的策略）
- **代码执行**：[[code-execution-with-mcp|通过 MCP 实现代码执行]]（Skill 中代码执行的模式）
