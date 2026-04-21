---
title: MCP Server Packaging
created: 2026-04-14
updated: 2026-04-21
type: concept
tags: [mcp, tool-use, open-source]
sources:
  - https://www.anthropic.com/engineering/desktop-extensions
---

## Definition

MCP Server Packaging（MCP 服务器打包）是指将 MCP 服务器及其运行时依赖封装为独立可分发单元的技术实践，旨在简化和标准化 AI Agent 与外部工具集成的安装流程。[[desktop-extensions|Desktop Extensions]] 通过 [[mcpb-format|MCPB]] 格式实现的"下载-双击-安装"范式，代表了该领域的重要突破。

## Key Techniques

1. **依赖封装**：将 Node.js、Python 等运行时与服务器代码打包在一起，解放用户免于手动安装环境。

2. **manifest.json 配置管理**：通过声明式配置定义扩展元数据、入口点、平台适配、用户配置模板，支持模板变量（${__dirname}、${user_config.key}）实现动态参数化。

3. **密钥链集成**：敏感配置（API 密钥等）自动路由至操作系统密钥链，而非明文存储在配置文件中。

4. **跨平台适配**：通过 platforms 字段分别为不同操作系统指定差异化命令和环境变量，实现一次打包多平台运行。

5. **目录发现机制**：内置扩展目录提供浏览、搜索和一键安装功能，替代传统的 GitHub 手动搜索。

## Related Entities and Concepts

- **核心实体**：[[desktop-extensions|Desktop Extensions]]、[[mcpb-format|MCPB Format]]、[[model-context-protocol|Model Context Protocol]]
- **工具链**：@anthropic-ai/mcpb
- **应用**：[[claude-code|Claude Code]]（用于构建扩展）、Claude Desktop
- **相关概念**：[[tool-use|工具使用]]、open-source、standard