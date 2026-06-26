# Changelog

All notable user-facing changes to **paper-research-skill** are documented here.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
Version scheme: `MAJOR.MINOR.PATCH` — see [CLAUDE.md](./CLAUDE.md) for rules.

---

## [1.4.0] — 2026-06-26

### Added
- `paper-wiki`: Synthesis page type — cross-paper landscape comparison, findings, contradictions, and open questions
- `paper-wiki`: `templates/synthesis.md` — full template with callouts, mermaid diagrams, coverage matrix, evidence heatmap
- `paper-wiki`: Step 7 Synthesis Check — auto-suggest synthesis when vault has 10+ papers; auto-update existing synthesis on new paper additions
- `paper-wiki`: Analysis and Framework paper types with dedicated templates (`templates/analysis.md`, `templates/framework.md`)
- `paper-wiki`: Visual Design System — callout types, emoji section headers, tag taxonomy, reading status tracking
- Demo screenshots: 14 Obsidian screenshots (method + benchmark examples) with cropped PersonaLens views
- `README.zh-CN.md`: Chinese README with language switcher

### Changed
- `paper-wiki`: Extracted 8 inline templates into on-demand `templates/` directory (~60% token savings per invocation)
- `paper-wiki`: Method template redesigned — flat structure with Quick Reference table, promoted sections, consistent callouts
- `CLAUDE.md`: Strengthened versioning rules with pre-commit checklist, anti-pattern examples, and commit message format
- `README.md`: Redesigned with visual pipeline diagram, paper type table, and detailed MCP setup guide

### Removed
- `paper-moc`: Consolidated into `paper-wiki` (MOC is now Step 6)
- `paper-dataview`: Consolidated into `paper-wiki` (frontmatter is now part of templates)
- `paper-note`: Fully replaced by `paper-wiki`

---

## [1.0.5] — 2026-06-10

### Added
- `paper-research`: Document/batch quick mode — parse arXiv IDs/DOIs directly from `.docx` or spreadsheet, skip Stage 1 search
- `paper-research`: Language prompt in Stage 3 (`en` / `zh` / `zh-en`), default English
- `paper-research`: PDF parallel download rule — require absolute paths when using `curl &` in parallel
- `paper-research`: Batch status report table printed after Stage 3 completes
- `paper-research`: Stage 2→3 `{arxiv_id → zotero_key}` mapping handoff documented
- `paper-zotero`: Deduplication pre-check — `search_library` before each import to skip papers already in library
- `paper-wiki`: `lang` parameter (`en`/`zh`/`zh-en`, default `en`) for output language
- `paper-wiki`: PDF prerequisite check — skip wiki entry (with log) if no PDF or Zotero fulltext available

---

## [1.0.3] — 2026-05-16

### Added
- `CLAUDE.md`: versioning rules, changelog policy, commit conventions
- `CHANGELOG.md`: this file
- `paper-version` skill: `/version` command — reports installed version and links to changelog

---

## [1.0.2] — 2026-05-16

### Added
- `paper-wiki`: `## Formatting Rules` section — GitHub MathJax LaTeX syntax guide with Unicode → LaTeX conversion table

### Changed
- `paper-wiki`: Paper Page Template equation example converted to LaTeX (`$$h_t = \tanh(...)$$`, inline `$h_t$`, `$W_{hh}$`, etc.)
- `paper-wiki`: Concept Entry Template equation example converted to LaTeX (`$$\text{Attention}(Q,K,V) = \text{softmax}(...)V$$`, inline `$Q$`, `$\sqrt{d_k}$`)

---

## [1.0.1] — 2026-05-16

### Changed
- `paper-wiki`: Full rewrite to Karpathy-style deep wiki format
  - Paper pages: 7-chapter structure (intro → background → method → key innovation → experiments → critique → limitations)
  - Equations unpacked term-by-term with inline annotations
  - Concept entries: intuition-first, formal definition, mental model, misconceptions, variants, when-to-use
  - 600–1200 words per paper page, 400–800 words per concept entry
- `paper-research`: Updated Stage 3 description to reflect new wiki format
- `paper-zotero`: Replaced pseudocode with real `mcp-zotero` tool names; auto-creates collection named after search topic; added PDF fallback step
- `paper-note`: Updated MCP reference; added metadata-only fallback when PDF unavailable
- `paper-research`: Added explicit PDF fallback step; graceful degradation when MCP unavailable

---

## [1.0.0] — 2026-05-15

### Added
- `paper-research`: Full pipeline orchestrator — search/download → Zotero → Obsidian wiki
- `paper-fetch`: Paper search across arXiv, PubMed, Semantic Scholar, bioRxiv with PDF download
- `paper-zotero`: Zotero import via DOI or local PDF path
- `paper-note`: Obsidian note generation from paper PDF or Zotero item
- `paper-moc`: Topic Map of Content generation
- `paper-wiki`: Karpathy-style deep wiki (initial version)
- `paper-dataview`: Dataview frontmatter injection for Obsidian queries
- `plugin.json` manifest for Claude Code skill registry
