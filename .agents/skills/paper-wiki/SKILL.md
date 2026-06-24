---
name: paper-wiki
description: Use when user wants to build or update a cumulative Karpathy-style wiki in Obsidian from academic papers. Use when user says "add to my wiki", "build wiki entries from these papers", "update my knowledge base from papers", or when paper-research calls this sub-skill. Requires AgriciDaniel/claude-obsidian to be installed.
---

# Paper Wiki

## Overview

Builds a compounding wiki in Obsidian from paper notes using the Karpathy LLM Wiki pattern (`AgriciDaniel/claude-obsidian`). Each run extracts key concepts from papers and creates or updates wiki entries, linking them to existing vault content. The wiki grows richer with every batch.

## Required Skills & MCP

**`AgriciDaniel/claude-obsidian`** skill must be installed:
```bash
npx skills add AgriciDaniel/claude-obsidian
```

**`MarkusPfundstein/mcp-obsidian`** MCP must be configured.
See setup guide: https://github.com/MarkusPfundstein/mcp-obsidian

## Input

Accepts `paper-note` output (list of note paths), or ask user for:
- Paper note paths or folder in Obsidian vault
- Wiki folder in vault (default: `Wiki/`)

## Steps

1. Read each paper note via `mcp-obsidian`
2. For each paper, extract 3–7 key concepts (methods, terms, model names, phenomena)
3. For each concept:
   a. Normalize to hyphenated slug (e.g. "Self-Attention" → `self-attention`)
   b. Check if a wiki entry already exists in the `Wiki/` folder
   c. If yes → read existing entry and augment with new information from this paper
   d. If no → create a new wiki entry using the `/wiki` command from `claude-obsidian`
4. In each paper note, add `[[wiki-links]]` to the concepts extracted from it
5. In each wiki entry, add a "Referenced in" section linking back to source paper notes

## Wiki Entry Template

File path: `Wiki/{{concept-slug}}.md`

```markdown
# {{Concept Name}}

{{2–3 sentence definition or explanation of the concept}}

## Key Properties
- {{property or characteristic}}

## How It's Used
{{how this concept appears in the literature}}

## Referenced In
- [[{{paper-note-title}}]] ({{year}})
```

## Design Principle

This skill is designed to compound. Running it on 5 papers produces a small wiki. Running it on 50 papers produces a rich, interconnected knowledge graph. Always check for existing entries before creating new ones — augment, don't duplicate.

## Output

Summary of wiki activity:
```
Created 4 new wiki entries:
  Wiki/attention-mechanism.md
  Wiki/positional-encoding.md
  Wiki/multi-head-attention.md
  Wiki/transformer-architecture.md

Updated 2 existing entries:
  Wiki/self-attention.md (+1 paper reference)
  Wiki/encoder-decoder.md (+2 paper references)
```

## Common Issues

- **Duplicate concepts**: "Self-attention" and "self attention" are the same concept. Always normalize to hyphenated slug before checking for existing entries.
- **Overly granular concepts**: Prefer 5 meaningful concepts per paper over 20 fine-grained ones. Quality over quantity.
