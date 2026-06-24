---
title: "Positional Encoding"
aliases: [positional-encoding]
type: concept
status: complete
created: 2026-06-24
tags:
  - type/concept
  - domain/foundations
  - status/complete
---

# Positional Encoding

> [!abstract] In One Sentence
> 给序列中每个位置一个唯一的向量标识，注入到 token embedding 中，让 Transformer 知道"谁在前谁在后"。

---

## Intuition First

Transformer 的 self-attention 是 **permutation-equivariant** 的——打乱输入顺序，输出也跟着打乱，但每个 token 的 representation 不变。换句话说，attention 本身不知道 "cat sat on mat" 和 "mat on sat cat" 有什么区别。

这在 NLP 中是灾难性的（语序改变意义），在 vision 中也有问题（空间关系重要）。

解决方案：在输入 embedding 上加一个 position-dependent 的向量。就像给每个 token 贴一个"座位号"。模型在计算 attention 时，就能通过这个 "座位号" 区分不同位置。

## The Formal Version

Transformer 原论文使用 **sinusoidal positional encoding**:

$$PE_{(pos, 2i)} = \sin\!\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)$$
$$PE_{(pos, 2i+1)} = \cos\!\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)$$

- **pos**: token 在序列中的位置（0, 1, 2, ...）
- **i**: embedding 维度的 index（0, 1, ..., d_model/2 - 1）
- 偶数维用 sin，奇数维用 cos
- 不同 i 对应不同频率的正弦波，形成从 2π 到 10000·2π 的几何级数

最终：`input = token_embedding + positional_encoding`

为什么用 sin/cos？因为 $PE_{pos+k}$ 可以表示为 $PE_{pos}$ 的线性变换（旋转矩阵），模型可以轻松学到 relative position。

## How to Think About It (Mental Model)

想象一根**无限长的标尺**。每个刻度（position）有一个唯一的"指纹"——由不同频率的波叠加而成。低频波区分远距离位置（"你在文章开头还是结尾"），高频波区分近距离位置（"你在这个词还是下一个词"）。

模型拿到这个"指纹"后，可以通过计算两个位置指纹的关系来推断它们的相对距离。

## Common Misconceptions

> [!danger] Watch Out
> - **"Sinusoidal encoding 是唯一选择"**：不是。ViT 用 learned position embedding（效果差不多）。RoPE 把位置信息编码进 attention score 而非 input embedding。ALiBi 直接在 attention logits 上加 position-dependent bias。
> - **"Positional encoding 固定了最大序列长度"**：sinusoidal 理论上可以外推到任意长度，但实际效果取决于模型是否在训练时见过足够长的序列。Learned embedding 确实固定了最大长度。
> - **"Position encoding 只需要加一次"**：标准做法是在第一层加。但有的架构（如 DeBERTa）在每层都重新注入位置信息。


## Variants and Extensions

- **Learned positional embedding**: 一个可训练的 embedding table（BERT、GPT-2、ViT 都用这个）
- **Rotary Position Embedding (RoPE)**: 把位置信息编码为 query/key 向量的旋转，天然编码 relative position（LLaMA、Qwen）
- **ALiBi**: 不加 embedding，直接在 attention score 上减去一个和距离成正比的 penalty（BLOOM）
- **2D positional encoding**: 把 row 和 column position 分别编码（适合图像 patches）
- **Relative position bias**: 在 attention logits 上加一个 learnable bias table indexed by relative position（Swin Transformer、T5）

## When to Use It (and When Not To)

> [!tip] Practical Guidance
> - **Sinusoidal when**: 需要外推到更长序列，或不想增加参数
> - **Learned when**: 序列长度固定，模型够大不在乎多几个参数
> - **RoPE when**: 需要 relative position、需要 KV cache 高效（现代 LLM 标配）


---

## Papers

| Paper | How it's used |
|-------|--------------|
| [[attention-is-all-you-need]] | Introduced sinusoidal positional encoding，加在 input embedding 上 |
| [[vit-image-worth-16x16]] | 用 learned 1D positional embedding（与 sinusoidal 效果相当），发现模型自动学到 2D 拓扑结构 |
