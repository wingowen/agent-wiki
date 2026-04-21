---
title: Desktop Extensions
created: 2026-04-14
updated: 2026-04-21
type: entity
tags: [mcp, tool-use, open-source, framework]
sources:
  - https://www.anthropic.com/engineering/desktop-extensions
---

## What it is

Desktop Extensions（桌面扩展）是 [[deepseek|Anthropic]] 推出的一种全新打包格式（.mcpb 文件，即 MCP Bundle），旨在让 MCP 服务器的安装变得像点击按钮一样简单。它将整个 MCP 服务器及其所有依赖打包成一个可安装的包，取代了此前需要手动安装运行时、编辑配置文件、管理依赖的复杂流程。

## Key facts

1. **一键安装**：用户只需下载 .mcpb 文件并拖入 Claude Desktop 设置窗口，即可完成安装，无需任何开发者工具或命令行操作。

2. **依赖封装**：扩展将 MCP 服务器及其所有依赖打包在一起，用户无需手动解决包冲突或版本不匹配问题。

3. **跨平台支持**：支持 Windows（win32）、macOS（darwin）和 Linux 平台，可通过 manifest.json 中的 platforms 配置实现平台适配。

4. **开源规范**：Anthropic 将 Desktop Extension 规范、工具链（@anthropic-ai/mcpb）和参考实现全部开源，版本号为 0.1。

5. **企业级功能**：支持组策略（Windows）、MDM（macOS）、私有扩展目录部署、扩展黑名单等企业安全管理功能。

6. **安全特性**：敏感数据自动保存在操作系统密钥链中，支持自动更新和安装审计。

## Relationships

- **创造者**：[[deepseek|Anthropic]] Engineering Team
- **核心组件**：[[model-context-protocol|MCP]] 服务器、manifest.json 配置文件
- **工具链**：@anthropic-ai/mcpb（npm 包）
- **使用场景**：通过 [[claude-code|Claude Code]] 构建扩展
- **相关概念**：[[tool-use|工具使用]]、open-source