# Wiki Index

> Content catalog. Every wiki page listed under its type with a one-line summary.
> Read this first to find relevant pages for any query.
> Last updated: 2026-04-21 | Total pages: 37

## Entities

- [[Dream]] — nanobot 记忆管理组件：通过两阶段（分析+编辑）实现长期记忆文件的手术式更新
- [[Consolidator]] — nanobot 记忆压缩组件：轻量级将会话消息压缩为 history.jsonl
- [[entities/dual-agent-architecture|Dual Agent Architecture]] — 双重 Agent 架构：Initializer Agent + Coding Agent 解决跨 Context Window 连续性问题
- [[entities/clean-state|Clean State]] — 干净状态：每个会话结束时应保持的代码可合并状态
- [[entities/ken-aizawa|Ken Aizawa]] — Anthropic 研究员，Agent 工具设计文章作者
- [[command-router]] — 命令路由系统：支持 priority/exact/prefix/interceptors 四种命令类型
- [[agent-hook]] — Agent 生命周期 Hook 基类：提供 agent 执行各阶段的拦截点
- [[openclaw|OpenClaw]] — AI Agent 框架：2026 年热门框架，为 AI Agent 带来新想象
- [[deepseek|DeepSeek]] — AI 模型提供商：deepseek-chat 模型支持 Tool Calls
- [[entities/claude-code|Claude Code]] — Anthropic 官方 CLI 编码 Agent，展示了上下文工程的多种实践
- [[entities/model-context-protocol|Model Context Protocol (MCP)]] — 标准化工具协议，上下文工程的关键组件
- [[Claude Research]] — Anthropic 多 Agent 研究系统：Lead Agent + Subagent 层级架构
- [[SessionManager]] — 会话历史管理器：管理会话上下文，影响工具选择决策
- [[web_search]] — 网络搜索工具：Agent 系统中获取实时信息的常用工具
- [[entities/anthropic|Anthropic]] — AI 安全研究公司，Claude 模型开发商
- [[entities/claude-haiku|Claude Haiku]] — Anthropic 轻量级模型
- [[entities/subagent|Subagent]] — Claude Code 子代理机制：在独立上下文中运行任务
- [[entities/claude-md|CLAUDE.md]] — Claude Code 配置文件：自动加载的项目级上下文
- [[entities/desktop-extensions|Desktop Extensions]] — Claude Desktop 一键安装 MCP 服务器的打包格式
- [[entities/mcpb-format|MCPB Format]] — MCP Bundle 打包格式，MCP 服务器的一键分发标准
- [[entities/contextual-retrieval|Contextual Retrieval]] — Anthropic 发布的上下文检索技术，降低检索失败率 49%
- [[entities/claude-code-sandboxing|Claude Code Sandboxing]] — 文件系统和网络双重隔离，减少 84% 权限提示
- [[entities/claude-code-on-the-web|Claude Code on the Web]] — 云端隔离执行 Claude Code，安全处理 git 凭据
- [[entities/tool-search-tool|Tool Search Tool]] — Claude 动态工具发现功能，BM25 搜索按需加载工具
- [[entities/programmatic-tool-calling|Programmatic Tool Calling]] — 通过代码编排工具执行，减少 Token 消耗 37%
- [[entities/claude-agent-sdk|Claude Agent SDK]] — Anthropic Agent 构建工具集，核心框架为 Agent Loop
- [[entities/think-tool|Think 工具]] — Anthropic 为 Claude 设计的反思性推理工具，提升复杂工具使用场景性能
- [[entities/tau-bench|τ-Bench]] — Sierra Research 开发的 Agent 评估基准，专注于客户服务场景的工具使用能力

## Concepts

- [[dual-layer-memory-system|双层记忆系统与记忆流动管理]] — 短期记忆（会话消息）与长期记忆（知识文件）的分层架构及流动机制
- [[concepts/agent-loop|Agent Loop]] — Agent 执行核心循环：Gather Context → Take Action → Verify Work → Repeat

- [[concepts/agent-tool-design|Agent 工具设计原则]] — 为 AI Agent 设计高效工具的核心方法论（评估驱动、工具整合、命名空间、上下文返回、Token 效率）
- [[agent-architecture-design]] — Agent 架构设计：从传统开发到智能体
- [[rag-retrieval-augmented-generation]] — RAG 检索增强生成：从分块到检索
- [[hyde-hypothetical-document-embedding]] — HyDE 假设文档嵌入
- [[context-management-short-long-term-memory]] — 上下文管理：短期与长期记忆
- [[langgraph-core-principles-react]] — LangGraph 核心原理与 ReAct 对比
- [[agent-interview-qa-overview]] — AI Agent 面试突击问答清单（总览）
- [[slash-command-hook-design]] — 自定义 Slash 命令 Hook 设计方案
- [[agent-framework-design-principles]] — AI Agent 框架设计原则：从零设计实现 Agent 的核心要素
- [[context-engineering]] — 上下文工程：从提示词工程演进，管理 Agent 上下文状态的艺术与科学
- [[multi-agent-system]] — 多 Agent 系统：多个 AI Agent 协同工作的架构
- [[session-context-skill-selection]] — Session Context 影响技能选择
- [[concepts/top-p-sampling|Top-p Sampling]] — 文本生成采样技术，控制输出多样性与质量
- [[concepts/code-execution-with-mcp|通过 MCP 实现代码执行]] — 使用代码执行模式提高 Agent 效率，解决工具定义过载问题
- [[concepts/skills-agent-capability-reuse|Skills（Agent 技能复用模式）]] — 可重用代码的组织模式，允许 Agent 积累和复用能力
- [[concepts/progressive-disclosure|Progressive Disclosure（渐进式披露）]] — 信息设计原则，通过三层结构避免 Context Window 过载
- [[concepts/mcp-server-packaging|MCP Server Packaging]] — MCP 服务器打包：Desktop Extensions 一键安装的技术实现
- [[concepts/contextual-embedding|Contextual Embedding]] — 为分块生成上下文说明再嵌入的技术，解决传统 RAG 上下文丢失问题
- [[concepts/agentic-coding|Agentic Coding]] — Agentic Coding：AI Agent 自主执行编程任务的实践方法
- [[concepts/agent-sandboxing|Agent 沙箱化]] — 通过文件系统和网络隔离提升 Agent 安全性，同时减少权限提示
- [[concepts/advanced-tool-use|高级工具使用]] — Claude 开发者平台三大 Beta 功能：工具发现、执行和参数准确性
- [[concepts/reflective-reasoning-tool|反思性推理工具]] — 让 Agent 在执行过程中暂停并进行结构化思考的机制

## Comparisons

## Queries
