## Framework Paper Page Template

File: `Wiki/Papers/frameworks/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: framework
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
domains: [{{domain1}}, {{domain2}}]
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/framework
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — the core thesis or proposed way of thinking.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Framework Type** | {{conceptual framework / taxonomy / position paper / perspective / design pattern}} |
| **Core Thesis** | {{the one-sentence argument this paper makes}} |
| **Scope** | {{what domain/problem this framework applies to}} |
| **Actionable?** | {{Yes — proposes concrete methodology / Partially — proposes principles / No — purely conceptual}} |
| **Validated?** | {{Yes — with experiments / Partially — with case studies / No — argument only}} |

---

## 🎯 The Problem This Framework Addresses

{{2–3 paragraphs on why the field needs a new way of thinking.
What's the current confusion or fragmentation?
What questions can't be answered with existing frameworks?
Karpathy voice: "Everyone is building X, but nobody has stopped to ask whether X is the right frame."}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line}}
> - [[concept-B]] — {{one-line}}

---

## 🗺️ The Framework

### Core Argument

{{The central thesis in 2–3 paragraphs. What is this paper proposing as the "right way to think about" the problem?}}

### Key Dimensions / Axes

{{Most frameworks propose dimensions along which to organize thinking:}}

#### Dimension 1: {{Name}}

{{Definition, why it matters, what it distinguishes.}}

| Category | Definition | Representative Work |
|----------|-----------|-------------------|
| {{Cat A}} | {{...}} | [[paper-1]], [[paper-2]] |
| {{Cat B}} | {{...}} | [[paper-3]] |

#### Dimension 2: {{Name}}

{{Repeat for each dimension.}}

### Proposed Methodology / Principles

{{If the framework is actionable — what should practitioners do?
Step-by-step or principle-by-principle:}}

1. **{{Principle 1}}**: {{explanation}}
2. **{{Principle 2}}**: {{explanation}}

---

## 📊 Validation (if any)

> [!example] Evidence
> {{How does the paper support its framework?
> - Case studies applying the framework?
> - Experiments comparing approaches along the proposed dimensions?
> - Historical analysis showing the framework explains past successes/failures?
>
> If no validation: state explicitly that this is an argument paper, not an empirical one.}}

---

## 💡 What This Changes

> [!success] Practical Impact
> {{How should this framework change how researchers and practitioners approach the problem?
> Be concrete:
> - "When designing a new X, use dimensions A and B to evaluate your design space"
> - "This framework reveals that most current work ignores dimension C"
> - "The key gap identified is Z — this is where new contributions are most needed"}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{3–5 bullet points:
> - Does the framework impose artificial categories?
> - Are the dimensions truly orthogonal or do they overlap?
> - What important work doesn't fit neatly into this framework?
> - Is the framework descriptive (explains what is) or prescriptive (says what should be)?
> - Will this framework age well or is it tied to current trends?}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related: validates framework / contradicts thesis / covers same domain}}
```

---
