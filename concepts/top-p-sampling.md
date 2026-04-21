---
title: Top-p Sampling
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [prompt-engineering, tool-use]
sources:
  - https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues
  - https://en.wikipedia.org/wiki/Nucleus_sampling
---

# Top-p Sampling

Top-p Sampling（又称 Nucleus Sampling）是一种文本生成策略，用于控制 LLM 输出的多样性和质量。该策略由 Holtzman 等人在 2019 年提出，是现代大语言模型生成文本的核心技术之一。

## 定义

Top-p Sampling 从累积概率达到阈值 p 的最小词集中进行采样。例如，当 p=0.9 时，模型只考虑累积概率加起来达到 90% 的最可能 token，然后从中随机采样。

## 工作原理

1. 模型计算所有可能下一个 token 的概率分布
2. 将 token 按概率从高到低排序
3. 累加概率，当达到阈值 p 时停止
4. 从这个候选集中随机采样一个 token

## 相关参数

### Temperature

Temperature（温度值）是控制模型输出随机性的参数：

- **Temperature = 0**：模型总是选择概率最高的 token（确定性输出）
- **Temperature > 0**：值越高，输出越随机
- **Temperature < 1**：值越低，输出越确定

### Top-k

Top-k 是另一种采样策略，限制模型只从概率最高的 k 个 token 中选择。Top-p 通常比 Top-k 更灵活，因为它能自适应地调整候选集大小。

## Anthropic 故障案例

在 2025 年 9 月 Anthropic 的故障复盘 中，Top-p Sampling 相关的 Bug 导致 Claude 响应质量下降：

1. **Context Window 路由错误**：短上下文请求被错误路由到配置错误的服务器
2. **输出损坏**：TPU 服务器在 token 生成过程中产生乱码
3. **Approximate Top-k XLA:TPU 编译错误**：混合精度算术问题导致最高概率 token 被丢弃

这些问题导致用户在英文提示中看到泰语或中文字符，或在代码中产生明显的语法错误。

## 技术细节

### 混合精度算术

现代 LLM 使用 bf16（16 位浮点数）计算 token 概率，但向量处理器是 fp32-native 的。XLA 编译器会将某些操作转换为 fp32 以保持精度。

这种混合精度可能导致：
- 精度不匹配
- 不同操作在不同的精度级别运行
- 无法就哪个 token 概率最高达成一致

### Approximate Top-k

Approximate Top-k 是一种性能优化算法，用于快速找到最高概率的 token。但在某些批量大小和模型配置下可能返回完全错误的结果。

## 最佳实践

1. **生产环境使用精确 Top-k**：虽然有轻微性能损失，但更可靠
2. **监控异常输出**：如发现乱码或语法错误，可能是采样问题
3. **隐私与调试平衡**：在保护用户隐私的同时获取足够的调试信息

## 关联概念

- [[prompt-engineering]] - 提示工程的基础技术
- [[agent-architecture-design]] - Agent 输出质量保障
- [[anthropic|Anthropic]] - 该技术的实践者
