---
title: "上下文检索 Contextual Retrieval"
created: 2026-04-21
updated: 2026-04-21
type: entity
tags: [rag, retrieval, anthropic, embedding, bm25, reranking, context-engineering]
sources: [notion:34267b21-8207-813f-b0e2-cae2d942f74c, https://www.anthropic.com/engineering/contextual-retrieval]
---

# 上下文检索 Contextual Retrieval

## 什么是上下文检索

上下文检索（Contextual Retrieval）是 Anthropic 于 2024 年 9 月发布的一种改进 RAG 检索效果的 technique。它通过为每个文档分块生成上下文说明，解决传统 RAG 在编码信息时丢失上下文的问题。该方法可将检索失败率降低 49%，结合 Reranking 后可降低 67%。

## 核心组件

### 上下文嵌入（Contextual Embedding）

在将文档分块并进行嵌入之前，使用 LLM 为每个分块生成简短的上下文说明，然后将该上下文附加到分块文本中再进行嵌入。这样检索时，嵌入向量包含了完整的上下文信息，大幅提升检索准确率。

**实现方式**：
```
原分块文本 + "上下文说明（由 LLM 生成）" → 一起进行嵌入
```

### 上下文 BM25（Contextual BM25）

传统 BM25 是基于词频的检索算法，同样会因分块丢失上下文信息。上下文 BM25 的做法与上下文嵌入类似：在建立 BM25 索引前，先为每个分块添加上下文说明，从而提升关键词匹配的准确性。

### Reranking（重排序）

在初始检索返回候选文本块后，使用专门的排序模型（如 Cohere reranker）对它们进行重新评分和排序，确保只有最相关的文本块被传递给生成模型。这提供了更好的回答质量，同时降低了成本和延迟。

**流程**：
1. 执行初始检索（获取排名前 150 的候选块）
2. 将 Top-N 块与用户查询传入 Reranking 模型
3. 模型评分后选择排名前 K 的块（通常 K=20）
4. 将 Top-K 块作为上下文传入模型生成最终结果

## 性能数据

| 方法 | Top-20 检索失败率 |
|------|-----------------|
| 标准嵌入 + BM25 | 5.7% |
| 上下文嵌入 + 上下文 BM25 + Reranking | 1.9% |
| **降低幅度** | **67%** |

## 最佳实践

- **嵌入模型选择**：Voyage 和 Gemini 在测试中表现最佳
- **分块策略**：考虑分块大小、边界和重叠对检索性能的影响
- **自定义上下文**：针对特定领域定制的上下文提示词可获得更好效果
- **文本块数量**：传递 20 个文本块比 10 或 5 个效果更好

## 成本考量

使用 Claude 3 Haiku 生成上下文说明的成本约为 **$1.02/百万文档 token**，结合 Prompt Caching 可进一步降低成本。上下文检索的成本主要在预处理阶段，运行时的增量成本很小。

## 关系

- 发布方：[[anthropic]]
- 相关概念：[[rag-retrieval-augmented-generation]]、[[embedding]]、[[bm25]]
- 相关工具：[[claude-code]]
- 应用领域：知识库检索、文档问答、企业知识管理