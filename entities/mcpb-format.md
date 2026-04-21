---
title: MCPB Format
created: 2026-04-14
updated: 2026-04-21
type: entity
tags: [mcp, tool-use, open-source, standard]
sources:
  - https://www.anthropic.com/engineering/desktop-extensions
  - https://github.com/anthropics/mcpb
---

## What it is

MCPB（MCP Bundle）是一种打包格式，用于将 MCP 服务器及其所有依赖封装为单一可安装文件（.mcpb），实现 MCP 服务器的一键安装。MCPB 规范（版本 0.1）由 [[deepseek|Anthropic]] 开源，旨在推动 MCP 服务器在不同 AI 桌面应用间的可移植性。

## Key facts

1. **文件格式**：.mcpb 文件实为 ZIP 压缩包，包含 manifest.json（扩展元数据配置）和 server/ 目录（服务器代码及依赖）。

2. **核心配置**：manifest.json 定义扩展的名称、版本、入口点、平台适配、用户配置模板、工具和 prompt 声明等。

3. **模板变量**：支持 ${__dirname}（扩展安装目录）、${user_config.key}（用户配置）、${HOME}/${TEMP}（系统环境变量）等动态占位符。

4. **敏感信息处理**：标记为 sensitive: true 的配置项（如 API 密钥）自动存储在操作系统密钥链中。

5. **打包工具**：通过 @anthropic-ai/mcpb 工具链（mcpb init/pack 命令）进行打包和验证。

6. **跨平台配置**：manifest.json 中的 platforms 字段可分别为 win32、darwin、linux 指定不同的命令和环境变量。

## Relationships

- **创造者**：[[deepseek|Anthropic]]
- **所属系统**：[[desktop-extensions|Desktop Extensions]] 打包格式
- **工具链**：@anthropic-ai/mcpb
- **配置文件**：manifest.json
- **应用场景**：MCP 服务器分发与一键安装
- **相关概念**：[[tool-use|工具使用]]、open-source、standard