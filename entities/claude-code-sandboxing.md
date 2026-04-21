---
title: Claude Code Sandboxing
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [coding-agent, safety, tool-use]
sources:
  - https://www.anthropic.com/engineering/claude-code-sandboxing
  - https://github.com/anthropic-experimental/sandbox-runtime
---

## What it is

Claude Code Sandboxing 是 [[claude-code]] 推出的安全隔离功能，通过操作系统级沙箱技术实现文件系统和网络双重边界，让 Claude 在受限环境中更安全、更自主地运行，同时大幅减少权限提示。

## Key facts

1. **双边界隔离**：
   - **文件系统隔离**：限制 Claude 只能访问当前工作目录，防止修改敏感系统文件
   - **网络隔离**：通过 Unix 域套接字连接外部代理服务器，限制可访问的域名

2. **权限提示减少 84%**：内部使用数据显示沙箱化技术安全地大幅降低了权限提示次数

3. **防护提示注入**：即使发生 [[prompt-injection|提示注入]] 攻击，沙箱也能完全隔离威胁，防止窃取 SSH 密钥或回传数据

4. **技术实现**：
   - Linux: bubblewrap（Linux 命名空间实现进程隔离）
   - MacOS: seatbelt（苹果沙箱执行框架）

5. **开源发布**：sandbox-runtime 已开源，供其他团队构建安全代理参考

## Relationships

- **所属产品**：[[claude-code]]
- **开发者**：[[anthropic|Anthropic]]
- **相关功能**：[[claude-code-on-the-web|Claude Code on the Web]]
- **开源仓库**：https://github.com/anthropic-experimental/sandbox-runtime
- **相关概念**：[[prompt-injection|提示注入]]、沙箱化（[[agent-sandboxing|Agent 沙箱化]]）
