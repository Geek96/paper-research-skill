## Benchmark Paper Page Template

File: `Wiki/Papers/benchmarks/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: benchmark
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
domains: [{{domain1}}, {{domain2}}]
modality: {{text-only / multimodal}}
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/benchmark
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — what this benchmark measures and the headline finding.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue — e.g. "NeurIPS 2025", "ACL 2025 Findings", "Workshop @ ICLR 2026 (Lifelong Agent)", or "Preprint"}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Domain(s)** | {{e.g. recommendation, memory, education — list all}} |
| **Modality** | {{text-only / multimodal (text+image+...) — specify all modalities involved}} |
| **# Tasks** | {{number and brief names}} |
| **Scale** | {{dataset size: # users, # items, # queries, # interactions, etc.}} |
| **Data Source** | {{synthetic / real / hybrid; platform or method of collection}} |
| **Key Metrics** | {{primary evaluation metrics used}} |
| **# Models Tested** | {{how many models evaluated in the paper}} |
| **Top Model** | {{best-performing model and its headline score}} |
| **Open Data** | {{Yes/No + link if available}} |
| **Open Code** | {{Yes/No + link if available}} |

---

## 🎯 Why This Benchmark Exists

{{2–3 paragraphs on the gap this benchmark fills.
What was impossible to measure before? What existing benchmarks miss?
Why does this specific evaluation matter for the field?
Karpathy voice: be direct about what was broken in prior evaluation.}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line explanation of why it's needed here}}
> - [[concept-B]] — {{one-line explanation}}

---

## 🔬 Task Definitions

{{For each task/subtask the benchmark defines:}}

### Task 1: {{Name}}

- **Input**: {{what the model receives}}
- **Output**: {{what the model must produce}}
- **Evaluation**: {{how correctness is judged — metric + method (automatic/human/LLM-judge)}}

> [!example] Example
> {{one concrete instance to make the task tangible}}

{{Repeat for each task. If there are many subtasks, group logically.}}

---

## 📦 Dataset Construction

### Data Composition

{{Describe the concrete makeup of the dataset:
- What does each data instance look like? (a user profile + query + ground truth? a conversation thread? a sequence of interactions?)
- What fields/columns/attributes does each instance contain?
- If multimodal: what modalities are present per instance, and how do they interact?
- If text-only: what is the text structure?
- Distribution: how balanced across domains/tasks/difficulty levels? Any notable skew?}}

### Curation Pipeline

{{Step-by-step how the data was created. Be specific:
1. **Raw source**: where did the raw data come from?
2. **Filtering**: what was removed and why?
3. **Annotation**: who labeled what? What was the annotation schema?
4. **Quality control**: inter-annotator agreement? validation rounds?
5. **Splits**: how are train/val/test divided? Is there data leakage risk?

If synthetic: what LLM generated it? What prompts? What post-filtering was applied?}}

---

## 📐 Evaluation Framework

{{Metrics used and why they were chosen. For each metric:
- Name and formula — **if the paper defines a metric with an explicit formula, always include it** (LaTeX or plain-text math). Unpack every term in the formula so the reader can compute it from scratch. Standard metrics (Accuracy, F1, BLEU, ROUGE) don't need formulas, but any custom or modified metric does.
- **Every variable or symbol that appears in the wiki page must be annotated on first use** — not just formula terms, but also data structure fields (e.g. T_ref, T₊, T₋ in a triplet), scoring rubric levels (what does score=1 vs 4 mean?), and task-specific notation. The reader should never encounter an unexplained symbol.
- What it captures that alternatives miss
- Known failure modes or blind spots

Example:
> MR (Misapplication Rate) = |{p ∈ P_suppress : applied(p)}| / |P_suppress|
> - P_suppress: the set of preferences that should be suppressed in this context
> - applied(p): whether preference p appeared in the generated text (judged by LLM-as-Judge)

If the benchmark uses LLM-as-judge, describe the rubric (what each score level means) and any inter-rater agreement stats.
If the benchmark uses a non-trivial data structure (triplets, tuples, multi-field instances), annotate every field/variable in the Data Composition or Task Definitions section.}}

---

## 📊 Baseline Results

{{Model comparison table — the core deliverable of a benchmark paper.}}

| Model | {{Metric 1}} | {{Metric 2}} | {{Metric 3}} | Notes |
|-------|-------------|-------------|-------------|-------|
| {{Best model}} | **{{score}}** | **{{score}}** | {{score}} | {{brief note}} |
| {{2nd model}} | {{score}} | {{score}} | {{score}} | |
| ... | | | | |

{{After the table: 2–3 paragraphs analyzing the results.
- What patterns emerge? (size matters? reasoning helps? proprietary > open?)
- What's surprising?
- Where is the ceiling effect or floor effect?}}

---

## 💡 Key Findings

> [!success] Main Takeaways
> {{The benchmark's main takeaways about model capabilities. 3–5 bullet points, each with evidence:
> - **Finding**: X. **Evidence**: Y.
> - Focus on findings that generalize beyond this specific benchmark
> - Flag any findings that contradict conventional wisdom}}

---

## 🛠️ Adoption Guide

> [!tip] Getting Started
> - **Data/Code**: {{how to access}}
> - **Compute**: {{cost requirements}}
> - **Quick Pilot**: {{suggested subset for fast experiments}}
> - **Pair With**: {{complementary benchmarks}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{3–5 bullet points on:
> - Validity concerns (does the benchmark actually measure what it claims?)
> - Missing scenarios or model types not tested
> - Potential for gaming or shortcut solutions
> - How this benchmark might age}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related: complementary benchmark / method tested here / dataset overlap}}
```

---
