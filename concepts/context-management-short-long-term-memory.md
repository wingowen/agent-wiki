---
title: "上下文管理：短期记忆与长期记忆"
created: 2026-04-15
updated: 2026-04-21
type: concept
tags:
  - agent
  - memory
  - architecture
  - prompt-engineering
sources:
  - notion_id: "34367b21-8207-81c6-8eed-f5368d5630dd"
---

# 上下文管理：短期记忆与长期记忆

## 一、为什么上下文管理是 Agent 的核心？

LLM 的 Context Window（上下文窗口）是有限的（如 GPT-4o 为 128K tokens，Claude 为 200K tokens）。Agent 在运行过程中会不断累积消息（用户输入、LLM 响应、工具执行结果），如果不加管理，很快就会超出窗口限制。

上下文管理的核心目标：**在有限的窗口内，让 Agent 拥有尽可能多的有效信息。**

---

## 二、短期记忆（Short-term Memory）

### 2.1 定义

短期记忆指的是**当前会话/任务**中的上下文信息，通常存储在 messages 列表中，随着对话的进行不断累积。

### 2.2 LangGraph 中的短期记忆实现

#### 方式一：State 直接传递

```python
class AgentState(TypedDict):
    messages: Annotated[list, add_messages]
    # 所有消息都在 State 中，随图的流转自动累积
```

State 直接传递是最简单的方式，所有消息都在 State 中随着图的流转自动累积。

#### 方式二：Checkpointer 快照机制

```python
from langgraph.checkpoint.postgres import PostgresSaver

checkpointer = PostgresSaver.from_conn_string("postgresql://...")
graph.compile(checkpointer=checkpointer)

# 每次调用都会自动保存状态
config = {"configurable": {"thread_id": "user_123"}}
result = graph.invoke(inputs, config=config)
```

Checkpointer 是 LangGraph 内置的状态快照机制，它保存的是**图的完整状态**（包括所有节点的中间结果），支持时间旅行（回到任意检查点重新执行）。

#### 方式三：摘要化策略（Summarization）

当消息列表过长时，使用 LLM 将其压缩为摘要：

```python
def summarize_if_needed(state: AgentState) -> AgentState:
    messages = state["messages"]
    if len(messages) > 10:
        summary_prompt = f"总结以下对话，保留关键信息：{messages}"
        summary = llm.invoke([HumanMessage(content=summary_prompt)])
        return {"messages": [summary]}
    return state
```

摘要化策略可以有效控制 token 消耗，但会丢失细节信息，适用于信息密度不高的对话场景。

### 2.3 短期记忆的挑战

- **累积性**：消息列表随对话进行不断增长
- **上下文污染**：早期对话可能干扰近期任务的推理
- **成本与延迟**：每次调用都包含完整历史，增加 token 消耗和响应延迟

---

## 三、长期记忆（Long-term Memory）

### 3.1 定义

长期记忆是**持久化存储的信息**，如用户偏好、历史行为、知识库等。与短期记忆不同，长期记忆不随会话结束而消失，可以在多次会话间共享。

### 3.2 实现方案

#### 方案一：关系型数据库存储

```python
# 存储用户画像和偏好
import psycopg2

def save_user_preference(user_id: str, key: str, value: str):
    conn = psycopg2.connect("postgresql://...")
    cursor = conn.cursor()
    cursor.execute(
        "INSERT INTO user_preferences (user_id, key, value) "
        "VALUES (%s, %s, %s) "
        "ON CONFLICT (user_id, key) DO UPDATE SET value = %s",
        (user_id, key, value, value)
    )
    conn.commit()

def get_user_preferences(user_id: str) -> dict:
    conn = psycopg2.connect("postgresql://...")
    cursor = conn.cursor()
    cursor.execute(
        "SELECT key, value FROM user_preferences WHERE user_id = %s",
        (user_id,)
    )
    return dict(cursor.fetchall())
```

#### 方案二：向量数据库存储（推荐）

```python
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings

# 存储历史交互的向量化表示
vectorstore = Chroma(
    persist_directory="./user_memory",
    embedding_function=OpenAIEmbeddings()
)

def save_interaction(user_id: str, query: str, response: str, metadata: dict):
    """将用户交互存入向量数据库"""
    vectorstore.add_texts(
        texts=[f"用户问：{query}\nAgent答：{response}"],
        metadatas=[{"user_id": user_id, **metadata}],
        ids=[f"{user_id}_{int(time.time())}"]
    )

def recall_relevant_history(user_id: str, query: str, k: int = 5):
    """检索与当前查询最相关的历史交互"""
    results = vectorstore.similarity_search(
        query, k=k, filter={"user_id": user_id}
    )
    return [doc.page_content for doc in results]
```

向量数据库是长期记忆存储的推荐方案，支持语义检索，可以根据当前上下文动态召回最相关的历史信息。

#### 方案三：Redis 缓存 + 数据库持久化

```python
import redis

# Redis 用于热数据快速访问
redis_client = redis.Redis(host='localhost', port=6379)

def get_cached_preference(user_id: str, key: str):
    cached = redis_client.get(f"pref:{user_id}:{key}")
    if cached:
        return cached.decode()
    # 缓存未命中，从数据库加载
    value = get_from_db(user_id, key)
    if value:
        redis_client.setex(f"pref:{user_id}:{key}", 3600, value)
    return value
```

冷热数据分离策略：Redis 缓存热数据，数据库持久化冷数据，兼顾访问速度和容量。

### 3.3 长期记忆在 Agent 中的应用模式

```
用户输入
  ↓
[检索长期记忆] → 从向量数据库/Redis 获取相关历史
  ↓
[注入 System Prompt] → 将检索到的信息注入上下文
  ↓
[Agent 推理] → LLM 基于增强后的上下文进行推理
  ↓
[更新长期记忆] → 将本次交互存入长期记忆
```

### 3.4 与 [[rag-retrieval-augmented-generation]] 的关系

长期记忆的检索机制与 RAG 技术高度相似。两者都通过向量检索从外部存储中获取相关信息，并注入到 LLM 的上下文中。RAG 更侧重于知识库的检索，而长期记忆更侧重于用户历史交互的回忆。在实际系统中，两者经常结合使用，共同为 Agent 提供信息增强。

---

## 四、面试高频追问

### Q: Checkpointer 和普通数据库存储的区别？

**A:** Checkpointer 是 LangGraph 内置的状态快照机制，它保存的是**图的完整状态**（包括所有节点的中间结果），支持时间旅行（回到任意步骤）。普通数据库存储的是**业务数据**（如用户偏好、历史记录），是应用层面的持久化。两者互补：Checkpointer 管理 Agent 运行状态，数据库管理业务数据。

### Q: 如何解决高并发下的上下文隔离？

**A:** 通过 thread_id 实现逻辑隔离。每个用户请求分配唯一的 thread_id，Checkpointer 根据 thread_id 存取对应的状态。在分布式环境中，可以使用 PostgreSQL 作为 Checkpointer 后端，配合连接池管理并发访问。

### Q: 摘要化策略会不会丢失关键信息？

**A:** 会。这是摘要化的固有缺陷。缓解方案包括：1）保留最近 N 轮原文不摘要；2）使用结构化摘要（按主题分类）；3）将关键信息（如用户 ID、任务目标）始终保留在 System Prompt 中；4）结合向量检索，在需要时从长期记忆中找回细节。

### Q: Context Window 溢出时 Agent 会怎样？

**A:** 会导致 API 调用失败（HTTP 400 错误）。预防措施：1）在每次 LLM 调用前计算 token 数；2）设置安全阈值（如窗口的 80%）；3）超过阈值时触发裁剪策略；4）使用支持更大窗口的模型作为备选。

---

## 五、实用要点总结

- **短期记忆** 管理当前会话状态，核心挑战是控制消息长度和 token 消耗
- **长期记忆** 支撑跨会话信息共享，向量数据库是主流存储方案
- **上下文管理** 是 [[agent-architecture-design]] 中的核心模块，直接影响 Agent 的推理质量
- 实际系统中通常需要**短期+长期记忆的混合架构**，结合 [[rag-retrieval-augmented-generation]] 的检索能力

---

## 相关概念

- [[agent-architecture-design]] — Agent 架构设计的核心概念
- [[rag-retrieval-augmented-generation]] — 与长期记忆检索机制高度相关的技术
- [[Memory]] — 记忆系统的整体框架（待创建）
- [[Context-Window]] — 上下文窗口管理与优化（待创建）
