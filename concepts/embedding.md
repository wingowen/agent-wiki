---
title: Embedding
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [retrieval, rag]
sources:
---

## Definition

Embedding（嵌入）是将文本、图像等数据转换为稠密向量的技术，使得语义相似的内容在向量空间中距离相近。

## Key Techniques

### Text Embedding

将文本转换为固定维度的实数向量。

### Contextual Embedding

为每个文本块生成上下文相关的嵌入，解决传统分块后的上下文丢失问题。

### Similarity Search

在向量空间中通过余弦相似度或点积查找最相关内容。

## Related

→ [[rag-retrieval-augmented-generation|RAG]]
→ [[contextual-embedding|Contextual Embedding]]
→ [[bm25|BM25]]
