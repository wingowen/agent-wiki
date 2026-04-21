---
title: "上下文嵌入 Contextual Embedding"
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [rag, embedding, context-engineering, retrieval, llm, chunking]
sources: [notion:34267b21-8207-813f-b0e2-cae2d942f74c, https://www.anthropic.com/engineering/contextual-retrieval]
---

# 上下文嵌入 Contextual Embedding

## 定义

上下文嵌入是一种改进 RAG 检索效果的 embedding 技术，其核心思想是：在将文档分块并进行向量嵌入之前，先使用 LLM 为每个分块生成简短的上下文说明，然后将上下文说明与原始分块文本合并后一起进行嵌入。

## 解决的核心问题

传统 RAG 在文档分块后独立对每个块进行嵌入，导致嵌入向量丢失了重要的上下文信息。例如，一个代码文件中的函数块，如果不知道它属于哪个文件、用于什么目的，嵌入向量就无法准确表达其语义含义。

**问题示例**：
- 分块前的原始文档包含章节标题、文件路径、函数关系等上下文
- 分块后，每个文本块独立进行嵌入，上下文信息丢失
- 检索时，查询可能与分块内容匹配，但该分块的上下文不清晰

**上下文嵌入的解决方案**：
1. 对每个分块使用 LLM 生成简短上下文说明
2. 将上下文说明与分块文本合并
3. 合并后的文本一起进行嵌入
4. 检索时，嵌入向量包含完整的上下文语义

## 实现方式

```python
# 示例伪代码
def contextual_embedding(chunk, document_context):
    context_prompt = f"文档标题: {document_context.title}\n文档摘要: {document_context.summary}\n分块内容: {chunk}"
    context = llm.generate(context_prompt)  # 生成上下文说明
    combined_text = f"{context}\n\n{chunk}"  # 合并上下文与原始文本
    embedding = embedding_model.encode(combined_text)
    return embedding
```

## 关键技术点

### 上下文说明的内容

上下文说明通常包含：
- 文档标题和章节信息
- 分块在文档中的位置
- 与其他分块的关系
- 领域特定的解释说明

### 嵌入模型的选择

Anthropic 测试发现：
- **Voyage** 和 **Gemini** 嵌入模型在此场景下表现最佳
- 上下文嵌入对所有测试的嵌入模型都有提升，但某些模型受益更多

## 与传统嵌入的对比

| 方面 | 传统嵌入 | 上下文嵌入 |
|------|---------|-----------|
| 输入 | 仅分块文本 | 上下文 + 分块文本 |
| 语义完整性 | 丢失上下文 | 保留完整上下文 |
| 检索准确率 | 基准 | 提升 49%（Top-20） |
| 预处理成本 | 低 | 需额外 LLM 调用 |

## 相关概念

- [[contextual-retrieval (待创建)]] — 上下文检索的整体方案
- [[rag-retrieval-augmented-generation]] — 上下文嵌入的应用场景
- [[bm25]] — 另一种与上下文嵌入配合使用的检索技术
- [[embedding]] — 向量化的基础技术
- [[hyde-hypothetical-document-embedding]] — 另一种检索优化策略

## 参考

Anthropic 官方博客：https://www.anthropic.com/engineering/contextual-retrieval