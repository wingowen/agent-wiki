---
title: CronTool
created: 2026-04-15
updated: 2026-04-15
type: entity
tags: [tool-use, agent]
sources:
  - NotionPageId: 34367b21-8207-8140-9199-e7fefb5ffb3c
---

# CronTool

## 什么是 CronTool

CronTool 是 nanobot 系统中用于调度提醒和周期性任务的工具。其功能描述通过硬编码提示词注入到代理的上下文中，指导代理正确使用该工具。

## 关键事实

- **提示词内容**：`Tool to schedule reminders and recurring tasks.`
- **注入方式**：硬编码在代码中，定制性低
- **参数说明**：包含详细的参数说明和使用示例，指导用户如何使用 cron 工具

## 关系

### 相关概念
- [[tool-use]] — 工具使用
- [[prompt-engineering]] — 提示词工程

### 相关实体
- [[nanobot]] — 使用 CronTool 的 AI Agent 框架
