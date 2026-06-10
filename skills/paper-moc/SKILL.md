---
name: paper-moc
description: Use when user wants a topic overview or index page in Obsidian that aggregates multiple related papers. Use when user says "create a map of content for my papers on X", "give me an overview of these papers", or when paper-research calls this sub-skill.
---

# Paper MOC (Map of Content)

## Overview

Reads a set of paper notes from the Obsidian vault and generates a single MOC (Map of Content) page: a topic summary + categorized index of all related papers. Uses `mcp-obsidian` to read existing notes and write the MOC.

## Required MCP

`MarkusPfundstein/mcp-obsidian` must be installed and configured.

## Input

Ask user for:
- Topic name (e.g. "Diffusion Models for Medical Imaging")
- Which paper notes to include (folder path, or list of note titles)

## Steps

1. Read all specified paper notes via `mcp-obsidian`
2. Identify 2–4 sub-themes that group the papers (based on method, application area, or chronology)
3. Write a 2–3 paragraph topic overview synthesizing the field
4. Create `MOC-{{topic}}.md` with the structure below

## MOC Template

File path: `MOCs/MOC-{{topic-slug}}.md`

```markdown
# MOC: {{topic}}

> {{2–3 paragraph overview of the research area: what problem it addresses, how approaches have evolved, open questions}}

## Sub-themes

### {{Sub-theme 1}}
- [[{{paper-note-title}}]] — {{one-line description}}
- [[{{paper-note-title}}]] — {{one-line description}}

### {{Sub-theme 2}}
- [[{{paper-note-title}}]] — {{one-line description}}

## Chronological View
| Year | Paper | Key Contribution |
|------|-------|-----------------|
| 2017 | [[Attention Is All You Need]] | Introduced transformer architecture |

## Open Questions
- {{unresolved question surfaced across these papers}}
```

## Output

Path of the created MOC file:
```
MOCs/MOC-diffusion-models-medical-imaging.md
```

## Common Issues

- **Too few papers**: MOC is most useful with 5+ papers. For fewer, suggest using `paper-note` only.
- **Sub-theme overlap**: If papers don't cluster neatly, use chronological grouping instead.
