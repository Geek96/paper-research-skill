---
title: "Scaled Dot-Product Attention"
aliases: [scaled-dot-product-attention]
type: concept
status: complete
created: 2026-06-24
tags:
  - type/concept
  - domain/foundations
  - status/complete
---

# Scaled Dot-Product Attention

> [!abstract] In One Sentence
> 用 query 和所有 key 的点积（除以 √d_k）做 softmax 加权，对 value 求加权和——本质上是一个可微分的 soft dictionary lookup。

---

## Intuition First

想象你在图书馆找资料。你脑中有一个"查询"（query）：比如"Transformer 的训练方法"。图书馆的每本书有一个"索引标签"（key），还有书的实际内容（value）。

普通的查询是 hard lookup：找到完全匹配的标签，返回那本书。但 scaled dot-product attention 做的是 **soft lookup**：计算你的 query 和所有 key 的"相似度"（dot product），然后用 softmax 变成概率分布，最后对所有 value 做加权平均。结果是一个融合了所有相关信息的 vector。

为什么要 "scaled"？当 d_k（key 的维度）很大时，dot product 的值会很大（方差为 d_k），softmax 输出会趋向 one-hot（梯度接近零）。除以 √d_k 把方差拉回 1，让 softmax 保持有意义的梯度。

## The Formal Version

$$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right) V$$

- **Q** (Query): 查询矩阵，shape [n, d_k]。每一行是一个 position 的查询向量。
- **K** (Key): 键矩阵，shape [m, d_k]。每一行是一个被查询位置的索引向量。
- **V** (Value): 值矩阵，shape [m, d_v]。每一行是一个被查询位置的实际信息。
- $QK^\top$: 所有 query-key pair 的 dot product，shape [n, m]。值越大 = 越"匹配"。
- $\sqrt{d_k}$: scaling factor = $\sqrt{\text{key 的维度}}$。防止 dot product 过大导致 softmax 饱和。
- **softmax**: 把每一行（一个 query 对所有 key 的分数）变成概率分布。

## How to Think About It (Mental Model)

把 attention 想成一个 **soft database query**：

```
SQL:   SELECT value FROM table WHERE key = query
Attn:  output = Σ (similarity(query, key_i) × value_i)
```

区别在于：SQL 是 exact match（返回 0 或 1 条），attention 是 soft match（返回所有 value 的加权混合）。weight 由 query-key 的相似度决定。

## Common Misconceptions

> [!danger] Watch Out
> - **"Attention weight 高 = 模型在'关注'那个位置"**：不完全对。Attention weight 是 learned 的 soft routing，不等于 causal importance。高 weight 可能只是因为 key 和 query 在 embedding space 中碰巧接近。
> - **"Scaling 只是为了数值稳定"**：不只是。没有 scaling，softmax 输出接近 one-hot，模型退化成 hard attention，失去了 soft blending 的好处。
> - **"Dot-product attention 是唯一选择"**：还有 additive attention（Bahdanau 2014），用一个小 FFN 计算 compatibility。理论复杂度相同，但 dot-product 更快因为可以用矩阵乘法。


## Variants and Extensions

- **Multi-head attention**: 并行跑多个 attention head，每个关注不同 subspace → [[multi-head-attention]]
- **Additive attention**: 用 FFN 替代 dot product 计算 compatibility（Bahdanau 2014）
- **Relative position attention**: 在 attention score 中加入 relative position bias（Shaw et al. 2018、RoPE）
- **Flash Attention**: 相同的数学，但通过 tiling 和 kernel fusion 大幅减少 memory 和延迟（Dao et al. 2022）
- **Sparse attention**: 只计算部分 query-key pair 的 attention（Sparse Transformer、BigBird）

## When to Use It (and When Not To)

> [!tip] Practical Guidance
> - **Works well when**: 需要建模序列中任意两个位置之间的关系（NLP、Vision、跨模态）
> - **Breaks down when**: 序列长度 n 非常大（O(n²) memory），比如像素级图像处理、超长文档
> - **Prefer RNN/SSM when**: 需要恒定 memory 的 streaming inference，或 n >> 10k 且 hardware memory 有限


---

## Papers

| Paper | How it's used |
|-------|--------------|
| [[attention-is-all-you-need]] | Introduced as the core operation of the Transformer |
| [[vit-image-worth-16x16]] | 直接复用，在 image patches 上做 self-attention |
| [[llava-visual-instruction-tuning]] | 贯穿 CLIP ViT（视觉编码）和 Vicuna LLM（语言解码）的核心运算 |
