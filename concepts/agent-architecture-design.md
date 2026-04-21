---
title: "Agent 架构设计：从传统开发到智能体"
created: 2026-04-15
updated: 2026-04-21
type: concept
tags: [agent, architecture, framework, planning, tool-use]
sources: [notion:34367b21-8207-8163-9cdf-c8c77b1efa54]
---

# Agent 架构设计：从传统开发到智能体

## 核心定义

AI Agent 是能够自主感知环境、做出决策、执行动作的智能系统。与传统程序不同，Agent 不是按预设路径执行，而是根据目标动态规划行动序列。

## 传统开发 vs Agent 开发：根本区别

| 维度 | 传统开发 | Agent 开发 |
|------|---------|-----------|
| 决策方式 | 规则/代码预设 | LLM 动态推理 |
| 执行路径 | 固定流程 | 动态规划 |
| 错误处理 | try-catch 预设 | 自我反思与修正 |
| 扩展方式 | 代码修改 | Prompt 调整 |

## Agent 核心架构组件

### 1. Perception（感知层）
- 用户输入解析
- 工具调用结果解析
- 多模态信息融合

### 2. Brain/Reasoning（推理层）
- 核心推理引擎（LLM）
- 规划与目标分解
- 短期记忆管理

### 3. Action（行动层）
- 工具选择与调用
- 对话/文档输出
- 外部系统对接

## C 端用户偏好实现

- **用户画像向量化**：将用户偏好转为 embedding 存入向量数据库
- **冷启动策略**：新用户用热门/通用偏好 + 逐步个性化
- **偏好更新机制**：基于交互反馈持续更新用户向量

## 相关概念

- [[Tool-Use]] — Agent 调用外部工具的能力
- [[Planning]] — Agent 的任务分解与规划
- [[Memory]] — Agent 的短期与长期记忆
- [[ReAct]] — 推理与行动结合的模式
