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

Determine if the paper is a **benchmark** or a **method** paper:
- **Benchmark**: primary contribution is a dataset, evaluation suite, or leaderboard. The paper defines tasks, collects/curates data, and tests existing models. Place in `Wiki/Papers/benchmarks/`.
- **Method**: primary contribution is a novel algorithm, architecture, or system. Place in `Wiki/Papers/methods/`.

Use the **Benchmark Paper Page Template** or the **Method Paper Page Template** accordingly.

### Step 4 — Write or augment concept entries

For each concept from Step 2:
- Check if `Wiki/Concepts/{{concept-slug}}.md` exists
- If **no** → create it using the **Concept Entry Template** below
- If **yes** → open it, add a new section `### As used in [[paper-slug]]` with how this paper uses or extends the concept

### Step 5 — Cross-link

- In the paper page, add `[[concept-slug]]` wiki links on first mention of each concept
- In each concept entry, add the paper to the `## Papers` backlink section

---

## Benchmark Paper Page Template

File: `Wiki/Papers/benchmarks/{{paper-slug}}.md`

```markdown
# {{Full Paper Title}}

> **TL;DR**: {{One sharp sentence — what this benchmark measures and the headline finding.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue — e.g. "NeurIPS 2025", "ACL 2025 Findings", "Workshop @ ICLR 2026 (Lifelong Agent)", or "Preprint"}}

---

## Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Domain(s)** | {{e.g. recommendation, memory, education — list all}} |
| **Modality** | {{text-only / multimodal (text+image+...) — specify all modalities involved}} |
| **# Tasks** | {{number and brief names}} |
| **Scale** | {{dataset size: # users, # items, # queries, # interactions, etc.}} |
| **Data Source** | {{synthetic / real / hybrid; platform or method of collection}} |
| **Key Metrics** | {{primary evaluation metrics used}} |
| **# Models Tested** | {{how many models evaluated in the paper}} |
| **Top Model** | {{best-performing model and its headline score}} |
| **Open Data** | {{Yes/No + link if available}} |
| **Open Code** | {{Yes/No + link if available}} |

---

## Why This Benchmark Exists

{{2–3 paragraphs on the gap this benchmark fills.
What was impossible to measure before? What existing benchmarks miss?
Why does this specific evaluation matter for the field?
Karpathy voice: be direct about what was broken in prior evaluation.}}

## Prerequisites

- [[concept-A]] — {{one-line explanation of why it's needed here}}
- [[concept-B]] — {{one-line explanation}}

---

## Task Definitions

{{For each task/subtask the benchmark defines:}}

### Task 1: {{Name}}

- **Input**: {{what the model receives}}
- **Output**: {{what the model must produce}}
- **Evaluation**: {{how correctness is judged — metric + method (automatic/human/LLM-judge)}}
- **Example**: {{one concrete instance to make the task tangible}}

{{Repeat for each task. If there are many subtasks, group logically.}}

---

## Dataset Construction

### Data Composition

{{Describe the concrete makeup of the dataset:
- What does each data instance look like? (a user profile + query + ground truth? a conversation thread? a sequence of interactions?)
- What fields/columns/attributes does each instance contain?
- If multimodal: what modalities are present per instance, and how do they interact? (e.g. "each item has a product image + text description + price, but user queries are text-only")
- If text-only: what is the text structure? (natural language queries? structured logs? JSON?)
- Distribution: how balanced across domains/tasks/difficulty levels? Any notable skew?}}

### Curation Pipeline

{{Step-by-step how the data was created. Be specific:
1. **Raw source**: where did the raw data come from? (real platform logs, crowdsourcing, LLM-generated, manual expert creation, existing public datasets)
2. **Filtering**: what was removed and why? (spam, duplicates, low-quality, too-easy examples)
3. **Annotation**: who labeled what? (expert annotators, crowdworkers, LLM-as-annotator, rule-based) What was the annotation schema?
4. **Quality control**: inter-annotator agreement? validation rounds? adversarial filtering?
5. **Splits**: how are train/val/test divided? Is there data leakage risk?

If the paper uses multiple data sources or a multi-stage pipeline, describe each stage separately.
If synthetic: what LLM generated it? What prompts? What post-filtering was applied?}}

---

## Evaluation Framework

{{Metrics used and why they were chosen. For each metric:
- Name and formula — **if the paper defines a metric with an explicit formula, always include it** (LaTeX or plain-text math). Unpack every term in the formula so the reader can compute it from scratch. Standard metrics (Accuracy, F1, BLEU, ROUGE) don't need formulas, but any custom or modified metric does.
- **Every variable or symbol that appears in the wiki page must be annotated on first use** — not just formula terms, but also data structure fields (e.g. T_ref, T₊, T₋ in a triplet), scoring rubric levels (what does score=1 vs 4 mean?), and task-specific notation. The reader should never encounter an unexplained symbol.
- What it captures that alternatives miss
- Known failure modes or blind spots

Example:
> MR (Misapplication Rate) = |{p ∈ P_suppress : applied(p)}| / |P_suppress|
> - P_suppress: the set of preferences that should be suppressed in this context
> - applied(p): whether preference p appeared in the generated text (judged by LLM-as-Judge)

If the benchmark uses LLM-as-judge, describe the rubric (what each score level means) and any inter-rater agreement stats.
If the benchmark uses a non-trivial data structure (triplets, tuples, multi-field instances), annotate every field/variable in the Data Composition or Task Definitions section.}}

---

## Baseline Results

{{Model comparison table — the core deliverable of a benchmark paper.}}

| Model | {{Metric 1}} | {{Metric 2}} | {{Metric 3}} | Notes |
|-------|-------------|-------------|-------------|-------|
| {{Best model}} | **{{score}}** | **{{score}}** | {{score}} | {{brief note}} |
| {{2nd model}} | {{score}} | {{score}} | {{score}} | |
| ... | | | | |

{{After the table: 2–3 paragraphs analyzing the results.
- What patterns emerge? (size matters? reasoning helps? proprietary > open?)
- What's surprising?
- Where is the ceiling effect or floor effect?}}

---

## Key Findings

{{The benchmark's main takeaways about model capabilities. 3–5 bullet points, each with evidence:
- "Finding: X. Evidence: Y." format
- Focus on findings that generalize beyond this specific benchmark
- Flag any findings that contradict conventional wisdom}}

---

## Adoption Guide

{{Practical info for researchers who want to use this benchmark:
- How to access data and code
- Compute/cost requirements to run a full evaluation
- Common pitfalls when running experiments
- Suggested subset for quick pilot experiments
- What to pair it with (complementary benchmarks)}}

---

## What I'd Question

{{3–5 bullet points on:
- Validity concerns (does the benchmark actually measure what it claims?)
- Missing scenarios or model types not tested
- Potential for gaming or shortcut solutions
- How this benchmark might age}}

---

## Key Concepts from This Paper

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## Related Papers

- [[related-paper-slug]] — {{why it's related: complementary benchmark / method tested here / dataset overlap}}
```

---

## Method Paper Page Template

File: `Wiki/Papers/methods/{{paper-slug}}.md`

```markdown
# {{Full Paper Title}}

> **TL;DR**: {{One sharp sentence — the core claim, not a description of structure.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue — e.g. "ICML 2025", "Workshop @ NeurIPS 2025 (MemAgent)", or "Preprint"}}

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
> The core update rule is:  h_t = tanh(W_hh · h_{t-1} + W_xh · x_t)
> - h_t: the new hidden state (what the network "knows" at step t)
> - h_{t-1}: what it knew last step
> - x_t: the new input
> - W_hh and W_xh: learned weight matrices
> - tanh: squishes values to [-1, 1] so things don't explode

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
> Attention(Q, K, V) = softmax(QK^T / √d_k) V

- **Q** (Query): {{what it represents, shape, intuition}}
- **K** (Key): {{what it represents, intuition}}
- **V** (Value): {{what it represents, intuition}}
- **√d_k**: {{why we divide — prevents dot products from growing too large}}

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
```

---

## Common Issues

- **Reading only the abstract**: Always read the method section. Abstracts lie by omission. If no PDF is available, skip the wiki entry entirely — do not generate from the abstract.
- **Language drift**: Pick one language (`lang`) and write all prose consistently in that language for the entire batch. Do not switch mid-entry.
- **Granularity mismatch**: Don't create concept entries for things like "Adam optimizer" unless the paper makes a novel contribution to it. Prefer meaningful depth over breadth.
- **Duplicate slugs**: "self-attention" and "self attention" are the same. Always normalize.
- **Augmenting blindly**: When updating an existing concept entry, re-read it first. Don't add contradictory information without flagging the tension explicitly.
