---
name: paper-note
description: Use when user wants a structured summary note in Obsidian for one or more academic papers. Use when user says "summarize this paper", "create a note for this paper in Obsidian", or when paper-research calls this sub-skill after importing to Zotero.
---

# Paper Note

## Overview

Reads each paper PDF, extracts structured content, and writes a formatted note to the Obsidian vault via `mcp-obsidian`. Each paper gets its own note file.

## Required MCP

`ajtruex/mcp-obsidian-streamable-http` — already installed.

## Input

Accepts `paper-fetch` + `paper-zotero` output, or ask user for:
- PDF file path(s)
- Obsidian vault folder path (default: `Papers/`)
- Whether to include YAML frontmatter (runs `paper-dataview` automatically if yes)

## Steps

1. Read PDF content (use the local path from `paper-fetch` output, or ask user)
2. Extract the following sections (use paper's own section headers as guide):
   - Problem the paper addresses
   - Key method or approach
   - Main results / findings
   - Limitations acknowledged by authors
3. Write note to Obsidian vault using the template below
4. If Zotero key is available, include it as a deep link in the note

## Note Template

File path: `Papers/{{slug-from-title}}.md`

```markdown
---
(YAML frontmatter added by paper-dataview if requested)
---
# {{title}}

> {{one-sentence summary of the paper's contribution}}

**Year:** {{year}} | **Authors:** {{authors}} | **Source:** [{{doi}}](https://doi.org/{{doi}}) | **Zotero:** [Open](zotero://select/items/{{zotero_key}})

## Problem
{{what gap or problem does this paper address}}

## Method
{{key approach, model, dataset, or technique used}}

## Results
{{main quantitative or qualitative findings}}

## Limitations
{{limitations the authors acknowledge}}

## Personal Notes
(empty — for your annotations)
```

## Output

List of created Obsidian note paths:
```
Papers/attention-is-all-you-need.md
Papers/bert-pre-training.md
```

## Common Issues

- **Long PDFs**: Focus extraction on Abstract, Introduction, Conclusion, and any explicit "Limitations" section. Do not attempt to summarize every section of a 40-page paper.
- **Non-English papers**: Extract in original language, add English title at top.
- **No PDF available**: If only metadata is available (no PDF), write the note using title/abstract/authors from Zotero metadata, and mark the "Method" and "Results" sections as "(PDF not available — manual review needed)".
- **Obsidian MCP unavailable**: Output the note content directly in chat so the user can paste it manually.
