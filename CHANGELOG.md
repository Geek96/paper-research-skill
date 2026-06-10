# Changelog

All notable user-facing changes to **paper-research-skill** are documented here.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
Version scheme: `MAJOR.MINOR.PATCH` — see [CLAUDE.md](./CLAUDE.md) for rules.

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
