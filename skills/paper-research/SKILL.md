---
name: paper-research
description: Use when user wants to run the full academic paper research pipeline from finding papers through to structured Obsidian output. Use when user says "research papers on X", "help me collect literature on Y", "batch process these papers", or "set up my paper workflow".
---

# Paper Research Pipeline

## Overview

Orchestrates the full academic paper workflow: search/download → Zotero import → Obsidian output. Guides the user through each stage and calls the appropriate sub-skills. Each sub-skill can also be used standalone.

## Installed MCP Tools

These are already configured — do not prompt user to install them:

| MCP | Purpose |
|-----|---------|
| `openags/paper-search-mcp` | Search arXiv, PubMed, Semantic Scholar + download PDFs |
| `mcp-zotero` (Xevos117) | Import papers to Zotero by DOI or PDF |
| `ajtruex/mcp-obsidian-streamable-http` | Read/write Obsidian vault |

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

**Stage 2 — Zotero Import**
Call `paper-zotero` with:
- Selected papers from Stage 1
- Collection name = search topic (e.g. "RAG for Code Generation") — create automatically, do not ask user

After import, run the PDF fallback step inside `paper-zotero`:
for each paper with no PDF attached, try `import_pdf_to_zotero` using the local path from Stage 1.

**Stage 2 → Stage 3 handoff**
After Stage 2 completes, build a mapping table `{arxiv_id → zotero_key}` from the `paper-zotero` output JSON. Pass this table to all Stage 3 sub-skills so every note/wiki page can include a working `zotero://select/items/{{zotero_key}}` link.

**Stage 3 — Obsidian Output**
Ask the user two questions at once:

1. "Which Obsidian output would you like? (choose one or more)"
   - **A) Single notes** — one structured note per paper (`paper-note`)
   - **B) Topic index** — MOC page aggregating papers (`paper-moc`)
   - **C) Wiki** — Karpathy-style deep wiki: chapter-by-chapter paper breakdown + detailed concept entries (`paper-wiki`). Reads the full PDF — not just the abstract. Each paper page 600–1200 words; each concept entry 400–800 words.
   - **D) Dataview metadata** — YAML frontmatter for queries (`paper-dataview`)
   - **E) Skip Obsidian** — Zotero only

2. "Language? (A) English  (B) 中文  (C) 中英混合"
   Default: **English**.

`paper-dataview` runs automatically alongside A, B, or C unless user opts out.

## Quick Mode

- **DOI list provided** → skip Stage 1, go straight to Stage 2
- **Document/batch provided** (`.docx`, spreadsheet, or pre-selected list with abstracts) → parse all arXiv IDs/DOIs from the document, skip Stage 1, go straight to Stage 2. If extra arXiv URLs are provided alongside the document, include them in the same batch.
- **PDF only, no DOI** → skip Stage 1 + Stage 2 Zotero metadata step, use `import_pdf_to_zotero` directly, then go to Stage 3
- **Summarize only** → skip Stage 1 + Stage 2, run `paper-note` directly on given PDF

## Example Interactions

> "Research the latest papers on RAG for code generation"
→ Keyword search → present list → user selects → import to Zotero collection "RAG for Code Generation" → ask Obsidian preference

> "Import these DOIs to Zotero and create wiki entries"
→ Skip fetch → import to Zotero → run paper-wiki

> "Summarize this PDF and add it to my Obsidian"
→ Skip fetch + Zotero → run paper-note directly

## PDF Parallel Download Rule

When downloading multiple PDFs in parallel with `curl`, **always use absolute paths**. Never rely on `cd` before `&` — the working directory does not carry over to backgrounded processes.

```bash
# ✅ CORRECT
RAW="/absolute/path/to/raw"
curl -L -o "$RAW/paper1.pdf" "url1" &
curl -L -o "$RAW/paper2.pdf" "url2" &
wait

# ❌ WRONG — cd only applies to the first command, backgrounded curls run from $HOME
cd /path/to/raw && curl -L -o paper1.pdf url1 & curl -L -o paper2.pdf url2 &
```

## Batch Status Report

After completing Stage 3, always print a summary table:

```
| Paper | Zotero | PDF | Wiki/Note |
|-------|--------|-----|-----------|
| MemLens | ✅ J68R3RD3 | ✅ | ✅ |
| FileGram | ✅ ARVIZ55V | ❌ download failed | ⚠️ abstract only |
```

## Error Handling

- If `paper-search-mcp` is unavailable, fall back to asking the user for a DOI list
- If Zotero import fails for a paper, log it and continue — do not abort the batch
- If Obsidian MCP is unavailable, output the note content inline in the chat instead
