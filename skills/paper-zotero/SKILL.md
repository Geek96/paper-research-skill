---
name: paper-zotero
description: Use when user wants to import academic papers into their Zotero library, whether from a list of PDFs, DOIs, or paper-fetch output. Also triggered when paper-research calls this sub-skill after fetching papers.
---

# Paper Zotero

## Overview

Imports selected papers into Zotero via `mcp-zotero`. Supports DOI-based import (preferred — auto-fetches full metadata + OA PDF via Unpaywall) and PDF import with fulltext indexing. Auto-creates a named collection for the batch.

## Required MCP

`mcp-zotero` (Xevos117) must be installed and configured with `ZOTERO_API_KEY` and `ZOTERO_USER_ID`.

## Actual MCP Tool Names

Use these exact tool names when calling mcp-zotero:

| Action | Tool |
|--------|------|
| List collections | `get_collections` |
| Create collection | `create_collection` |
| Import by DOI | `add_items_by_doi` |
| Import PDF from path/URL | `import_pdf_to_zotero` |
| Search library | `search_library` |
| Get item details | `get_items_details` |

## Input

Accepts `paper-fetch` output JSON, or ask the user for:
- PDF file paths and/or DOI list
- Target Zotero collection name (default: derive from search topic, e.g. `RAG for Code Generation`)

## Steps

1. **Resolve collection**
   - Call `get_collections` to check if a collection with the target name already exists
   - If not, call `create_collection` with the topic name → save returned `collection_key`

1b. **Deduplication pre-check** (run before any import)
   - For each paper with a DOI or arXiv ID, call `search_library` with the arXiv ID or DOI as the query
   - If a result is returned: skip `add_items_by_doi` for that paper, use the existing `zotero_key` instead, log as "already in library"
   - Only call `add_items_by_doi` for papers not already in the library

2. **Import each paper**
   - If DOI available → call `add_items_by_doi` with `dois` array and `collection_key`
     - This auto-attaches OA PDF via Unpaywall when a direct PDF link is found
   - If only local PDF path available → call `import_pdf_to_zotero` with the file path

3. **PDF fallback** (run after step 2)
   - For each paper where `pdf_attached: false` in the `add_items_by_doi` response:
     - If `paper-fetch` downloaded a local PDF for this paper, call `import_pdf_to_zotero` with that path
     - Otherwise log it as "PDF unavailable" — do not block the workflow

4. **Report summary**
   - "Imported N papers to Zotero collection '{{topic}}'"
   - List papers with PDF status: ✅ PDF attached / ⚠️ metadata only

## Output

```json
{
  "collection_key": "NUZMGKE7",
  "collection_name": "RAG for Code Generation",
  "imported": [
    {
      "title": "Attention Is All You Need",
      "doi": "10.48550/arXiv.1706.03762",
      "zotero_key": "ABC123DE",
      "pdf_attached": true
    }
  ],
  "failed": []
}
```

Pass `zotero_key` values to downstream Obsidian sub-skills so notes can link back to Zotero.

## Common Issues

- **Duplicate detection**: Always run the deduplication pre-check in step 1b before importing. If `add_items_by_doi` still returns a duplicate warning despite the pre-check, skip and log the existing key.
- **Missing metadata**: If DOI import returns incomplete metadata, inform the user which fields are missing rather than silently leaving them blank.
- **PDF quota exceeded**: Zotero free storage is 300 MB. If upload fails with a quota error, log the paper title and continue — do not abort the whole batch.
