---
title: "Multi-Head Attention"
aliases: [multi-head-attention]
type: concept
status: complete
created: 2026-06-24
tags:
  - type/concept
  - domain/foundations
  - status/complete
---

# Multi-Head Attention

> [!abstract] In One Sentence
> 把 Q/K/V 分别投影 h 次到低维子空间，并行跑 h 个 attention，再拼接投影回来——让模型同时从多个角度"看"序列。

---

## Intuition First

想象你在看一段文字："The animal didn't cross the street because it was too tired." 理解这句话需要同时关注多种关系：

- **指代关系**: "it" 指的是 "animal" 还是 "street"？
- **因果关系**: "because" 连接了哪两件事？
- **语法结构**: 主语-谓语-宾语是什么？

如果只有一个 attention head，它需要在一次 softmax 中把这些不同类型的关系都 encode 进去——这很难，因为 softmax 输出的是一个概率分布，只能表达一种"混合体"。

Multi-head attention 的解法：**给模型 h 个平行的"视角"**。每个 head 有自己的 W^Q、W^K、W^V 投影矩阵，可以学到关注不同的 pattern。最后把所有 head 的输出拼起来，再做一次线性投影。

## The Formal Version

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) \, W^O$$
$$\text{where} \quad \text{head}_i = \text{Attention}(Q W_i^Q, \; K W_i^K, \; V W_i^V)$$

- $W_i^Q \in \mathbb{R}^{d_{\text{model}} \times d_k}$: 第 i 个 head 的 query 投影矩阵
- $W_i^K \in \mathbb{R}^{d_{\text{model}} \times d_k}$: 第 i 个 head 的 key 投影矩阵
- $W_i^V \in \mathbb{R}^{d_{\text{model}} \times d_v}$: 第 i 个 head 的 value 投影矩阵
- $W^O \in \mathbb{R}^{h \cdot d_v \times d_{\text{model}}}$: 最终的输出投影
- $d_k = d_v = d_{\text{model}} / h$: 每个 head 的维度是总维度除以 head 数

Transformer base: h=8, d_model=512, d_k=d_v=64。总计算量和 single-head full-dimension attention 相同。

## How to Think About It (Mental Model)

Multi-head attention 就像一个**委员会投票**：

你需要对一个 token 做决策（生成 representation）。不是让一个人看所有因素，而是让 8 个专家各自从自己擅长的角度分析，最后综合他们的意见。有人看语法，有人看语义，有人看位置关系。Concat + $W^O$ 就是"综合讨论"步骤。

## Common Misconceptions

> [!danger] Watch Out
> - **"每个 head 一定学到了不同的、可解释的 pattern"**：不一定。虽然 Transformer 论文的 visualization 展示了有的 head 关注距离、有的关注共指，但很多 head 的行为是 redundant 或难以解释的。
> - **"Head 越多越好"**：不对。Transformer 论文 Table 3 显示 h=8 最优，h=32 时性能下降。每个 head 的 d_k 太小会损害 compatibility 的表达能力。
> - **"去掉某些 head 模型就坏了"**：研究（Michel et al. 2019 "Are Sixteen Heads Really Better than One?"）表明大部分 head 可以在 inference 时被 prune 掉，性能损失很小。


## Variants and Extensions

- **Grouped Query Attention (GQA)**: 多个 query head 共享同一组 K/V head，减少 KV cache 大小（Llama 2, Mistral）
- **Multi-Query Attention (MQA)**: 所有 query head 共享一组 K/V（极端版 GQA），大幅减少 memory（PaLM）
- **Cross-attention**: Q 来自一个 sequence，K/V 来自另一个 sequence（encoder-decoder 之间，或 VLM 中视觉 tokens → 语言模型）

## When to Use It (and When Not To)

> [!tip] Practical Guidance
> - **Works well when**: 几乎所有 Transformer-based 模型都用 multi-head attention，是标准组件
> - **用 GQA/MQA when**: 需要长 context 且 KV cache 成为 bottleneck（大型 LLM inference）
> - **考虑减少 heads when**: 参数/计算预算有限且不需要太多并行 attention pattern


---

## Papers

| Paper | How it's used |
|-------|--------------|
| [[attention-is-all-you-need]] | Introduced. h=8 heads, d_k=64, 是 Transformer 的核心组件 |
| [[vit-image-worth-16x16]] | 直接复用，ViT-Base 用 12 heads，ViT-Large 用 16 heads |
