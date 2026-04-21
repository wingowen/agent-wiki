---
title: Claude Code on the Web
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [coding-agent, safety, framework]
sources:
  - https://www.anthropic.com/news/claude-code-on-the-web
  - https://docs.claude.com/en/docs/claude-code/claude-code-on-the-web
---

## What it is

Claude Code on the Web 是 [[claude-code]] 的云端版本，允许用户在隔离沙箱中运行 Claude Code，实现安全的云端编程辅助体验。

## Key facts

1. **云端隔离执行**：每个会话在隔离沙箱中运行，完全访问其服务器，同时确保敏感凭据（git 凭据、签名密钥）永不存放在沙箱内

2. **安全代理服务**：通过自定义代理服务（Proxy Service）透明处理所有 git 交互，使用范围限定凭据（Scoped Credential）认证

3. **git 集成安全**：代理验证凭据和 git 交互内容（如确保只推送到配置的分支），然后附加正确的认证令牌

4. **防护设计**：即使沙箱中代码被攻破，用户也能免受进一步损害

## Relationships

- **所属产品**：[[claude-code]]
- **类似功能**：[[claude-code-sandboxing|Claude Code Sandboxing]]（本地沙箱版本）
- **开发者**：[[anthropic|Anthropic]]
- **相关概念**：[[agent-sandboxing|Agent 沙箱化]]、范围限定凭据
