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
| 🗺️ | Taxonomy / Framework (surveys) |
| 🏗️ | Architecture (technical reports) |

### Tag Taxonomy

Frontmatter `tags` use a hierarchical system:

```yaml
tags:
  - type/benchmark          # type/method, type/survey, type/technical-report, type/concept
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

---

## Benchmark Paper Page Template

File: `Wiki/Papers/benchmarks/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: benchmark
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
domains: [{{domain1}}, {{domain2}}]
modality: {{text-only / multimodal}}
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/benchmark
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — what this benchmark measures and the headline finding.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue — e.g. "NeurIPS 2025", "ACL 2025 Findings", "Workshop @ ICLR 2026 (Lifelong Agent)", or "Preprint"}}

---

## 📋 Quick Reference

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

## 🎯 Why This Benchmark Exists

{{2–3 paragraphs on the gap this benchmark fills.
What was impossible to measure before? What existing benchmarks miss?
Why does this specific evaluation matter for the field?
Karpathy voice: be direct about what was broken in prior evaluation.}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line explanation of why it's needed here}}
> - [[concept-B]] — {{one-line explanation}}

---

## 🔬 Task Definitions

{{For each task/subtask the benchmark defines:}}

### Task 1: {{Name}}

- **Input**: {{what the model receives}}
- **Output**: {{what the model must produce}}
- **Evaluation**: {{how correctness is judged — metric + method (automatic/human/LLM-judge)}}

> [!example] Example
> {{one concrete instance to make the task tangible}}

{{Repeat for each task. If there are many subtasks, group logically.}}

---

## 📦 Dataset Construction

### Data Composition

{{Describe the concrete makeup of the dataset:
- What does each data instance look like? (a user profile + query + ground truth? a conversation thread? a sequence of interactions?)
- What fields/columns/attributes does each instance contain?
- If multimodal: what modalities are present per instance, and how do they interact?
- If text-only: what is the text structure?
- Distribution: how balanced across domains/tasks/difficulty levels? Any notable skew?}}

### Curation Pipeline

{{Step-by-step how the data was created. Be specific:
1. **Raw source**: where did the raw data come from?
2. **Filtering**: what was removed and why?
3. **Annotation**: who labeled what? What was the annotation schema?
4. **Quality control**: inter-annotator agreement? validation rounds?
5. **Splits**: how are train/val/test divided? Is there data leakage risk?

If synthetic: what LLM generated it? What prompts? What post-filtering was applied?}}

---

## 📐 Evaluation Framework

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

## 📊 Baseline Results

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

## 💡 Key Findings

> [!success] Main Takeaways
> {{The benchmark's main takeaways about model capabilities. 3–5 bullet points, each with evidence:
> - **Finding**: X. **Evidence**: Y.
> - Focus on findings that generalize beyond this specific benchmark
> - Flag any findings that contradict conventional wisdom}}

---

## 🛠️ Adoption Guide

> [!tip] Getting Started
> - **Data/Code**: {{how to access}}
> - **Compute**: {{cost requirements}}
> - **Quick Pilot**: {{suggested subset for fast experiments}}
> - **Pair With**: {{complementary benchmarks}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{3–5 bullet points on:
> - Validity concerns (does the benchmark actually measure what it claims?)
> - Missing scenarios or model types not tested
> - Potential for gaming or shortcut solutions
> - How this benchmark might age}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related: complementary benchmark / method tested here / dataset overlap}}
```

---

## Method Paper Page Template

File: `Wiki/Papers/methods/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: method
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
domains: [{{domain1}}, {{domain2}}]
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/method
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — the core claim, not a description of structure.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Core Contribution** | {{one-line: what this paper contributes — e.g. "RL-based preference inference for personalization"}} |
| **Method Type** | {{architecture / training pipeline / inference-time / framework / system}} |
| **Base Model(s)** | {{what models were used — e.g. "Qwen3-8B", "Llama-3-8B", or "model-agnostic"}} |
| **Key Result** | {{the headline number — e.g. "87.5% preference accuracy (+15.7pp over best baseline)"}} |
| **Key Ablation** | {{the most revealing ablation — e.g. "RL vs SFT: 69.1% vs 47.9% (+21pp)"}} |
| **Open Code** | {{Yes/No + link if available}} |

---

## 🎯 Why This Paper Exists

{{2–3 paragraphs on the problem this paper solves. Write in plain language.
What was broken or missing before? What makes this hard?
What would happen if you didn't have this paper's contribution?
Karpathy voice: be direct, use concrete examples, avoid hedging.}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line explanation of why it's needed here}}
> - [[concept-B]] — {{one-line explanation}}
> - [[concept-C]] — {{one-line explanation}}
>
> If these are unfamiliar, read the concept entries first.

---

## 🔬 The Method

### Background / Prior Work

{{What are the 2–3 key papers this builds on? What does this paper need from them?
Don't just list citations — explain the dependency.
Example: "This paper needs [[DTW]] because the core metric is built on top of it."}}

### How It Actually Works

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

Repeat for each component. Include ASCII diagrams if they aid clarity.
If there are multiple sub-modules, use ### sub-headers for each.}}

---

## 💡 The Key Innovation

> [!success] What's Genuinely New
> {{In 1–2 paragraphs: what is genuinely new in this paper?
> Be specific and critical. Examples:
> - "The novel part is X. Prior work did Y instead, which caused problem Z."
> - "This looks like technique A from [prior paper], but differs in that..."
> Karpathy style: distinguish between what's actually new and what's packaging.}}

---

## 📊 Experiments

{{What did they test? On what data? Against what baselines?}}

| Setting / Dataset | Metric | This Paper | Best Baseline | Δ | Notes |
|-------------------|--------|------------|---------------|---|-------|
| {{dataset-1}} | {{metric}} | **{{score}}** | {{baseline score}} | {{+X.X}} | {{brief}} |
| {{dataset-2}} | {{metric}} | **{{score}}** | {{baseline score}} | {{+X.X}} | |

{{After the table: 2–3 paragraphs analyzing the results.
- What patterns emerge?
- How big is the improvement? Is it meaningful in practice?
- What ablation is most revealing?
- What did they NOT test that you'd want to see?

Be concrete: "They report a 3.2% improvement on benchmark X. Baseline Y was strong.
They did not test on out-of-distribution data, which would matter most for deployment."}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{3–5 bullet points combining questions AND limitations:
> - What experiment would you run next?
> - What assumption seems shakiest?
> - What does the method fail on? What assumptions does it make?
> - When would you NOT use this approach?
> - What does this suggest for your own work?
>
> Be specific. "I would try X because Y" not "future work should explore X."
> Include the authors' stated limitations + any they missed.}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related: shares method / problem / dataset}}
```

---

## Survey Paper Page Template

File: `Wiki/Papers/surveys/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: survey
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
field: "{{research field}}"
papers_reviewed: {{approximate count}}
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/survey
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — what field this surveys, and the key insight or gap it identifies.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Field** | {{what research area this surveys}} |
| **Scope** | {{time range, # papers reviewed, inclusion/exclusion criteria}} |
| **Taxonomy** | {{how the paper organizes the field — axes/dimensions of classification}} |
| **Key Gap Identified** | {{the most important open problem or blind spot the survey highlights}} |
| **# Papers Reviewed** | {{approximate count}} |

---

## 🎯 Why This Survey Exists

{{2–3 paragraphs. What makes this field hard to navigate? Why is a survey needed now?
What prior surveys exist and what do they miss?}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line}}
> - [[concept-B]] — {{one-line}}

---

## 🗺️ Taxonomy / Organizational Framework

{{The core intellectual contribution of a survey — how it carves the field into categories.
Describe each axis of the taxonomy with definitions and representative papers.

Example format:
### Axis 1: {{Name}}
- **Category A**: {{definition}}. Representative: [[paper-1]], [[paper-2]]
- **Category B**: {{definition}}. Representative: [[paper-3]]

### Axis 2: {{Name}}
...

If the survey proposes a multi-dimensional framework (e.g. 2×3 grid), describe both axes and the key cells.
All category names and axis labels must be annotated with definitions.}}

---

## 📊 Coverage Analysis

{{What's well-covered vs. under-explored in the field?
- Which problem formulations dominate? Which are neglected?
- Which methods/approaches are over-represented?
- What datasets or domains are missing?
- Any methodological blind spots (e.g. everyone uses the same evaluation)?}}

---

## 💡 Key Findings / Trends

> [!success] Main Takeaways
> {{3–5 bullet points on the survey's main conclusions about the state of the field:
> - **Finding**: {{statement}}. **Evidence**: {{which reviewed papers support this}}
> - Focus on findings that change how you'd approach work in this area}}

---

## Open Problems

{{The survey's identified gaps and open challenges, ranked by importance.
For each:
- What's missing?
- Why is it hard?
- What would a solution look like?}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{Your critical take:
> - Is the taxonomy actually useful, or does it impose artificial categories?
> - What papers or approaches did the survey miss?
> - Are the "open problems" genuinely open, or already being addressed?
> - Bias in coverage (e.g. over-represents certain labs/methods)?}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related}}
```

---

## Technical Report Paper Page Template

File: `Wiki/Papers/technical-reports/{{paper-slug}}.md`

```markdown
---
title: "{{Full Paper Title}}"
aliases: [{{short-name}}]
type: technical-report
year: {{year}}
venue: "{{venue}}"
arxiv: "{{arxiv_id}}"
model_type: {{foundation model / architecture / system / infrastructure}}
scale: "{{parameter count or key scale metric}}"
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/technical-report
  - domain/{{primary-domain}}
  - venue/{{venue-slug}}
  - status/complete
---

# {{Full Paper Title}}

> [!abstract] TL;DR
> {{One sharp sentence — what this system/model is and its core design bet.}}

**Authors**: {{authors}} | **Year**: {{year}} | **arXiv**: [{{arxiv_id}}](https://arxiv.org/abs/{{arxiv_id}}) | **Zotero**: [Open](zotero://select/items/{{zotero_key}})
**Venue**: {{venue}}

---

## 📋 Quick Reference

| Dimension | Detail |
|-----------|--------|
| **Type** | {{foundation model / architecture / system / infrastructure}} |
| **Modality** | {{text / vision / multimodal / etc.}} |
| **Scale** | {{parameter count, training data size, compute budget if known}} |
| **Core Design Bet** | {{the one architectural or design decision that defines this work}} |
| **Downstream Impact** | {{what this enabled — list major follow-up works or applications}} |
| **Open Source** | {{Yes/No; link if available}} |

---

## 🎯 Why This Paper Matters

{{2–3 paragraphs. Not "why this paper exists" (it exists because a lab built something) but why it matters:
- What did the field look like before this?
- What shifted after? What became possible?
- Is its importance from the architecture, the scale, the training recipe, or something else?
Karpathy voice: be honest about what's genuinely novel vs. well-executed engineering.}}

> [!info] Prerequisites
> - [[concept-A]] — {{one-line}}
> - [[concept-B]] — {{one-line}}

---

## 🏗️ Architecture

{{The core technical content. Walk through the architecture or system design:

### High-Level Design
{{Block diagram description — what are the major components and how do they connect?}}

### Key Components
{{For each non-trivial component:
- What it does (one sentence)
- How it works (mechanistic)
- Key equations with every term annotated
- Why this design choice over alternatives}}

### Training Recipe
{{If applicable:
- Training data: sources, scale, curation
- Training procedure: stages, hyperparameters, curriculum
- Key tricks that matter for reproduction}}

### Design Decisions

{{The most interesting part — WHY choices were made:
- "They chose X over Y because Z"
- "This is unusual because most prior work does W instead"
- Trade-offs: what did this design choice sacrifice?}}

---

## 📊 Scaling & Performance

{{Results framed as "what can this system do":
- Key benchmarks and scores
- Comparison with contemporaries (not just prior work — what shipped around the same time?)
- Scaling laws or emergent capabilities if discussed}}

---

## Legacy & Influence

{{What happened after this paper:
- What follow-up works built directly on this?
- Which design decisions became standard? Which were abandoned?
- Did the paper's own framing hold up, or did the field reinterpret its contribution?
This section is unique to technical reports — benchmark/method papers don't need it because they're too recent.}}

---

## ⚠️ What I'd Question

> [!warning] Critical Assessment
> {{Critical assessment:
> - What limitations are underplayed in the paper?
> - What would break at larger/smaller scale?
> - Which claims haven't been independently verified?
> - What's missing from the evaluation?}}

---

## 🧩 Key Concepts

- [[concept-a]] — {{one-line}}
- [[concept-b]] — {{one-line}}

---

## 🔗 Related Papers

- [[related-paper-slug]] — {{why it's related: predecessor / successor / alternative design}}
```

---

## Concept Entry Template

File: `Wiki/Concepts/{{concept-slug}}.md`

```markdown
---
title: "{{Concept Name}}"
aliases: [{{alternative names}}]
type: concept
category: {{method / evaluation / architecture / phenomenon / framework}}
first_introduced: "[[{{paper-slug}}]]"
status: complete
created: {{YYYY-MM-DD}}
tags:
  - type/concept
  - domain/{{primary-domain}}
  - status/complete
---

# {{Concept Name}}

> [!abstract] In One Sentence
> {{The sharpest, most concrete definition. Avoid "is a method that" — describe what it does.}}

---

## Intuition First

{{2–3 paragraphs explaining the concept from scratch, as if to someone who hasn't seen it.
Use an analogy or a toy example. Make it concrete.
Karpathy rule: if you can't explain it simply, you don't understand it yet.}}

## The Formal Version

{{Now give the precise definition. Include the key equation(s).
After each equation, unpack every term:}}

> {{Key equation}}

- **Term 1**: {{what it represents, shape, intuition}}
- **Term 2**: {{what it represents, intuition}}

## How to Think About It (Mental Model)

{{A single concrete mental model or analogy that makes the concept stick.
This should be different from the intuition paragraph — more specific, more memorable.}}

## Common Misconceptions

> [!danger] Watch Out
> {{2–4 bullet points on things people get wrong about this concept.}}

## Variants and Extensions

{{List known variants with one-line descriptions. Link to concept entries if they exist.}}

## When to Use It (and When Not To)

> [!tip] Practical Guidance
> - **Works well when**: ...
> - **Breaks down when**: ...
> - **Prefer X instead when**: ...

---

## Papers

Papers that introduce or use this concept (augmented automatically):

| Paper | How it's used |
|-------|--------------|
| [[paper-slug]] | {{one-line: introduced / extends / applies to X domain}} |
```

---

## MOC (Map of Content) Template

File: `Wiki/MOC.md`

After each batch, create or update the MOC. This is the central navigation page.

```markdown
---
title: "Map of Content"
type: moc
created: {{YYYY-MM-DD}}
updated: {{YYYY-MM-DD}}
tags:
  - type/moc
---

# 🗺️ Research Wiki — Map of Content

> [!abstract] Overview
> This wiki contains **{{N}} papers** and **{{M}} concepts** across {{K}} research domains.
> Last updated: {{YYYY-MM-DD}}

---

## 📊 By Type

### Benchmarks
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|
| [[paper-slug]] | {{domain}} | {{venue}} | {{year}} |

### Methods
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|
| [[paper-slug]] | {{domain}} | {{venue}} | {{year}} |

### Surveys
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|

### Technical Reports
| Paper | Domain | Venue | Year |
|-------|--------|-------|------|

---

## 🏷️ By Domain

### {{Domain Name}}
- [[paper-1]]
- [[paper-2]]
- Related concepts: [[concept-a]], [[concept-b]]

---

## 🧩 Concepts Index

| Concept | Category | Introduced By |
|---------|----------|---------------|
| [[concept-slug]] | {{method/evaluation/...}} | [[paper-slug]] |

---

## 📈 Dataview Queries

> [!tip] Auto-Generated Views
> The following Dataview blocks auto-update as you add papers.

### Recent Additions
\`\`\`dataview
TABLE type, venue, year
FROM "Wiki/Papers"
SORT created DESC
LIMIT 10
\`\`\`

### Papers by Domain
\`\`\`dataview
TABLE type, venue, year
FROM "Wiki/Papers"
WHERE contains(tags, "domain/personalization")
SORT year DESC
\`\`\`

### Benchmarks Only
\`\`\`dataview
TABLE domains as "Domain", venue, year
FROM "Wiki/Papers/benchmarks"
SORT year DESC
\`\`\`

### Concepts by Category
\`\`\`dataview
TABLE category, first_introduced as "First Introduced"
FROM "Wiki/Concepts"
SORT title ASC
\`\`\`
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

MOC updated: Wiki/MOC.md (+1 paper, +3 concepts)
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
1. Stage and commit the changed files in the relevant repo (e.g. `Geek96/ICLR2027` for personalization wiki, `Geek96/paper-research-skill` for skill updates)
2. Push to remote

This ensures the GitHub repos stay in sync with local changes.
