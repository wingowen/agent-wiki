---
title: Agent 沙箱化
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [safety, autonomous-agent, coding-agent]
sources:
  - https://www.anthropic.com/engineering/claude-code-sandboxing
---

## Definition

Agent 沙箱化（Agent Sandboxing）是一种安全技术，通过为 AI Agent 创建预定义的隔离边界，使其能够在受限环境中自由执行操作，同时防止恶意输入或被攻破的 Agent 对系统造成损害。沙箱化同时提升了安全性和自主性——定义好边界后，Agent 无需为每个操作请求权限。

## Key techniques

1. **文件系统隔离**
   - 限制 Agent 只能访问特定目录
   - 防止修改系统文件或敏感配置
   - 在 [[prompt-injection|提示注入]] 攻击场景下尤为重要

2. **网络隔离**
   - 限制 Agent 只能连接已批准的服务器
   - 防止数据泄露或下载恶意软件
   - 通过代理服务器实现域名级访问控制

3. **操作系统级原语**
   - Linux: bubblewrap（轻量级容器化，通过 Linux 命名空间隔离进程）
   - MacOS: seatbelt（苹果沙箱框架，通过策略配置文件限制进程能力）

4. **范围限定凭据（Scoped Credential）**
   - 权限受限的身份验证凭据
   - 例如：仅允许向特定仓库的特定分支推送代码

5. **审批疲劳（Approval Fatigue）解决方案**
   - 传统权限模型要求用户频繁点击"批准"
   - 沙箱化通过定义边界实现自动放行安全操作、阻止恶意操作

## Related entities and concepts

- **实践案例**：[[claude-code-sandboxing|Claude Code Sandboxing]]、[[claude-code-on-the-web|Claude Code on the Web]]
- **核心产品**：[[claude-code]]
- **开发者**：[[anthropic|Anthropic]]
- **相关威胁**：[[prompt-injection|提示注入]]
- **开源实现**：sandbox-runtime（https://github.com/anthropic-experimental/sandbox-runtime）
- **相关概念**：[[agent-architecture-design|Agent 架构设计]]、权限模型
