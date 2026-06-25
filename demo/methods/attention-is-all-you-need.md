---
title: "Attention Is All You Need"
aliases: [attention-is-all-you-need]
type: method
year: 2017
venue: "NeurIPS 2017"
arxiv: "1706.03762"
domains: [foundations]
status: complete
created: 2026-06-24
tags:
  - type/method
  - domain/foundations
  - venue/neurips-2017
  - status/complete
---

# Attention Is All You Need

> [!abstract] TL;DR
> 提出了 Transformer 架构——完全基于 attention mechanism，抛弃了 RNN 和 CNN，在机器翻译上达到 SOTA 且训练速度大幅提升。

**Authors**: Vaswani, Shazeer, Parmar, Uszkoreit, Jones, Gomez, Kaiser, Polosukhin | **Year**: 2017 | **arXiv**: [1706.03762](https://arxiv.org/abs/1706.03762) | **Zotero**: [Open](zotero://select/items/HWC2I7ZT)
**Venue**: NeurIPS 2017

---

## 🎯 Why This Paper Exists

2017 年之前，序列建模（sequence transduction）的主力是 RNN 及其变体（LSTM、GRU）。它们有一个根本性问题：**时间步必须串行计算**。position t 的 hidden state 依赖 position t-1，无法并行化。当序列变长，训练速度和显存都成为瓶颈。

Attention mechanism 早在 Bahdanau (2014) 就被提出，但一直是 RNN 的"附件"——加在 encoder-decoder 之间帮助对齐。没人试过完全用 attention 替代 recurrence。

这篇论文的核心赌注是：**attention alone 就够了**。不需要 recurrence，不需要 convolution。结果不仅翻译质量更好（EN-DE 28.4 BLEU，超过所有 ensemble），而且训练时间从几周缩短到 3.5 天（8 块 P100）。这个架构后来成为 GPT、BERT、ViT 等几乎所有现代 AI 模型的基础。

## Prerequisites

> [!info] Prerequisites
> - [[scaled-dot-product-attention]] — Transformer 的最基本运算单元
> - [[encoder-decoder-architecture]] — 理解 seq2seq 框架，知道为什么需要 encoder 和 decoder
> - [[positional-encoding]] — Transformer 没有 recurrence，位置信息怎么注入

---

## 🔬 The Method

### Introduction / Motivation

论文开门见山：RNN 的串行性是 fundamental constraint，不是 engineering problem。之前的工作（factorization tricks、conditional computation）只是在绕开这个问题，没有从根本上解决。

核心主张：用一个完全基于 attention 的架构（the Transformer），可以做到 (1) 任意两个位置之间的依赖路径长度为 O(1)，(2) 完全可并行化，(3) 翻译质量更好。

### Background / Prior Work

三个前驱方向：
- **Extended Neural GPU / ByteNet / ConvS2S**：用 CNN 替代 RNN 实现并行，但远距离依赖需要 O(n/k) 或 O(log_k(n)) 层堆叠
- **Self-attention**：已有人在阅读理解、文本蕴含等任务上用过，但都是和 RNN 配合使用
- **End-to-end memory networks**：用 recurrent attention 替代 sequence-aligned recurrence

Transformer 是第一个**完全只用 self-attention** 做 transduction 的模型。

### The Method — How It Actually Works

#### 1 整体架构：Encoder-Decoder Stacks

Encoder 由 N=6 层相同的 layer 堆叠。每层有两个 sub-layer：
1. Multi-head self-attention
2. Position-wise feed-forward network (FFN)

每个 sub-layer 都有 residual connection + layer normalization：$\text{LayerNorm}(x + \text{Sublayer}(x))$

Decoder 也是 N=6 层，但多了一个 sub-layer：encoder-decoder attention（cross-attention）。Decoder 的 self-attention 还加了 mask，确保 position i 只能看到 < i 的位置（保持 auto-regressive 性质）。

#### 2 Scaled Dot-Product Attention

核心公式：

$$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right) V$$

- **Q** (Query): 你想查询的内容，shape [seq_len, d_k]
- **K** (Key): 被查询的索引，shape [seq_len, d_k]
- **V** (Value): 实际的内容/信息，shape [seq_len, d_v]
- **√d_k**: scaling factor——当 d_k 很大时，dot product 的方差为 d_k，softmax 会被推到梯度极小的区域，除以 √d_k 来缓解

为什么用 dot-product 而不用 additive attention？速度。Dot-product 可以直接调用优化过的矩阵乘法。

#### 3 Multi-Head Attention

不做一次 d_model 维的 attention，而是做 h=8 次 d_k=64 维的 attention：

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) \, W^O$$
$$\text{where} \quad \text{head}_i = \text{Attention}(Q W_i^Q, \; K W_i^K, \; V W_i^V)$$

为什么要 multi-head？Single-head 做 averaging 会抑制模型从不同 subspace 获取信息的能力。每个 head 可以学习关注不同的 pattern（比如一个 head 关注语法结构，另一个关注语义相似性）。

总计算量和 single-head full-dimension attention 差不多（因为每个 head 的维度是 d_model/h）。

#### 4 Attention 的三种使用场景

1. **Encoder self-attention**: Q/K/V 都来自上一层 encoder 输出。每个位置可以看到所有其他位置。
2. **Decoder self-attention**: Q/K/V 都来自 decoder，但 mask 掉未来位置。
3. **Encoder-decoder attention (cross-attention)**: Q 来自 decoder，K/V 来自 encoder 输出。这让 decoder 的每个位置都能"查看"整个输入序列。

#### 5 Position-wise FFN

每层的第二个 sub-layer：两层 linear transformation + ReLU：

$$\text{FFN}(x) = \max(0, \; xW_1 + b_1) W_2 + b_2$$

输入输出维度 d_model = 512，中间层 d_ff = 2048。可以理解为 1×1 convolution。

#### 6 Positional Encoding

Transformer 没有 recurrence，token 顺序信息全丢了。解决方案：加 positional encoding。

用不同频率的 sin/cos 函数：
$$PE_{(pos, 2i)} = \sin\!\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)$$
$$PE_{(pos, 2i+1)} = \cos\!\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)$$

为什么用 sinusoidal 而不是 learned？因为 sin/cos 有一个好性质：$PE_{pos+k}$ 可以表示为 $PE_{pos}$ 的线性函数，模型可以轻松学到相对位置关系。而且可以外推到训练时没见过的序列长度。


---

## 💡 The Key Innovation

> [!success] What's Genuinely New
> 真正新的东西其实就一个决定：**把 attention 从配角变成主角**。Scaled dot-product attention 本身不新（multiplicative attention 早有了），multi-head 的 idea 也不算全新。但把 RNN 完全扔掉、only use attention，这在当时是非常大胆的。
>
> 论文的 Table 1 精炼地总结了为什么这个决定合理：self-attention 的 maximum path length 是 O(1)，而 RNN 是 O(n)。这意味着学习 long-range dependency 从理论上变得更容易。

---

## 📊 Experiments

**EN-DE translation**: Transformer (big) 达到 28.4 BLEU，超过之前所有单模型和 ensemble 2+ BLEU。训练 3.5 天 on 8 P100s。
**EN-FR translation**: 41.8 BLEU（single model SOTA），训练成本不到之前 SOTA 的 1/4。
**English constituency parsing**: 在 WSJ 上达到 92.7 F1（semi-supervised），证明 Transformer 不只是翻译专用。

Model variations（Table 3）的关键发现：
- Single-head 比 8-head 差 0.9 BLEU，但 head 太多（32）也会下降
- 减小 d_k 会 hurt 质量（说明 compatibility function 不简单）
- Dropout 很重要（不用 dropout，dev PPL 从 4.92 涨到 5.77）
- Learned positional embedding 和 sinusoidal 效果几乎一样

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> - **Sparse attention**: 论文提到了 restricted self-attention（只看 neighborhood），但没有深入。后来 Sparse Transformer、Longformer 等证明这是个重要方向。
> - **Decoder-only**: 论文用的是 encoder-decoder，但后来 GPT 证明 decoder-only 在 language modeling 上更好。为什么？encoder-decoder 的 cross-attention 有什么 decoder-only 做不到的？
> - **为什么 layer normalization 放在 sub-layer 之前（Pre-LN）会更稳定？** 论文用的是 Post-LN，但后来 Pre-LN 成为标准。
> - **Scaling law**: 论文只试了 base 和 big 两个 size。如果继续 scale up 呢？（答案：GPT-3 证明了可以 scale 到 175B）。
> - **Self-supervised pre-training**: 论文只做了 supervised translation。BERT 和 GPT 后来证明 self-supervised pre-training + fine-tuning 才是 Transformer 的真正杀手应用。
> - **O(n²) complexity**: Self-attention 的计算和内存都是序列长度的平方。对于长序列（比如整本书、高分辨率图像），这是硬伤。
> - **没有 inductive bias**: 不像 CNN 有 locality 和 translation invariance，Transformer 对结构一无所知。小数据集上会 underperform（ViT 论文后来证实了这一点）。
> - **位置编码的局限**: Sinusoidal encoding 是 absolute position，不能很好地处理 relative position（后来 RoPE、ALiBi 等改进了这一点）。
> - **只在 NLP 上验证**: 论文结尾提到想扩展到 image/audio/video，但没有实验。这要等 ViT (2020) 才实现。

---

## 🧩 Key Concepts

- [[scaled-dot-product-attention]] — Transformer 的核心运算：$\text{softmax}(QK^\top/\sqrt{d_k})V$
- [[multi-head-attention]] — 多个 attention head 并行，捕捉不同 subspace 的信息
- [[positional-encoding]] — 用 sin/cos 函数注入位置信息，替代 RNN 的隐式顺序
- [[encoder-decoder-architecture]] — 经典的 seq2seq 框架，Transformer 在此基础上构建
- [[residual-connection]] — 每个 sub-layer 的输出加上输入，缓解深层网络的梯度消失
- [[layer-normalization]] — 在 residual connection 之后做归一化，稳定训练

---

## 🔗 Related Papers

- [[vit-image-worth-16x16]] — 将 Transformer 直接应用于图像，证明 attention is all you need for vision too
