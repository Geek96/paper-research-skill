## MOC (Map of Content) Template

File: `Wiki/MOC.md`

After each batch, create or update the MOC. This is the central navigation page.

```markdown
---
title: "Map of Content"
type: moc
created: {{YYYY-MM-DD}}
updated: {{YYYY-MM-DD}}
tags:
  - type/moc
---

# 🗺️ Research Wiki — Map of Content

> [!abstract] Overview
> This wiki contains **{{N}} papers** and **{{M}} concepts** across {{K}} research domains.
> Last updated: {{YYYY-MM-DD}}

---

## 📊 By Type

### Benchmarks
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|
| [[paper-slug]] | {{domain}} | {{venue}} | {{year}} |

### Methods
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|
| [[paper-slug]] | {{domain}} | {{venue}} | {{year}} |

### Analyses
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|
| [[paper-slug]] | {{domain}} | {{venue}} | {{year}} |

### Frameworks
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|
| [[paper-slug]] | {{domain}} | {{venue}} | {{year}} |

### Surveys
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|

### Technical Reports
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|

---

## 🏷️ By Domain

### {{Domain Name}}
- [[paper-1]]
- [[paper-2]]
- Related concepts: [[concept-a]], [[concept-b]]

---

## 🧩 Concepts Index

| Concept | Category | Introduced By |
|---------|----------|---------------|
| [[concept-slug]] | {{method/evaluation/...}} | [[paper-slug]] |

---

## 📈 Dataview Queries

> [!tip] Auto-Generated Views
> The following Dataview blocks auto-update as you add papers.

### Recent Additions
\`\`\`dataview
TABLE type, venue, year
FROM "Wiki/Papers"
SORT created DESC
LIMIT 10
\`\`\`

### Papers by Domain
\`\`\`dataview
TABLE type, venue, year
FROM "Wiki/Papers"
WHERE contains(tags, "domain/personalization")
SORT year DESC
\`\`\`

### Benchmarks Only
\`\`\`dataview
TABLE domains as "Domain", venue, year
FROM "Wiki/Papers/benchmarks"
SORT year DESC
\`\`\`

### Concepts by Category
\`\`\`dataview
TABLE category, first_introduced as "First Introduced"
FROM "Wiki/Concepts"
SORT title ASC
\`\`\`
```
