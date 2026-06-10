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

## Required MCP

`ajtruex/mcp-obsidian-streamable-http` — already installed.

If MCP is unavailable, write all files directly to the local filesystem using the path provided by the user.

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

### Step 2 — Extract 4–8 key concepts

For each paper, identify the concepts worth a dedicated wiki entry. Prefer:
- Novel methods introduced by this paper
- Existing techniques the paper builds on (if not already in the wiki)
- Terms that will appear again in future papers

Normalize to hyphenated slug: `"Adaptive Conformal Inference"` → `adaptive-conformal-inference`

### Step 3 — Write the paper wiki page

Create `Wiki/Papers/{{paper-slug}}.md` using the **Paper Page Template** below.

### Step 4 — Write or augment concept entries

For each concept from Step 2:
- Check if `Wiki/Concepts/{{concept-slug}}.md` exists
- If **no** → create it using the **Concept Entry Template** below
- If **yes** → open it, add a new section `### As used in [[paper-slug]]` with how this paper uses or extends the concept

### Step 5 — Cross-link

- In the paper page, add `[[concept-slug]]` wiki links on first mention of each concept
- In each concept entry, add the paper to the `## Papers` backlink section

---

## Formatting Rules

**All mathematical expressions must use GitHub-compatible LaTeX syntax.**

- Display equations (standalone, centered): `$$...$$`
- Inline variables and operators: `$...$`

| Instead of | Use |
|------------|-----|
| `·` | `$\cdot$` |
| `√` | `$\sqrt{}$` |
| `∑` | `$\sum$` |
| `≤` / `≥` | `$\leq$` / `$\geq$` |
| `×` | `$\times$` |
| `O(n²)` | `$O(n^2)$` |

GitHub renders LaTeX via MathJax in `.md` files. Never write plain-text Unicode math symbols in equation context.

---

## Paper Page Template

File: `Wiki/Papers/{{paper-slug}}.md`

```markdown
# {{Full Paper Title}}

> **TL;DR**: {{One sharp sentence — the core claim, not a description of structure.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})

---

## Why This Paper Exists

{{2–3 paragraphs on the problem this paper solves. Write in plain language.
What was broken or missing before? What makes this hard?
What would happen if you didn't have this paper's contribution?
Karpathy voice: be direct, use concrete examples, avoid hedging.}}

## Prerequisites

To understand this paper, you need:
- [[concept-A]] — {{one-line explanation of why it's needed here}}
- [[concept-B]] — {{one-line explanation}}
- [[concept-C]] — {{one-line explanation}}

If these are unfamiliar, read the concept entries first.

---

## Chapter-by-Chapter Breakdown

### 1. Introduction / Motivation

{{What problem does the intro set up? What claim does it make?
Paraphrase the core argument in 2–3 sentences. Is the framing honest about prior work?
Note any surprising or counterintuitive claims the authors make upfront.}}

### 2. Background / Prior Work

{{What are the 2–3 key papers this builds on? What does this paper need from them?
Don't just list citations — explain the dependency.
Example: "This paper needs [[DTW]] because the core metric is built on top of it."}}

### 3. The Method — How It Actually Works

{{This is the longest section. Walk through the method step by step.

For each key component:
- What does it do? (one sentence intuition)
- How does it work? (mechanistic explanation)
- If there's a key equation: write it out, then unpack every term.

Example format:
> The core update rule is:
> $$h_t = \tanh(W_{hh} \cdot h_{t-1} + W_{xh} \cdot x_t)$$
> - $h_t$: the new hidden state (what the network "knows" at step t)
> - $h_{t-1}$: what it knew last step
> - $x_t$: the new input
> - $W_{hh}$ and $W_{xh}$: learned weight matrices
> - $\tanh$: squishes values to $[-1, 1]$ so things don't explode

Repeat for each component. Include ASCII diagrams if they aid clarity.}}

### 4. The Key Innovation

{{In 1–2 paragraphs: what is genuinely new in this paper?
Be specific and critical. Examples:
- "The novel part is X. Prior work did Y instead, which caused problem Z."
- "This looks like technique A from [prior paper], but differs in that..."
Karpathy style: distinguish between what's actually new and what's packaging.}}

### 5. Experiments

{{What did they test? On what data? Against what baselines?

For each key result:
- What metric? Why does that metric matter?
- How big is the improvement? Is it meaningful in practice?
- What did they NOT test that you'd want to see?

Be concrete: "They report a 3.2% improvement on benchmark X. Baseline Y was strong.
They did not test on out-of-distribution data, which would matter most for deployment."}}

### 6. What I'd Try (or Question)

{{Karpathy-style actionable section. 3–5 bullet points on:
- What experiment would you run next?
- What assumption seems shakiest?
- What would you change in the method?
- What does this suggest for your own work?

Be specific. "I would try X because Y" not "future work should explore X."}}

### 7. Limitations (Honest Version)

{{The authors' stated limitations + any they missed.
What does the method fail on? What assumptions does it make?
When would you NOT use this approach?}}

---

## Key Concepts from This Paper

{{Bulleted list of wiki links to concept entries created/updated for this paper:}}
- [[concept-a]] — {{one-line}]
- [[concept-b]] — {{one-line}}

---

## Related Papers

- [[related-paper-slug]] — {{why it's related: shares method / problem / dataset}}
```

---

## Concept Entry Template

File: `Wiki/Concepts/{{concept-slug}}.md`

```markdown
# {{Concept Name}}

> **In one sentence**: {{The sharpest, most concrete definition. Avoid "is a method that" — describe what it does.}}

---

## Intuition First

{{2–3 paragraphs explaining the concept from scratch, as if to someone who hasn't seen it.
Use an analogy or a toy example. Make it concrete.
Karpathy rule: if you can't explain it simply, you don't understand it yet.

Example for "attention mechanism":
"Imagine you're reading a sentence and need to decide which words to focus on
to translate the current word. Attention lets the model learn to do exactly this —
it computes a weighted sum over all positions in the input, where the weights
represent 'how relevant is this position to the current step?'"}}

## The Formal Version

{{Now give the precise definition. Include the key equation(s).
After each equation, unpack every term:}}

> {{Key equation, e.g.:}}
> $$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right) V$$

- **$Q$** (Query): {{what it represents, shape, intuition}}
- **$K$** (Key): {{what it represents, intuition}}
- **$V$** (Value): {{what it represents, intuition}}
- **$\sqrt{d_k}$**: {{why we divide — prevents dot products from growing too large}}

## How to Think About It (Mental Model)

{{A single concrete mental model or analogy that makes the concept stick.
This should be different from the intuition paragraph — more specific, more memorable.

Example: "Think of attention as a soft dictionary lookup:
Q is your search query, K is the set of keys, V is the values.
Unlike a hard lookup (exact match), you get a weighted blend of all values."}}

## Common Misconceptions

{{2–4 bullet points on things people get wrong about this concept.
Example:
- "Attention does NOT mean the model is 'looking at' specific tokens in a human sense — the weights are differentiable approximations."
- "Higher attention weight ≠ more 'important' in a causal sense."}}

## Variants and Extensions

{{List known variants with one-line descriptions.
Example: "Multi-head attention: runs H attention heads in parallel, then concatenates."
Link to concept entries for each variant if they exist.}}

## When to Use It (and When Not To)

{{Practical guidance:
- Works well when: ...
- Breaks down when: ...
- Prefer X instead when: ...}}

---

## Papers

Papers that introduce or use this concept (augmented automatically):

| Paper | How it's used |
|-------|--------------|
| [[paper-slug]] | {{one-line: introduced / extends / applies to X domain}} |
```

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

Each paper page should be **600–1200 words** of actual content (not counting template headers).
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
```

---

## Common Issues

- **Reading only the abstract**: Always read the method section. Abstracts lie by omission.
- **Granularity mismatch**: Don't create concept entries for things like "Adam optimizer" unless the paper makes a novel contribution to it. Prefer meaningful depth over breadth.
- **Duplicate slugs**: "self-attention" and "self attention" are the same. Always normalize.
- **Augmenting blindly**: When updating an existing concept entry, re-read it first. Don't add contradictory information without flagging the tension explicitly.
