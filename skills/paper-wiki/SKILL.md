---
name: paper-wiki
description: Use when user wants to build or update a cumulative Karpathy-style wiki in Obsidian from academic papers. Produces deep, chapter-by-chapter paper breakdowns and detailed concept entries — not summaries. Use when user says "add to my wiki", "build wiki entries", "deep dive on this paper", "explain this paper Karpathy style", or when paper-research calls this sub-skill.
---

# Paper Wiki (Karpathy Style)

## Philosophy

Karpathy's writing style is the model: **intuition before formalism, concrete before abstract, honest about what's new vs. borrowed**. A good wiki entry reads like a smart friend explaining a paper over coffee — not an abstract. Every concept earns its place by connecting to something the reader already knows.

This skill produces two kinds of output that compound over time:

1. **Paper pages** (`Wiki/Papers/`) — deep chapter-by-chapter breakdowns of each paper
2. **Concept entries** (`Wiki/Concepts/`) — standalone deep dives into methods, terms, and phenomena, augmented each time a new paper touches them

Running this on 5 papers produces a small wiki. Running on 50 produces a rich, interconnected knowledge graph.

---

## Visual Design System

All wiki pages use a consistent visual language optimized for Obsidian reading experience:

### Frontmatter (Dataview Integration)

Every paper and concept page includes structured YAML frontmatter for Dataview queries. This enables automatic indexing, filtering, and dashboard generation.

### Callout Types

Use Obsidian callouts to highlight key sections:

| Callout | Usage | Example |
|---------|-------|---------|
| `> [!abstract]` | TL;DR / one-line summary | Paper and concept summaries |
| `> [!success]` | Key Findings / positive results | Findings with evidence |
| `> [!warning]` | Critical questions / limitations | "What I'd Question" sections |
| `> [!tip]` | Practical advice / adoption | Adoption Guide, "When to Use" |
| `> [!info]` | Prerequisites / context | Background knowledge needed |
| `> [!example]` | Concrete examples / case studies | Task examples, data instances |
| `> [!danger]` | Safety concerns / red flags | Medical safety, data leakage risks |

### Emoji Section Headers

Use emoji sparingly but consistently as visual anchors:

| Emoji | Section |
|-------|---------|
| 🎯 | Why This Exists / Motivation |
| 📋 | Quick Reference / Overview |
| 🔬 | Method / Architecture / Task Definitions |
| 📊 | Results / Baseline / Performance |
| 💡 | Key Findings / Insights |
| ⚠️ | What I'd Question / Limitations |
| 🧩 | Key Concepts |
| 🔗 | Related Papers |
| 🛠️ | Adoption Guide / Practical |
| 📦 | Dataset Construction |
| 📐 | Evaluation Framework |
| 🗺️ | Taxonomy / Framework (surveys, frameworks) |
| 🏗️ | Architecture (technical reports) / The Framework (frameworks) |
| 🔎 | Study Design (analyses) |

### Formula Rendering (LaTeX)

Obsidian uses MathJax. Always render formulas in LaTeX:
- **Block formula**: `$$..$$` on its own line (with blank lines before and after)
- **Inline formula**: `$...$` within text
- After each block formula, annotate every variable on first use with inline `$...$`
- Never write formulas as plain text in blockquotes (`> h_t = tanh(...)`) — always use LaTeX

### Tag Taxonomy

Frontmatter `tags` use a hierarchical system:

```yaml
tags:
  - type/benchmark          # type/method, type/analysis, type/framework, type/survey, type/technical-report, type/concept
  - domain/personalization  # domain/memory, domain/recommendation, domain/education, domain/medical
  - modality/text           # modality/multimodal, modality/vision
  - venue/neurips-2025      # venue/preprint, venue/acl-2025, venue/iclr-2026
  - status/complete         # status/reading, status/to-read
```

### Reading Status

Track reading progress in frontmatter:
- `status: to-read` — queued
- `status: reading` — in progress
- `status: complete` — wiki page written

---

## Required MCP

`ajtruex/mcp-obsidian-streamable-http` — already installed.

If MCP is unavailable, write all files directly to the local filesystem using the path provided by the user.

### Local Filesystem Fallback Convention (no Obsidian MCP, no Zotero)

For a personal paper-reading folder that spans multiple unrelated research topics (as opposed to one shared, synced wiki), use a **topic-first** layout instead of the flat `Wiki/Papers/{type}/` used above:

```
{project_root}/
  {topic}/              e.g. rag/, memory/
    raw/                original PDFs for this topic (ungrouped by type)
    wiki/
      {type}/           surveys/ methods/ benchmarks/ analyses/ frameworks/ technical-reports/
      concepts/         concept entries touched by this topic's papers (Step 4)
```

- `raw/` holds the actual PDF files. Typically listed in `.gitignore` so PDFs stay local and don't get pushed to the repo. Do not put PDFs under `wiki/`.
- `wiki/{type}/` holds the generated paper notes, using the same Step 3 type taxonomy and templates.
- `wiki/concepts/` replaces `Wiki/Concepts/` from Step 4 — same rules (create if missing, augment with `### As used in [[paper-slug]]` if it exists), just scoped to this topic instead of a global concepts folder.
- Ask the user for the `{topic}` slug before creating the structure — do not infer it silently from the paper title.
- This is a different convention from the tag-based one below (topic via folder, not via `domain/*` frontmatter). Do not mix the two inside the same project — pick one per project root.

---

## Input

Accepts `paper-note` or `paper-zotero` output, or ask user for:
- Paper PDF paths or Zotero item keys
- Wiki root folder (default: `Wiki/`)
- **Language** (`lang`): `en` (English) / `zh` (中文) / `zh-en` (混合). Default: `en`. All prose in the wiki page and concept entries is written in the specified language. Code, equations, paper titles, and author names remain in their original form regardless of `lang`.

---

## Step-by-Step Process

### Step 1 — Verify PDF and read the paper

**Before writing anything**, confirm a PDF source is available:
- Check if a local PDF path was provided
- If not, try `get_item_fulltext` from Zotero using the `zotero_key`
- If neither is available: do NOT generate a wiki entry from the abstract alone. Log the paper as "PDF missing — skipped wiki" in the output report and move to the next paper.

Read the full PDF. Do not proceed from the abstract alone — the method section and experiments are essential.

Focus reading on:
- Abstract + Introduction (motivation and claim)
- Background / Related Work (what prior work does this build on?)
- Method / Model (the core contribution)
- Experiments (what was tested, what was held out)
- Conclusion + Limitations (what the authors admit)

### Step 1b — Check publication venue

Before writing, determine the paper's publication status:
1. Get the paper's arXiv URL from Zotero (`get_items_details` → `url` or `archiveID` field) or from the user-provided arXiv ID.
2. Fetch the arXiv abstract page (`https://arxiv.org/abs/{{arxiv_id}}`) and look for the **Comments** field. Common patterns:
   - `"Accepted at NeurIPS 2025"` → venue is `NeurIPS 2025`
   - `"Published as a workshop paper in Lifelong Agent @ ICLR 2026"` → venue is `Workshop @ ICLR 2026 (Lifelong Agent)`
   - `"Accepted to ACL 2025 Findings"` → venue is `ACL 2025 Findings`
   - `"Under review"` or no comment → venue is `Preprint`
3. If no venue information is found, set venue to `Preprint`.

**Venue hierarchy** (for the reader's quick calibration):
- **Main conference** (Oral / Spotlight / Poster): top-tier publication
- **Findings**: peer-reviewed but not selected for main proceedings (e.g. ACL Findings)
- **Workshop**: satellite event, lighter review, early-stage work — always mark as `Workshop @ {{Conference}} ({{Workshop Name}})`
- **Preprint**: not yet peer-reviewed

Place the venue on the **Venue** line in the wiki page (see templates below).

### Step 2 — Extract 4–8 key concepts

For each paper, identify the concepts worth a dedicated wiki entry. Prefer:
- Novel methods introduced by this paper
- Existing techniques the paper builds on (if not already in the wiki)
- Terms that will appear again in future papers

Normalize to hyphenated slug: `"Adaptive Conformal Inference"` → `adaptive-conformal-inference`

### Step 3 — Classify and write the paper wiki page

Determine the paper type and use the corresponding template:

- **Benchmark**: primary contribution is a dataset, evaluation suite, or leaderboard. The paper defines tasks, collects/curates data, and tests existing models. Place in `Wiki/Papers/benchmarks/`.
- **Method**: primary contribution is a novel algorithm, architecture, or system. Place in `Wiki/Papers/methods/`.
- **Analysis**: primary contribution is an empirical study, ablation, or deep investigation of existing methods' behavior, failure modes, or properties. Does NOT propose a new method — the value is the insight, not the technique. Includes negative results and reproducibility studies. Place in `Wiki/Papers/analyses/`.
- **Framework**: primary contribution is a conceptual framework, taxonomy, or position paper that proposes a new way of thinking about a problem. Stronger than a survey (takes a stance), weaker than a method (doesn't implement a full solution). Includes position papers and perspective papers. Place in `Wiki/Papers/frameworks/`.
- **Survey**: primary contribution is organizing, categorizing, and synthesizing a research field. The paper reviews many existing works and proposes a taxonomy or framework for understanding the landscape. Place in `Wiki/Papers/surveys/`.
- **Technical Report**: primary contribution is a foundation model, infrastructure, or system design. Typically from industry labs — describes architecture, training, and capabilities without formal novelty claim. Includes model cards, system papers, and seminal architecture papers. Place in `Wiki/Papers/technical-reports/`.

Use the corresponding template below.

### Step 4 — Write or augment concept entries

For each concept from Step 2:
- Check if `Wiki/Concepts/{{concept-slug}}.md` exists
- If **no** → create it using the **Concept Entry Template** below
- If **yes** → open it, add a new section `### As used in [[paper-slug]]` with how this paper uses or extends the concept

### Step 5 — Cross-link

- In the paper page, add `[[concept-slug]]` wiki links on first mention of each concept
- In each concept entry, add the paper to the `## Papers` backlink section

### Step 6 — Update MOC (Map of Content)

After creating/updating paper and concept pages, update the MOC index:

1. Check if `Wiki/MOC.md` exists
2. If **no** → create it using the **MOC Template** below
3. If **yes** → add new entries to the appropriate sections

The MOC serves as the central navigation hub for the entire wiki.

### Step 7 — Synthesis Check

After completing all paper/concept/MOC updates, perform a synthesis check:

#### 7a — Auto-suggest (first synthesis)

If `Wiki/Synthesis/` does not exist or is empty:
1. Count the total number of paper pages in `Wiki/Papers/` (across all subdirectories)
2. If **≥ 10 papers** → prompt the user:
   > "Wiki 中已有 {{N}} 篇论文。是否需要生成一份 Synthesis（跨论文横向对比 + 共性发现 + 开放问题）？"
3. If the user agrees → read `templates/synthesis.md`, generate the synthesis page in `Wiki/Synthesis/`

#### 7b — Auto-update (existing synthesis)

If `Wiki/Synthesis/` already contains synthesis pages:
1. For each synthesis page, read its `scope` frontmatter field (list of paper wiki-links)
2. Check if the **newly added paper** belongs to the same research direction as an existing synthesis (based on `domains` tag overlap or subdirectory match)
3. If yes → **automatically update** the existing synthesis:
   - Add the new paper to the `scope` list in frontmatter
   - Add a row to the relevant Landscape comparison table
   - Check if the new paper provides evidence for existing Findings → update Evidence sections
   - Check if the new paper contradicts existing Findings → add to Contradictions
   - Update the `updated` date in frontmatter
   - Update the Findings evidence heatmap counts
4. Report what was updated:
   ```
   Synthesis updated: Wiki/Synthesis/{{slug}}.md
     + Added [[new-paper]] to scope (now {{N}} papers)
     + Updated F2 (隐式偏好) with new evidence
     + Added new row to Method comparison table
   ```

---

## Templates (loaded on demand)

Templates are stored in `templates/` next to this file. **Read only the ones you need** — do not load all templates at once.

### Paper type → template

| Paper Type | Template File | Output Path |
|-----------|--------------|-------------|
| **Benchmark** — dataset, evaluation suite, leaderboard | `templates/benchmark.md` | `Wiki/Papers/benchmarks/` |
| **Method** — novel algorithm, architecture, system | `templates/method.md` | `Wiki/Papers/methods/` |
| **Survey** — field synthesis, taxonomy of existing work | `templates/survey.md` | `Wiki/Papers/surveys/` |
| **Analysis** — empirical study, ablation, negative results | `templates/analysis.md` | `Wiki/Papers/analyses/` |
| **Framework** — conceptual framework, position paper | `templates/framework.md` | `Wiki/Papers/frameworks/` |
| **Technical Report** — foundation model, system paper | `templates/technical-report.md` | `Wiki/Papers/technical-reports/` |

### Always-loaded templates

These are used for every paper regardless of type — load them alongside the paper template:

| Template | When |
|----------|------|
| `templates/concept.md` | Step 4 — creating new concept entries |
| `templates/moc.md` | Step 6 — creating or updating the MOC |
| `templates/synthesis.md` | Step 7 — creating or updating synthesis pages |

### How to use

In Step 3, after classifying the paper type:
1. Read the corresponding paper template file (e.g. `templates/method.md`)
2. Read `templates/concept.md` for Step 4
3. Read `templates/moc.md` for Step 6 (if MOC needs creation)
4. Read `templates/synthesis.md` for Step 7 (if synthesis creation/update needed)


---

## Augmentation Rules (for Existing Concept Entries)

When a paper uses a concept already in the wiki:

1. Add the paper to the `## Papers` table
2. If the paper introduces a **variant**: add a new row to `## Variants and Extensions`
3. If the paper **challenges or corrects** the concept: add a `### Debate` subsection
4. If the paper adds a **new mental model** or analogy: add it as a `### Alternative Mental Model` subsection
5. Never delete existing content — only add or annotate

---

## Depth Standards

Each method paper page should be **600–1200 words** of actual content (not counting template headers).
Each benchmark paper page should be **500–1000 words** — the Quick Reference table and Baseline Results table carry significant information density, so the prose can be tighter.
Each concept entry should be **400–800 words**.

Short entries are a smell: they usually mean the concept wasn't understood deeply enough.
Long rambling entries are also a smell: they mean the key insight wasn't identified.

The test: could someone read this entry and then read the paper, and have the paper feel like "filling in the details" rather than "explaining from scratch"?

---

## Output Report

After completing a batch:

```
Paper pages created:
  Wiki/Papers/soft-msm-time-series-alignment.md

Concept entries created (3):
  Wiki/Concepts/elastic-time-series-alignment.md
  Wiki/Concepts/move-split-merge-distance.md
  Wiki/Concepts/soft-dtw.md

Concept entries updated (1):
  Wiki/Concepts/dynamic-time-warping.md  (+1 paper, +1 variant)

MOC updated: Wiki/MOC.md (+1 paper, +3 concepts)

Synthesis: Wiki 中已有 12 篇论文，是否生成 Synthesis？
  — or —
Synthesis updated: Wiki/Synthesis/personalization-landscape.md
  + Added [[soft-msm]] to scope (now 13 papers)
  + Updated F3 (长上下文衰减) with new evidence
```

---

## Common Issues

- **Reading only the abstract**: Always read the method section. Abstracts lie by omission. If no PDF is available, skip the wiki entry entirely — do not generate from the abstract.
- **Language drift**: Pick one language (`lang`) and write all prose consistently in that language for the entire batch. Do not switch mid-entry.
- **Granularity mismatch**: Don't create concept entries for things like "Adam optimizer" unless the paper makes a novel contribution to it. Prefer meaningful depth over breadth.
- **Duplicate slugs**: "self-attention" and "self attention" are the same. Always normalize.
- **Augmenting blindly**: When updating an existing concept entry, re-read it first. Don't add contradictory information without flagging the tension explicitly.

---

## Post-Modification GitHub Sync

After every wiki modification session (adding/updating paper pages, concept entries, or skill changes), ask the user:

> "Wiki 已更新。是否需要 push 到 GitHub？"

If yes:
1. Stage and commit the changed files in the relevant repo (the wiki's own repo, or `Geek96/paper-research-skill` for skill updates)
2. Push to remote

This ensures the GitHub repos stay in sync with local changes.
