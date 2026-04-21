---
title: LangGraph 核心原理与 ReAct 对比
created: 2026-04-15
updated: 2026-04-21
type: concept
tags:
  - framework
  - agent
  - architecture
  - tool-use
  - planning
  - memory
sources:
  - notion_id: 34367b21-8207-81b2-9f8b-c1218f7b3104
---

# LangGraph 核心原理与 ReAct 对比

## 定义

**ReAct**（Reasoning + Acting）是 Yao 等人于 2022 年提出的 Agent 范式，核心思想是将**推理（Reasoning）**和**行动（Acting）**结合在一个循环中。**LangGraph** 是 LangChain 团队专门为构建**有状态的、多步骤的 Agent 应用**而开发的子框架，基于图（Graph）的架构来实现复杂的 Agent 流程控制。

两者都是 Agent 开发的重要范式，但面向的场景和复杂度有显著差异。

## 一、ReAct 模式回顾

### 1.1 什么是 ReAct？

ReAct 是最早被广泛采用的 Agent 范式之一，通过在 LLM 输出中嵌入特定的 token 格式（Thought、Action、Observation）来实现推理与工具调用的交替进行。

### 1.2 ReAct 的工作流程

```
用户输入 → LLM 推理（Thought）→ 执行工具（Action）→ 观察结果（Observation）→ 再次推理 → ... → 最终回答
```

这是一个**线性单循环**：每次迭代都是"思考→行动→观察"的固定模式。

### 1.3 ReAct 的局限性

- **线性流程**：只能按固定顺序执行，无法实现条件分支
- **状态丢失**：每次调用都是独立上下文，历史推理链需要手动维护
- **调试困难**：一旦执行路径错误，难以回溯和修正
- **难以组合**：多个 ReAct 链之间难以优雅地共享状态

## 二、LangGraph 核心概念

### 2.1 核心组件

LangGraph 的四大核心概念：

| 组件 | 作用 |
|------|------|
| **State** | 贯穿整个图执行流程的共享状态对象 |
| **Node** | 图中的计算节点，代表一个处理步骤 |
| **Edge** | 节点之间的连接，定义数据流向 |
| **Graph** | 将节点和边组织成完整的执行图 |

### 2.2 StateGraph 架构

```python
from langgraph.graph import StateGraph, END

# 定义状态 schema
class AgentState(TypedDict):
    messages: list
    current_step: str
    result: str

# 创建图
graph = StateGraph(AgentState)

# 添加节点
graph.add_node("llm", llm_node)
graph.add_node("tools", tool_node)

# 添加边
graph.add_edge("llm", "tools")
graph.add_edge("tools", END)

# 编译
app = graph.compile()
```

### 2.3 状态管理与消息处理

LangGraph 使用 **Reducer 函数** 来处理状态的并发更新：

- `add_messages`：将新消息追加到列表，而不是替换整个列表
- 如果消息 id 相同，则更新该消息（幂等性保证）
- 这使得多个节点可以安全地修改共享状态

## 三、ReAct vs LangGraph 对比

| 维度 | ReAct | LangGraph |
|------|-------|-----------|
| **架构模式** | 线性循环 | 有向图 |
| **状态管理** | 外部维护 | 内置 State 对象 |
| **流程控制** | 固定顺序 | 条件分支、循环 |
| **错误恢复** | 困难 | 通过状态回溯 |
| **调试能力** | 有限 | 支持断点、检查点 |
| **适用场景** | 简单问答、单一工具调用 | 复杂多步骤任务 |

### 核心区别

**ReAct 本质上是一个预定义好的 while 循环**：

```python
while not done:
    thought = llm.think(query, history)
    action = llm.act(thought)
    obs = execute(action)
    history += [(thought, action, obs)]
```

**LangGraph 则将这个循环本身变成了一个可编程的数据结构**，允许任意拓扑结构的节点连接，支持条件分支、并行分支、循环等图论操作。

## 四、LangGraph 高级特性

### 4.1 持久化与恢复（checkpointer）

LangGraph 内置状态持久化机制，可以在任意步骤中断和恢复执行：

```python
from langgraph.checkpoint.memory import MemorySaver

checkpointer = MemorySaver()
app = graph.compile(checkpointer=checkpointer)

# 恢复执行
config = {"configurable": {"thread_id": "session-123"}}
app.invoke(input, config=config)
```

典型应用场景：
- **会话恢复**：用户关闭页面后重新打开，可以继续之前的对话
- **多轮对话隔离**：通过 thread_id 区分不同用户/会话
- **调试回溯**：可以查看任意步骤的状态

### 4.2 Human-in-the-loop（人机协作）

LangGraph 支持在图的任意节点中断，等待人类确认后再继续：

```python
# 在工具执行前中断，等待人类确认
app = graph.compile(
    checkpointer=checkpointer,
    interrupt_before=["tools"]  # 在执行 tools 节点前中断
)
```

### 4.3 多 Agent 协作

LangGraph 支持将多个 Agent 图组合成一个更大的系统：

```python
# Agent A：负责规划
planner = create_planner_graph()

# Agent B：负责执行
executor = create_executor_graph()

# 组合成多 Agent 系统
super_graph = StateGraph(SuperState)
super_graph.add_node("planner", planner)
super_graph.add_node("executor", executor)
super_graph.add_edge("planner", "executor")
super_graph.add_edge("executor", "planner")  # 反馈循环
```

## 五、相关概念

- [[agent-architecture-design]]：Agent 架构设计的通用原则
- [[rag-retrieval-augmented-generation]]：RAG 作为 Agent 的重要工具
- [[tool-use]]：工具使用是 LangGraph 和 ReAct 的核心能力

## 六、未来待创建页面

- **Agent 持久化与状态恢复**：深入探讨 checkpointer 机制的实现原理和应用场景
- **多 Agent 协作框架**：探讨多个 Agent 之间的通信协议和协调机制

## 七、面试高频追问

**Q: LangGraph 的 add_messages 注解底层做了什么？**

A: `add_messages` 是一个 Reducer 函数，它的逻辑是：当新消息到来时，不是替换整个 messages 列表，而是将新消息**追加**到现有列表中。如果新消息的 id 与已有消息相同，则会**更新**该消息（而不是重复添加）。这使得多个节点可以安全地修改 messages 字段而不互相覆盖。

**Q: conditional_edges 和普通 add_edge 的区别？**

A: `add_edge` 是静态的，固定从节点 A 到节点 B；`add_conditional_edges` 接收一个路由函数，该函数读取当前 State，返回一个字符串键值，映射到不同的目标节点。这使得 Agent 可以根据运行时状态（如 LLM 是否返回了 tool_calls）动态决定下一步走向。

**Q: LangGraph 如何处理工具调用失败？**

A: 在 tool_node 中，可以捕获工具执行异常，将错误信息作为 ToolMessage 返回给 LLM。LLM 会看到错误信息并尝试修正（如换一个工具或调整参数），这就是 LangGraph 自我修复能力的来源。

**Q: LangGraph 和 LangChain 的关系？**

A: LangChain 是一个通用的 LLM 应用框架（提供 Chain、Tool、Memory 等抽象）；LangGraph 是 LangChain 团队专门为构建**有状态的、多步骤的 Agent 应用**而开发的子框架。LangGraph 使用 LangChain 的底层组件（如 ChatModel、Tool），但提供了更精细的流程控制能力。
