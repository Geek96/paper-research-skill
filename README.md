<p align="center">
  <img src="https://img.shields.io/badge/Claude_Code-Skill-7C3AED?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IndoaXRlIiBzdHJva2Utd2lkdGg9IjIiPjxwYXRoIGQ9Ik0xMiAyMGgxYTEgMSAwIDAgMCAxLTFWM2ExIDEgMCAwIDAtMS0xSDZhMSAxIDAgMCAwLTEgMXYyIi8+PHBhdGggZD0iTTEyIDRWMmExIDEgMCAwIDEgMS0xaDRhMSAxIDAgMCAxIDEgMXY0Ii8+PC9zdmc+" alt="Claude Code Skill"/>
  <img src="https://img.shields.io/github/v/release/Geek96/paper-research-skill?style=for-the-badge&color=10B981" alt="Latest Release"/>
  <img src="https://img.shields.io/github/license/Geek96/paper-research-skill?style=for-the-badge&color=6B7280" alt="MIT License"/>
</p>

<h1 align="center">📚 paper-research-skill</h1>

<p align="center">
  <strong>A modular Claude Code skill for the full academic paper research pipeline</strong>
  <br/>
  <code>search / download → Zotero → Obsidian wiki</code>
</p>

<p align="center">
  <strong>English</strong> | <a href="README.zh-CN.md">简体中文</a>
</p>

---

## ✨ Features

- **Multi-source search** — arXiv, PubMed, Semantic Scholar, bioRxiv, Google Scholar
- **Zotero integration** — import by DOI or PDF with auto-deduplication
- **Karpathy-style wiki** — deep paper breakdowns with intuition-first concept entries
- **6 paper types** — benchmark, method, analysis, framework, survey, technical report
- **LaTeX formula rendering** — MathJax-compatible equations in Obsidian
- **Dataview-ready** — structured frontmatter for queries, dashboards, and MOCs
- **Multilingual** — output in English, 中文, or mixed

---

## 🚀 Quick Start

```bash
npx skills add Geek96/paper-research-skill
```

Then in Claude Code:

```
> Research the latest papers on RAG for code generation, last 2 years
> Import these 5 DOIs to Zotero and create wiki entries
> Summarize this PDF and create a note in my Obsidian vault
```

---

## 🧩 Skills Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    paper-research                            │
│               (Full pipeline orchestrator)                   │
│                                                             │
│  User input (keywords / DOIs / PDF paths)                   │
│         │                                                   │
│         ▼                                                   │
│  ┌─────────────┐    ┌──────────────┐                        │
│  │ paper-fetch  │───▶│ paper-zotero │                        │
│  │  Search &    │    │  Import to   │                        │
│  │  Download    │    │  Zotero      │                        │
│  └─────────────┘    └──────┬───────┘                        │
│                            │                                │
│                   ┌────────┴────────┐                       │
│                   ▼                 ▼                        │
│           ┌────────────┐    ┌───────────┐                   │
│           │ paper-wiki │    │ paper-moc │                    │
│           │ Deep wiki  │    │ Topic map │                    │
│           └─────┬──────┘    └───────────┘                   │
│                 ▼                                            │
│          ┌──────────────┐                                    │
│          │paper-dataview│                                    │
│          │  Frontmatter │                                    │
│          └──────────────┘                                    │
└─────────────────────────────────────────────────────────────┘
```

| Skill | What it does | Trigger example |
|-------|-------------|----------------|
| **`paper-research`** | Full pipeline orchestrator — start here | "Research papers on X" |
| `paper-fetch` | Search & download PDFs | "Find papers on X" / provide DOIs |
| `paper-zotero` | Import to Zotero with dedup | "Import these papers to Zotero" |
| `paper-wiki` | Deep Karpathy-style wiki entry | "Add to my wiki" |
| `paper-moc` | Topic Map of Content | "Create overview for topic X" |
| `paper-dataview` | Inject Dataview frontmatter | "Add metadata to my notes" |
| `paper-version` | Check installed version | `/version` |

---

## 📝 Wiki Paper Types

`paper-wiki` classifies papers into 6 types, each with a dedicated template:

| Type | Directory | When to use | Key sections |
|------|-----------|-------------|-------------|
| **Benchmark** | `Papers/benchmarks/` | Datasets, eval suites, leaderboards | Task Definitions → Dataset Construction → Baseline Results |
| **Method** | `Papers/methods/` | Novel algorithms, architectures | The Method → Key Innovation → Experiments |
| **Analysis** | `Papers/analyses/` | Empirical studies, ablations, negative results | Study Design → Key Findings → Core Insight |
| **Framework** | `Papers/frameworks/` | Conceptual frameworks, position papers | Core Argument → Dimensions → Validation |
| **Survey** | `Papers/surveys/` | Field overviews, taxonomies | Taxonomy → Coverage Analysis → Open Problems |
| **Tech Report** | `Papers/technical-reports/` | Foundation models, system papers | Architecture → Training → Capability Analysis |

All templates include:
- 📋 **Quick Reference** table for at-a-glance overview
- `> [!abstract]` / `[!success]` / `[!warning]` / `[!info]` Obsidian callouts
- `$$...$$` block and `$...$` inline LaTeX formula rendering
- Structured frontmatter for Dataview queries
- `[[wiki-links]]` for concept cross-referencing

---

## 🔧 Prerequisites Setup

This skill relies on 3 MCP servers. Follow the steps below for each one.

### 1. Paper Search MCP

> Provides: arXiv, PubMed, Semantic Scholar, bioRxiv search + PDF download

**One-command install via Smithery:**

```bash
npx -y @smithery/cli install @openags/paper-search-mcp --client claude
```

<details>
<summary><strong>Manual install (if Smithery doesn't work)</strong></summary>

1. Clone the repo:
   ```bash
   git clone https://github.com/openags/paper-search-mcp.git
   cd paper-search-mcp
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Build:
   ```bash
   npm run build
   ```

4. Add to your Claude Code MCP config (`~/.claude/settings.json` → `mcpServers`):
   ```json
   {
     "paper-search-mcp": {
       "command": "node",
       "args": ["/path/to/paper-search-mcp/dist/index.js"]
     }
   }
   ```

</details>

**Verify**: In Claude Code, try `Search arXiv for "transformer attention"`. If it returns results, you're good.

---

### 2. Zotero MCP

> Provides: import papers by DOI, manage collections, search library, inject citations

**Step 1 — Get your Zotero API credentials:**

1. Go to [zotero.org/settings/keys](https://www.zotero.org/settings/keys)
2. Click **Create new private key**
3. Give it a name (e.g. "Claude Code")
4. Under **Personal Library**, check:
   - ✅ Allow library access
   - ✅ Allow write access
5. Click **Save Key** and copy the key

6. Find your **User ID**: Go to [zotero.org/settings/keys](https://www.zotero.org/settings/keys) — your user ID is shown at the top of the page (a number like `12345678`)

**Step 2 — Install the MCP server:**

```bash
npx -y @smithery/cli install @Xevos117/mcp-zotero --client claude
```

When prompted, enter:
- `ZOTERO_API_KEY`: your key from Step 1
- `ZOTERO_USER_ID`: your user ID from Step 1

<details>
<summary><strong>Manual install</strong></summary>

1. Clone and build:
   ```bash
   git clone https://github.com/Xevos117/mcp-zotero.git
   cd mcp-zotero
   npm install && npm run build
   ```

2. Add to MCP config:
   ```json
   {
     "mcp-zotero": {
       "command": "node",
       "args": ["/path/to/mcp-zotero/dist/index.js"],
       "env": {
         "ZOTERO_API_KEY": "your_api_key_here",
         "ZOTERO_USER_ID": "your_user_id_here"
       }
     }
   }
   ```

</details>

**Verify**: In Claude Code, try `Search my Zotero library for "attention"`. If it queries your library, you're good.

---

### 3. Obsidian MCP

> Provides: read/write files in your Obsidian vault

You need the **Obsidian Local REST API** plugin + an MCP bridge.

**Step 1 — Install the Obsidian plugin:**

1. Open Obsidian → Settings → Community plugins → Browse
2. Search for **Local REST API**
3. Install and enable it
4. In the plugin settings:
   - Note the **API port** (default: `27123`)
   - Copy the **API Key** (click "Copy API key")
   - Enable HTTPS if you want (optional)

**Step 2 — Install the MCP server:**

```bash
npx -y @smithery/cli install @MarkusPfundstein/mcp-obsidian --client claude
```

When prompted, enter your API key from Step 1.

<details>
<summary><strong>Manual install</strong></summary>

1. Clone and build:
   ```bash
   git clone https://github.com/MarkusPfundstein/mcp-obsidian.git
   cd mcp-obsidian
   npm install && npm run build
   ```

2. Add to MCP config:
   ```json
   {
     "mcp-obsidian": {
       "command": "node",
       "args": ["/path/to/mcp-obsidian/dist/index.js"],
       "env": {
         "OBSIDIAN_API_KEY": "your_api_key_here",
         "OBSIDIAN_API_PORT": "27123"
       }
     }
   }
   ```

</details>

> [!NOTE]
> **Obsidian must be open** with the Local REST API plugin running for the MCP to work. If MCP is unavailable, the skill falls back to writing files directly to your filesystem.

**Verify**: In Claude Code, try `List files in my Obsidian vault`. If it returns your vault contents, you're good.

---

### Summary: MCP Setup Checklist

```
☐ paper-search-mcp  ← npx install (no credentials needed)
☐ mcp-zotero         ← npx install + Zotero API key + User ID
☐ mcp-obsidian       ← npx install + Obsidian Local REST API plugin + API key
```

---

## 📖 Example Workflows

### Research a new topic end-to-end

```
> Research the latest papers on personalized LLM agents, last 2 years
```

The orchestrator will:
1. Search arXiv/Semantic Scholar → present candidates
2. You select papers → PDFs downloaded
3. Import to Zotero with auto-dedup
4. Ask your preferred output → wiki / notes / MOC

### Import a specific list of papers

```
> Import these papers and create wiki entries:
> - 2405.12345
> - 2406.67890
> - 10.1234/example.doi
```

### Add a single paper to your wiki

```
> Add this paper to my wiki: /path/to/paper.pdf
```

### Create a topic overview

```
> Create a Map of Content for all my personalization papers
```

---

## 👀 Demo

> All files in [`demo/`](demo/) are **real outputs** generated by `paper-wiki` — not hand-crafted mock-ups.

### Method — [Attention Is All You Need](demo/methods/attention-is-all-you-need.md)

The paper that started the Transformer era. This demo showcases:

- 7 LaTeX formulas rendered in Obsidian (`$$\text{Attention}(Q,K,V)$$`, `$$\text{MultiHead}$$`, FFN, Positional Encoding)
- Step-by-step method walkthrough (6 sub-sections with diagrams)
- `> [!success]` callout for the key innovation
- `> [!warning]` with 9 critical questions
- 6 linked `[[concept]]` entries with bidirectional backlinks

### Benchmark — [PersonaLens](demo/benchmarks/personalens.md)

ACL 2025 Findings — personalized dialogue evaluation across 20 domains. This demo showcases:

- Quick Reference table with 10 dimensions
- Evaluation rubric inside `> [!example]` callout (Personalization 1–4 scale)
- Baseline results table (5 models × 4 metrics × single/multi-domain)
- `> [!tip]` Adoption Guide with quick-pilot suggestions
- Dataset construction pipeline (7-step curation process)

### Concept Entries

Linked from the method paper — showing the intuition-first Karpathy style:

| Entry | Key Feature |
|-------|-------------|
| [scaled-dot-product-attention.md](demo/concepts/scaled-dot-product-attention.md) | Core formula + "why √d_k" explanation |
| [multi-head-attention.md](demo/concepts/multi-head-attention.md) | Matrix dimension annotations (`$W_i^Q \in \mathbb{R}^{d \times d_k}$`) |
| [positional-encoding.md](demo/concepts/positional-encoding.md) | sin/cos formulas + RoPE/ALiBi variant comparison |

---

## 📁 Project Structure

```
paper-research-skill/
├── skills/
│   ├── paper-research/SKILL.md    # Pipeline orchestrator
│   ├── paper-fetch/SKILL.md       # Search & download
│   ├── paper-zotero/SKILL.md      # Zotero import
│   ├── paper-wiki/SKILL.md        # Deep wiki (templates + visual design system)
│   ├── paper-moc/SKILL.md         # Map of Content
│   ├── paper-dataview/SKILL.md    # Dataview metadata
│   └── paper-version/SKILL.md     # Version check
├── demo/                          # Real wiki output examples
│   ├── methods/                   #   Method paper demo
│   ├── benchmarks/                #   Benchmark paper demo
│   └── concepts/                  #   Concept entry demos
├── .claude-plugin/plugin.json     # Claude Code plugin manifest
├── CHANGELOG.md
├── CLAUDE.md                      # Dev guidelines
├── README.md
└── README.zh-CN.md                # 简体中文
```

---

## 📄 License

MIT © [Geek96](https://github.com/Geek96)
