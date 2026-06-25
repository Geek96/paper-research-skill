## Technical Report Paper Page Template

File: `Wiki/Papers/technical-reports/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: technical-report
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
model_type: {{foundation model / architecture / system / infrastructure}}
scale: "{{parameter count or key scale metric}}"
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/technical-report
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — what this system/model is and its core design bet.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Type** | {{foundation model / architecture / system / infrastructure}} |
| **Modality** | {{text / vision / multimodal / etc.}} |
| **Scale** | {{parameter count, training data size, compute budget if known}} |
| **Core Design Bet** | {{the one architectural or design decision that defines this work}} |
| **Downstream Impact** | {{what this enabled — list major follow-up works or applications}} |
| **Open Source** | {{Yes/No; link if available}} |

---

## 🎯 Why This Paper Matters

{{2–3 paragraphs. Not "why this paper exists" (it exists because a lab built something) but why it matters:
- What did the field look like before this?
- What shifted after? What became possible?
- Is its importance from the architecture, the scale, the training recipe, or something else?
Karpathy voice: be honest about what's genuinely novel vs. well-executed engineering.}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line}}
> - [[concept-B]] — {{one-line}}

---

## 🏗️ Architecture

{{The core technical content. Walk through the architecture or system design:

### High-Level Design
{{Block diagram description — what are the major components and how do they connect?}}

### Key Components
{{For each non-trivial component:
- What it does (one sentence)
- How it works (mechanistic)
- Key equations with every term annotated
- Why this design choice over alternatives}}

### Training Recipe
{{If applicable:
- Training data: sources, scale, curation
- Training procedure: stages, hyperparameters, curriculum
- Key tricks that matter for reproduction}}

### Design Decisions

{{The most interesting part — WHY choices were made:
- "They chose X over Y because Z"
- "This is unusual because most prior work does W instead"
- Trade-offs: what did this design choice sacrifice?}}

---

## 📊 Scaling & Performance

{{Results framed as "what can this system do":
- Key benchmarks and scores
- Comparison with contemporaries (not just prior work — what shipped around the same time?)
- Scaling laws or emergent capabilities if discussed}}

---

## Legacy & Influence

{{What happened after this paper:
- What follow-up works built directly on this?
- Which design decisions became standard? Which were abandoned?
- Did the paper's own framing hold up, or did the field reinterpret its contribution?
This section is unique to technical reports — benchmark/method papers don't need it because they're too recent.}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{Critical assessment:
> - What limitations are underplayed in the paper?
> - What would break at larger/smaller scale?
> - Which claims haven't been independently verified?
> - What's missing from the evaluation?}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related: predecessor / successor / alternative design}}
```

---
