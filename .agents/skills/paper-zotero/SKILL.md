---
name: paper-zotero
description: Use when user wants to import academic papers into their Zotero library, whether from a list of PDFs, DOIs, or paper-fetch output. Also triggered when paper-research calls this sub-skill after fetching papers.
---

# Paper Zotero

## Overview

Imports selected papers into Zotero via `mcp-zotero`. Supports DOI-based import (preferred — Zotero auto-fetches full metadata) and PDF import with fulltext indexing. Assigns papers to a specified collection.

## Required MCP

`Xevos117/mcp-zotero` must be installed and configured.
See setup guide: https://github.com/Xevos117/mcp-zotero

## Input

Accepts `paper-fetch` output JSON, or ask the user for:
- PDF file paths and/or DOI list
- Target Zotero collection name (default: `Inbox`)

## Steps

1. For each paper in input:
   a. If DOI available → call `zotero_add_by_doi(doi)` (Zotero fetches full metadata automatically)
   b. If only PDF available → call `zotero_import_pdf(pdf_path)` with fulltext indexing
   c. Zotero auto-attaches open-access PDF via Unpaywall if available
2. Assign each imported item to the target collection
3. Collect returned Zotero item keys
4. Report summary: "Imported N papers to collection 'Inbox'"

## Output

```json
{
  "imported": [
    {
      "title": "Attention Is All You Need",
      "doi": "10.48550/arXiv.1706.03762",
      "zotero_key": "ABC123DE",
      "collection": "Inbox"
    }
  ],
  "failed": []
}
```

Pass `zotero_key` values to downstream Obsidian sub-skills so notes can link back to Zotero.

## Common Issues

- **Duplicate detection**: Zotero warns on duplicates. Skip and log rather than creating duplicates.
- **Missing metadata**: If DOI import returns incomplete metadata, inform the user which fields are missing rather than silently leaving them blank.
