# agent-wiki

AI Agent 研究知识库 — 基于 Karpathy's LLM Wiki 模式构建的持久化、可交叉引用的 Markdown 知识库。

## 结构

```
agent-wiki/
├── SCHEMA.md           # 规范、结构规则、标签分类
├── index.md            # 内容目录
├── log.md              # 操作日志
├── raw/                # 原始来源 (不可变)
│   ├── articles/       # 网络文章
│   ├── papers/         # 论文
│   ├── transcripts/    # 会议记录/访谈
│   └── assets/         # 图片/图表
├── entities/           # 实体页面 (人物/组织/产品)
├── concepts/           # 概念页面 (主题/技术)
├── comparisons/        # 对比分析
└── queries/            # 查询结果归档
```

## 使用

使用 Obsidian 打开此目录即可获得图形化导航和双向链接支持。

## 规范

详见 [SCHEMA.md](SCHEMA.md)
