## Survey Paper Page Template

File: `Wiki/Papers/surveys/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: survey
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
field: "{{research field}}"
papers_reviewed: {{approximate count}}
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/survey
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — what field this surveys, and the key insight or gap it identifies.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Field** | {{what research area this surveys}} |
| **Scope** | {{time range, # papers reviewed, inclusion/exclusion criteria}} |
| **Taxonomy** | {{how the paper organizes the field — axes/dimensions of classification}} |
| **Key Gap Identified** | {{the most important open problem or blind spot the survey highlights}} |
| **# Papers Reviewed** | {{approximate count}} |

---

## 🎯 Why This Survey Exists

{{2–3 paragraphs. What makes this field hard to navigate? Why is a survey needed now?
What prior surveys exist and what do they miss?}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line}}
> - [[concept-B]] — {{one-line}}

---

## 🗺️ Taxonomy / Organizational Framework

{{The core intellectual contribution of a survey — how it carves the field into categories.
Describe each axis of the taxonomy with definitions and representative papers.

Example format:
### Axis 1: {{Name}}
- **Category A**: {{definition}}. Representative: [[paper-1]], [[paper-2]]
- **Category B**: {{definition}}. Representative: [[paper-3]]

### Axis 2: {{Name}}
...

If the survey proposes a multi-dimensional framework (e.g. 2×3 grid), describe both axes and the key cells.
All category names and axis labels must be annotated with definitions.}}

---

## 📊 Coverage Analysis

{{What's well-covered vs. under-explored in the field?
- Which problem formulations dominate? Which are neglected?
- Which methods/approaches are over-represented?
- What datasets or domains are missing?
- Any methodological blind spots (e.g. everyone uses the same evaluation)?}}

---

## 💡 Key Findings / Trends

> [!success] Main Takeaways
> {{3–5 bullet points on the survey's main conclusions about the state of the field:
> - **Finding**: {{statement}}. **Evidence**: {{which reviewed papers support this}}
> - Focus on findings that change how you'd approach work in this area}}

---

## Open Problems

{{The survey's identified gaps and open challenges, ranked by importance.
For each:
- What's missing?
- Why is it hard?
- What would a solution look like?}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{Your critical take:
> - Is the taxonomy actually useful, or does it impose artificial categories?
> - What papers or approaches did the survey miss?
> - Are the "open problems" genuinely open, or already being addressed?
> - Bias in coverage (e.g. over-represents certain labs/methods)?}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related}}
```

---
