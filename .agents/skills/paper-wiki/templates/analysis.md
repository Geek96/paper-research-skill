## Analysis Paper Page Template

File: `Wiki/Papers/analyses/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: analysis
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
domains: [{{domain1}}, {{domain2}}]
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/analysis
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — the key finding or insight, not a description of what was studied.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Study Type** | {{empirical analysis / ablation study / negative result / reproducibility study / scaling study}} |
| **Subject** | {{what method(s) or phenomenon is being studied}} |
| **Key Finding** | {{the one-sentence takeaway that changes how you think}} |
| **Methodology** | {{how the study was conducted — controlled experiments / probing / visualization / formal analysis}} |
| **Scope** | {{# models tested, # datasets, # experiments}} |
| **Counterintuitive?** | {{Yes/No — does this contradict conventional wisdom?}} |

---

## 🎯 Why This Study Exists

{{2–3 paragraphs on what assumption or gap motivated this study.
What did the field believe before? What wasn't tested?
Why is this question important enough to dedicate a paper to?
Karpathy voice: what's the "wait, has anyone actually checked this?" moment.}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line}}
> - [[concept-B]] — {{one-line}}

---

## 🔎 Study Design

### Research Questions

{{List the specific questions this paper asks. Number them — the results section should answer each one.}}

1. {{RQ1: ...}}
2. {{RQ2: ...}}
3. {{RQ3: ...}}

### Experimental Setup

{{How the study was conducted:
- What models/systems were compared?
- What datasets/tasks were used?
- What variables were controlled?
- What metrics were measured?

Be specific enough that someone could replicate the study.}}

### Controls and Baselines

{{What was held constant? What was the null hypothesis?
Karpathy voice: good experiments have clear controls. Flag if controls are weak.}}

---

## 📊 Key Findings

{{For each research question, present the finding with evidence:}}

### Finding 1: {{Short statement}}

> [!success] Evidence
> {{Specific numbers, tables, or comparisons that support this finding.
> Include effect sizes, not just p-values.}}

{{Interpretation: why does this happen? What mechanism explains it?}}

### Finding 2: {{Short statement}}

> [!success] Evidence
> {{...}}

{{Repeat for each major finding.}}

---

## 💡 The Core Insight

> [!success] What Changes After Reading This
> {{1–2 paragraphs on the single most important takeaway.
> How should this change how the field approaches the problem?
> What should practitioners do differently?
> Be concrete: "Before this paper, you would do X. After, you should do Y instead."}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{3–5 bullet points:
> - Are the findings robust across different settings?
> - Could confounding variables explain the results?
> - Does the study generalize beyond the specific models/datasets tested?
> - What follow-up experiments would strengthen or weaken the claims?
> - Any methodological concerns?}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related: studies same phenomenon / contradicts findings / extends analysis}}
```

---
