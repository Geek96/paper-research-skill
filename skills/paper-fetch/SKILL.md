---
name: paper-fetch
description: Use when user wants to search for academic papers by keywords or topic, or download specific papers from a DOI/URL list. Triggers when user says "find papers on X", "search for research about Y", "get these papers" with a DOI list, or when paper-research calls this sub-skill.
---

# Paper Fetch

## Overview

Searches multiple academic databases and downloads user-selected papers as PDFs.
Uses `paper-search-mcp` for multi-source concurrent search with deduplication and open-access fallback.

## Required MCP

`openags/paper-search-mcp` must be installed and configured.

Install:
```bash
npx -y @smithery/cli install @openags/paper-search-mcp --client claude
```

## Input

**Option A — Keyword Search**

Ask the user for:
- Search terms (required)
- Sources to search: `arxiv`, `pubmed`, `semantic_scholar`, `biorxiv` (default: all)
- Date range, e.g. `2022-2025` (optional)
- Max results, e.g. `10` (default: 10)

**Option B — DOI / URL List**

Accept a plain list, one per line:
```
10.48550/arXiv.1706.03762
10.1038/s41586-021-03819-2
https://arxiv.org/abs/2301.00001
```

## Steps

1. Call `search_papers` (Option A) or skip directly to step 3 (Option B)
2. Present candidate table to user:

   | # | Title | Authors | Year | Source |
   |---|-------|---------|------|--------|
   | 1 | Attention Is All You Need | Vaswani et al. | 2017 | arxiv |

   Include one-line abstract excerpt under each row.

3. Ask: "Which papers to download? (e.g. 1,3,5 or 'all')"
4. Call `download_with_fallback` for each selected paper (tries publisher link → Unpaywall → open access)
5. Return structured output (see below)

## Output

Pass this JSON to the next step (`paper-zotero` or `paper-note`):

```json
{
  "papers": [
    {
      "title": "Attention Is All You Need",
      "authors": ["Vaswani", "Shazeer", "Parmar"],
      "year": 2017,
      "doi": "10.48550/arXiv.1706.03762",
      "source": "arxiv",
      "pdf_path": "/tmp/papers/attention-is-all-you-need.pdf",
      "abstract": "We propose a new simple network architecture..."
    }
  ]
}
```

## Common Issues

- **PDF unavailable**: `download_with_fallback` tries Unpaywall automatically. If still unavailable, set `"pdf_path": null` and continue with remaining papers.
- **Too many results**: Remind user to narrow date range or add more specific terms.
