---
title: "RAG 检索增强生成：从分块到检索"
created: 2026-04-15
updated: 2026-04-21
type: concept
tags: [rag, retrieval, llm, chunking, vector-search, hybrid-search]
sources: [notion:34367b21-8207-818e-a7de-d00cd93fab00]
---

# RAG 检索增强生成：从分块到检索

## 核心定义

RAG（Retrieval-Augmented Generation）将外部知识检索与 LLM 生成结合：先从知识库检索相关信息，再基于检索结果生成回答，避免 LLM "凭空想象"产生幻觉。

## 为什么需要 RAG

| 对比 | 纯 LLM | RAG |
|------|-------|-----|
| 知识时效性 | 训练数据截止 | 实时检索最新文档 |
| 幻觉 | 容易编造事实 | 基于真实文档，减少幻觉 |
| 领域知识 | 通用模型缺乏专业知识 | 注入企业私有知识库 |
| 可追溯性 | 无法验证来源 | 可引用检索到的文档 |

## 文档分块策略

| 策略 | 原理 | 优缺点 |
|------|------|--------|
| 固定大小 | 按字符/token 数划分 | 简单，但可能打断语义 |
| 递归分块 | 按层级分隔符递归切分 | 平衡效果与成本 |
| 语义分块 | 用 embedding 识别语义边界 | 效果最好，成本最高 |

**参数建议**：chunk_size 300-1000 tokens，overlap 为 10-20%（必须不为 0）

## 检索策略

- **向量检索**：余弦相似度、欧氏距离、点积
- **混合检索**：0.7×向量相似度 + 0.3×BM25，兼顾语义与精确匹配
- **HyDE**：生成假设答案引导检索，适合查询与文档风格差异大的场景
- **Reranker**：Cross-Encoder 二次精排

## 大文档处理

- **分层索引**：子块精确检索 + 父块提供上下文
- **Map-Reduce**：分块独立检索后合并去重
- **流式读取**：避免大文件内存问题

## 评估指标

| 指标 | 说明 |
|------|------|
| Recall@K | Top-K 中包含相关文档的比例 |
| MRR | 第一个相关结果的排名倒数 |
| Faithfulness | 回答与检索文档的一致性 |
| Answer Relevance | 回答与问题的相关度 |

## 相关概念

- [[hyde-hypothetical-document-embedding]] — RAG 检索环节的优化策略
- [[embedding]] — 向量化的基础
- [[memory (待创建)]] — RAG 上下文管理的组成部分
- [[hybrid-search (待创建)]] — 结合向量与关键词检索
