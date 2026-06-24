---
name: paper-research
description: Use when user wants to run the full academic paper research pipeline from finding papers through to structured Obsidian output. Use when user says "research papers on X", "help me collect literature on Y", "batch process these papers", or "set up my paper workflow".
---

# Paper Research Pipeline

## Overview

Orchestrates the full academic paper workflow: search/download → Zotero import → Obsidian output. Guides the user through each stage and calls the appropriate sub-skills. Each sub-skill can also be used standalone.

## Required Setup

Install all dependencies before running:

**MCP tools** (configure in Claude Code MCP settings):
```bash
# paper-search-mcp
npx -y @smithery/cli install @openags/paper-search-mcp --client claude

# mcp-zotero (supports DOI import + Unpaywall open access)
# See: https://github.com/Xevos117/mcp-zotero

# mcp-obsidian
# See: https://github.com/MarkusPfundstein/mcp-obsidian
```

**Skill** (for wiki mode only):
```bash
npx skills add AgriciDaniel/claude-obsidian
```

## Sub-skills

| Sub-skill | Purpose | Standalone? |
|-----------|---------|-------------|
| `paper-fetch` | Search + select + download papers | ✓ |
| `paper-zotero` | Import to Zotero library | ✓ |
| `paper-note` | Structured Obsidian note per paper | ✓ |
| `paper-moc` | Topic MOC index page | ✓ |
| `paper-wiki` | Karpathy cumulative wiki | ✓ |
| `paper-dataview` | YAML frontmatter for Dataview queries | ✓ |

## Pipeline Flow

**Stage 1 — Fetch**
Call `paper-fetch`. Present candidate list. Wait for user selection.

**Stage 2 — Import**
Call `paper-zotero` with selected papers. Confirm import to Zotero.

**Stage 3 — Obsidian Output**
Ask the user: "Which Obsidian output would you like? (choose one or more)"

- **A) Single notes** — one structured note per paper (`paper-note`)
- **B) Topic index** — MOC page aggregating papers (`paper-moc`)
- **C) Wiki** — Karpathy cumulative wiki entries (`paper-wiki`)
- **D) Dataview metadata** — YAML frontmatter for queries (`paper-dataview`)
- **E) Skip Obsidian** — Zotero only

`paper-dataview` runs automatically alongside A, B, or C unless user opts out.

## Quick Mode

If user provides a DOI list directly, skip the search step and go straight to Stage 2.

## Example Interactions

> "Research the latest papers on RAG for code generation"
→ Keyword search → present list → user selects → import to Zotero → ask Obsidian preference

> "Import these DOIs to Zotero and create wiki entries"
→ Skip fetch → import to Zotero → run paper-wiki

> "Summarize this PDF and add it to my Obsidian"
→ Skip fetch + Zotero → run paper-note directly
