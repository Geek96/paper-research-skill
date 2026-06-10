---
name: paper-dataview
description: Use when user wants queryable YAML frontmatter on Obsidian paper notes so they can use the Dataview plugin to filter, sort, or display papers as tables. Runs alongside paper-note, paper-moc, or paper-wiki. Use when user says "add metadata to my notes" or "I want to query my papers with Dataview".
---

# Paper Dataview

## Overview

Adds standardized YAML frontmatter to Obsidian paper notes, enabling queries with the Dataview plugin. Can be run on new notes (alongside `paper-note`) or retroactively on existing notes.

## Required MCP

`MarkusPfundstein/mcp-obsidian` must be installed and configured.

## Input

Accepts paper metadata from `paper-fetch` / `paper-zotero` output, or ask user for:
- Note file path(s) to add frontmatter to
- Zotero item key (if available)
- Custom tags (optional)

## Steps

1. For each paper, collect: title, authors, year, DOI, source, Zotero key, user tags
2. Read existing note via `mcp-obsidian` (if note already exists)
3. Prepend YAML frontmatter block (insert before existing content, or add at top if none)
4. Write updated note back via `mcp-obsidian`

## YAML Frontmatter Template

```yaml
---
title: "{{title}}"
authors: ["{{Author1}}", "{{Author2}}"]
year: {{year}}
doi: "{{doi}}"
source: "{{arxiv|pubmed|biorxiv|other}}"
tags: [{{topic-tag-1}}, {{topic-tag-2}}]
zotero_key: "{{zotero_key}}"
read_status: unread
date_added: {{YYYY-MM-DD}}
---
```

## Example Dataview Queries

After frontmatter is added, users can query in Obsidian:

```dataview
TABLE authors, year, source
FROM "Papers"
WHERE contains(tags, "transformer")
SORT year DESC
```

```dataview
TABLE title, read_status
FROM "Papers"
WHERE read_status = "unread"
```

## Output

Confirmation of updated notes:
```
Updated frontmatter in:
  Papers/attention-is-all-you-need.md
  Papers/bert-pre-training.md
```

## Common Issues

- **Existing frontmatter**: Check for an existing `---` block before prepending. Merge fields rather than overwriting if a note already has partial frontmatter.
- **Author list format**: Always write as a YAML list `["Author1", "Author2"]`, not a comma-separated string.
