## Method Paper Page Template

File: `Wiki/Papers/methods/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: method
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
domains: [{{domain1}}, {{domain2}}]
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/method
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — the core claim, not a description of structure.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Core Contribution** | {{one-line: what this paper contributes — e.g. "RL-based preference inference for personalization"}} |
| **Method Type** | {{architecture / training pipeline / inference-time / framework / system}} |
| **Base Model(s)** | {{what models were used — e.g. "Qwen3-8B", "Llama-3-8B", or "model-agnostic"}} |
| **Key Result** | {{the headline number — e.g. "87.5% preference accuracy (+15.7pp over best baseline)"}} |
| **Key Ablation** | {{the most revealing ablation — e.g. "RL vs SFT: 69.1% vs 47.9% (+21pp)"}} |
| **Open Code** | {{Yes/No + link if available}} |

---

## 🎯 Why This Paper Exists

{{2–3 paragraphs on the problem this paper solves. Write in plain language.
What was broken or missing before? What makes this hard?
What would happen if you didn't have this paper's contribution?
Karpathy voice: be direct, use concrete examples, avoid hedging.}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line explanation of why it's needed here}}
> - [[concept-B]] — {{one-line explanation}}
> - [[concept-C]] — {{one-line explanation}}
>
> If these are unfamiliar, read the concept entries first.

---

## 🔬 The Method

### Background / Prior Work

{{What are the 2–3 key papers this builds on? What does this paper need from them?
Don't just list citations — explain the dependency.
Example: "This paper needs [[DTW]] because the core metric is built on top of it."}}

### How It Actually Works

{{This is the longest section. Walk through the method step by step.

For each key component:
- What does it do? (one sentence intuition)
- How does it work? (mechanistic explanation)
- If there's a key equation: write it out, then unpack every term.

Example format:

$$h_t = \tanh(W_{hh} \cdot h_{t-1} + W_{xh} \cdot x_t)$$

- $h_t$: the new hidden state (what the network "knows" at step t)
- $h_{t-1}$: what it knew last step
- $x_t$: the new input
- $W_{hh}$ and $W_{xh}$: learned weight matrices
- $\tanh$: squishes values to $[-1, 1]$ so things don't explode

Repeat for each component. Include ASCII diagrams if they aid clarity.
If there are multiple sub-modules, use ### sub-headers for each.}}

---

## 💡 The Key Innovation

> [!success] What's Genuinely New
> {{In 1–2 paragraphs: what is genuinely new in this paper?
> Be specific and critical. Examples:
> - "The novel part is X. Prior work did Y instead, which caused problem Z."
> - "This looks like technique A from [prior paper], but differs in that..."
> Karpathy style: distinguish between what's actually new and what's packaging.}}

---

## 📊 Experiments

{{What did they test? On what data? Against what baselines?}}

| Setting / Dataset | Metric | This Paper | Best Baseline | Δ | Notes |
|-------------------|--------|------------|---------------|---|-------|
| {{dataset-1}} | {{metric}} | **{{score}}** | {{baseline score}} | {{+X.X}} | {{brief}} |
| {{dataset-2}} | {{metric}} | **{{score}}** | {{baseline score}} | {{+X.X}} | |

{{After the table: 2–3 paragraphs analyzing the results.
- What patterns emerge?
- How big is the improvement? Is it meaningful in practice?
- What ablation is most revealing?
- What did they NOT test that you'd want to see?

Be concrete: "They report a 3.2% improvement on benchmark X. Baseline Y was strong.
They did not test on out-of-distribution data, which would matter most for deployment."}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{3–5 bullet points combining questions AND limitations:
> - What experiment would you run next?
> - What assumption seems shakiest?
> - What does the method fail on? What assumptions does it make?
> - When would you NOT use this approach?
> - What does this suggest for your own work?
>
> Be specific. "I would try X because Y" not "future work should explore X."
> Include the authors' stated limitations + any they missed.}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related: shares method / problem / dataset}}
```

---
