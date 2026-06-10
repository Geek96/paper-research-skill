# paper-research-skill

A modular Claude Code skill for the full academic paper research pipeline:

**search / download → Zotero → Obsidian wiki**

## Install

```bash
npx skills add Geek96/paper-research-skill
```

## Prerequisites

Install these MCP tools before use:

| Tool | Purpose | Link |
|------|---------|------|
| `openags/paper-search-mcp` | Search arXiv, PubMed, Semantic Scholar + download PDFs | [GitHub](https://github.com/openags/paper-search-mcp) |
| `Xevos117/mcp-zotero` | Import papers to Zotero by DOI or PDF | [GitHub](https://github.com/Xevos117/mcp-zotero) |
| `MarkusPfundstein/mcp-obsidian` | Read/write Obsidian vault | [GitHub](https://github.com/MarkusPfundstein/mcp-obsidian) |
| `AgriciDaniel/claude-obsidian` | Karpathy wiki pattern (for `paper-wiki` only) | [GitHub](https://github.com/AgriciDaniel/claude-obsidian) |

Quick install for paper-search-mcp:
```bash
npx -y @smithery/cli install @openags/paper-search-mcp --client claude
```

## Skills

### Entry Point
- **`paper-research`** — Full pipeline orchestrator. Start here.

### Sub-skills (also usable standalone)

| Skill | Trigger example |
|-------|----------------|
| `paper-fetch` | "Find papers on X" / provide DOI list |
| `paper-zotero` | "Import these papers to Zotero" |
| `paper-note` | "Summarize this paper in Obsidian" |
| `paper-moc` | "Create a topic overview for my papers on X" |
| `paper-wiki` | "Add these papers to my wiki" |
| `paper-dataview` | "Add Dataview metadata to my notes" |

## Pipeline

```
User input (keywords or DOI list)
        ↓
  paper-fetch → candidate list → user selects
        ↓
  paper-zotero → Zotero library
        ↓
  paper-research asks: which Obsidian output?
        ↓
  paper-note / paper-moc / paper-wiki / paper-dataview (choose one or more)
```

## Example Usage

```
> Research the latest papers on RAG for code generation, last 2 years
> Import these 5 DOIs to Zotero and create wiki entries
> Summarize this PDF and create a note in my Obsidian vault
```

## License

MIT © [Geek96](https://github.com/Geek96)
