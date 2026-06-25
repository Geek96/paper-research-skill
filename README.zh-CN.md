<p align="center">
  <img src="https://img.shields.io/badge/Claude_Code-Skill-7C3AED?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IndoaXRlIiBzdHJva2Utd2lkdGg9IjIiPjxwYXRoIGQ9Ik0xMiAyMGgxYTEgMSAwIDAgMCAxLTFWM2ExIDEgMCAwIDAtMS0xSDZhMSAxIDAgMCAwLTEgMXYyIi8+PHBhdGggZD0iTTEyIDRWMmExIDEgMCAwIDEgMS0xaDRhMSAxIDAgMCAxIDEgMXY0Ii8+PC9zdmc+" alt="Claude Code Skill"/>
  <img src="https://img.shields.io/github/v/release/Geek96/paper-research-skill?style=for-the-badge&color=10B981" alt="Latest Release"/>
  <img src="https://img.shields.io/github/license/Geek96/paper-research-skill?style=for-the-badge&color=6B7280" alt="MIT License"/>
</p>

<h1 align="center">📚 paper-research-skill</h1>

<p align="center">
  <strong>模块化的 Claude Code 技能包 —— 一站式学术论文研究流水线</strong>
  <br/>
  <code>搜索 / 下载 → Zotero 文献管理 → Obsidian 知识库</code>
</p>

<p align="center">
  <a href="README.md">English</a> | <strong>简体中文</strong>
</p>

---

## ✨ 功能亮点

- **多源搜索** — 同时检索 arXiv、PubMed、Semantic Scholar、bioRxiv、Google Scholar
- **Zotero 集成** — 通过 DOI 或 PDF 导入，自动去重
- **Karpathy 风格 Wiki** — 深度论文拆解 + 直觉优先的概念词条
- **6 种论文模板** — Benchmark、Method、Analysis、Framework、Survey、Technical Report
- **LaTeX 公式渲染** — Obsidian 中 MathJax 兼容的数学公式
- **Dataview 就绪** — 结构化 frontmatter，支持查询、仪表盘和 MOC（Map of Content）
- **多语言输出** — 支持英文、中文、中英混合三种模式

---

## 🚀 快速开始

```bash
npx skills add Geek96/paper-research-skill
```

然后在 Claude Code 中直接说：

```
> 帮我搜一下最近两年 RAG for code generation 的论文
> 把这 5 个 DOI 导入 Zotero 并生成 wiki 词条
> 帮我总结这篇 PDF，写成 Obsidian 笔记
```

---

## 🧩 技能一览

```
┌─────────────────────────────────────────────────────────────┐
│                    paper-research                            │
│                  （全流程编排器）                              │
│                                                             │
│  用户输入（关键词 / DOI / PDF 路径）                          │
│         │                                                   │
│         ▼                                                   │
│  ┌─────────────┐    ┌──────────────┐                        │
│  │ paper-fetch  │───▶│ paper-zotero │                        │
│  │  搜索 & 下载  │    │  导入 Zotero  │                        │
│  └─────────────┘    └──────┬───────┘                        │
│                            │                                │
│                            ▼                                │
│                    ┌────────────┐                            │
│                    │ paper-wiki │                            │
│                    │  深度 Wiki  │                            │
│                    │ + 概念词条  │                            │
│                    │ + MOC 地图  │                            │
│                    │ + Dataview │                            │
│                    └────────────┘                            │
└─────────────────────────────────────────────────────────────┘
```

| 技能 | 功能 | 触发示例 |
|------|------|---------|
| **`paper-research`** | 全流程编排器 — 从这里开始 | "帮我调研 X 方向的论文" |
| `paper-fetch` | 搜索论文 & 下载 PDF | "搜一下 X 相关的论文" / 直接给 DOI |
| `paper-zotero` | 导入 Zotero（自动去重） | "把这些论文导入 Zotero" |
| `paper-wiki` | 深度 Wiki + 概念词条 + MOC + Dataview | "加到我的 wiki 里" |
| `paper-version` | 查看当前安装版本 | `/version` |

---

## 📝 Wiki 论文分类

`paper-wiki` 将论文分为 6 种类型，每种有专属模板：

| 类型 | 目录 | 适用场景 | 核心结构 |
|------|------|---------|---------|
| **Benchmark** | `Papers/benchmarks/` | 数据集、评测套件（eval suite）、排行榜 | Task Definitions → Dataset Construction → Baseline Results |
| **Method** | `Papers/methods/` | 提出新算法、新架构 | The Method → Key Innovation → Experiments |
| **Analysis** | `Papers/analyses/` | 实证研究、消融实验（ablation）、负面结果 | Study Design → Key Findings → Core Insight |
| **Framework** | `Papers/frameworks/` | 概念框架、立场论文（position paper） | Core Argument → Dimensions → Validation |
| **Survey** | `Papers/surveys/` | 综述、分类体系（taxonomy） | Taxonomy → Coverage Analysis → Open Problems |
| **Tech Report** | `Papers/technical-reports/` | 基础模型、系统论文 | Architecture → Training → Capability Analysis |

所有模板均包含：
- 📋 **Quick Reference 速查表** — 一眼掌握论文关键信息
- Obsidian Callout 提示框 — `[!abstract]` / `[!success]` / `[!warning]` / `[!info]`
- LaTeX 公式 — `$$...$$` 块级公式 + `$...$` 行内公式
- 结构化 frontmatter — 支持 Dataview 查询
- `[[wiki-links]]` — 概念词条间的双向链接

---

## 🔧 前置依赖安装

本技能包依赖 3 个 MCP Server（Model Context Protocol 服务），需逐一安装。

### 1. Paper Search MCP — 论文搜索与下载

> 提供：arXiv、PubMed、Semantic Scholar、bioRxiv 搜索 + PDF 下载

**Smithery 一键安装：**

```bash
npx -y @smithery/cli install @openags/paper-search-mcp --client claude
```

<details>
<summary><strong>手动安装（Smithery 失败时备用）</strong></summary>

1. 克隆仓库：
   ```bash
   git clone https://github.com/openags/paper-search-mcp.git
   cd paper-search-mcp
   ```

2. 安装依赖：
   ```bash
   npm install
   ```

3. 构建：
   ```bash
   npm run build
   ```

4. 添加到 Claude Code 的 MCP 配置文件（`~/.claude/settings.json` → `mcpServers`）：
   ```json
   {
     "paper-search-mcp": {
       "command": "node",
       "args": ["/你的实际路径/paper-search-mcp/dist/index.js"]
     }
   }
   ```

</details>

**验证**：在 Claude Code 中输入 `Search arXiv for "transformer attention"`，能返回搜索结果即安装成功。

---

### 2. Zotero MCP — 文献管理

> 提供：通过 DOI 导入论文、管理 Collection（分组）、搜索文献库、注入引用

**第 1 步 — 获取 Zotero API 凭据：**

1. 打开 [zotero.org/settings/keys](https://www.zotero.org/settings/keys)
2. 点击 **Create new private key**
3. 填写名称（如 "Claude Code"）
4. 在 **Personal Library** 下勾选：
   - ✅ Allow library access（允许访问文献库）
   - ✅ Allow write access（允许写入）
5. 点击 **Save Key**，复制生成的 API Key

6. 获取 **User ID**：回到 [zotero.org/settings/keys](https://www.zotero.org/settings/keys) 页面顶部，可以看到你的 User ID（一串数字，如 `12345678`）

**第 2 步 — 安装 MCP Server：**

```bash
npx -y @smithery/cli install @Xevos117/mcp-zotero --client claude
```

安装过程中会提示输入：
- `ZOTERO_API_KEY`：第 1 步中复制的 API Key
- `ZOTERO_USER_ID`：第 1 步中获取的 User ID

<details>
<summary><strong>手动安装</strong></summary>

1. 克隆并构建：
   ```bash
   git clone https://github.com/Xevos117/mcp-zotero.git
   cd mcp-zotero
   npm install && npm run build
   ```

2. 添加到 MCP 配置：
   ```json
   {
     "mcp-zotero": {
       "command": "node",
       "args": ["/你的实际路径/mcp-zotero/dist/index.js"],
       "env": {
         "ZOTERO_API_KEY": "你的_api_key",
         "ZOTERO_USER_ID": "你的_user_id"
       }
     }
   }
   ```

</details>

**验证**：在 Claude Code 中输入 `Search my Zotero library for "attention"`，能查询到你的文献库即安装成功。

---

### 3. Obsidian MCP — 知识库读写

> 提供：读写 Obsidian Vault（知识库）中的文件

需要安装两部分：Obsidian 插件 + MCP Server 桥接。

**第 1 步 — 安装 Obsidian 插件：**

1. 打开 Obsidian → 设置 → 第三方插件 → 浏览
2. 搜索 **Local REST API**
3. 安装并启用
4. 在插件设置中：
   - 记下 **API 端口号**（默认 `27123`）
   - 复制 **API Key**（点击 "Copy API key"）
   - HTTPS 可选开启

**第 2 步 — 安装 MCP Server：**

```bash
npx -y @smithery/cli install @MarkusPfundstein/mcp-obsidian --client claude
```

安装时输入第 1 步复制的 API Key。

<details>
<summary><strong>手动安装</strong></summary>

1. 克隆并构建：
   ```bash
   git clone https://github.com/MarkusPfundstein/mcp-obsidian.git
   cd mcp-obsidian
   npm install && npm run build
   ```

2. 添加到 MCP 配置：
   ```json
   {
     "mcp-obsidian": {
       "command": "node",
       "args": ["/你的实际路径/mcp-obsidian/dist/index.js"],
       "env": {
         "OBSIDIAN_API_KEY": "你的_api_key",
         "OBSIDIAN_API_PORT": "27123"
       }
     }
   }
   ```

</details>

> [!NOTE]
> **使用时必须保持 Obsidian 打开**，且 Local REST API 插件处于运行状态。如果 MCP 不可用，技能会自动降级为直接写入本地文件系统。

**验证**：在 Claude Code 中输入 `List files in my Obsidian vault`，能列出你的 Vault 文件即安装成功。

---

### 安装检查清单

```
☐ paper-search-mcp  ← npx 安装（无需凭据）
☐ mcp-zotero         ← npx 安装 + Zotero API Key + User ID
☐ mcp-obsidian       ← npx 安装 + Obsidian Local REST API 插件 + API Key
```

---

## 📖 使用示例

### 从零调研一个新方向

```
> 帮我调研最近两年 personalized LLM agent 的论文
```

编排器会自动：
1. 搜索 arXiv / Semantic Scholar → 展示候选论文列表
2. 你选择感兴趣的论文 → 自动下载 PDF
3. 导入 Zotero（自动去重）
4. 询问你想要的输出格式 → Wiki / 笔记 / MOC

### 批量导入指定论文

```
> 导入这些论文并生成 wiki 词条：
> - 2405.12345
> - 2406.67890
> - 10.1234/example.doi
```

### 给单篇论文写 Wiki

```
> 把这篇论文加到我的 wiki 里：/path/to/paper.pdf
```

### 生成主题总览

```
> 帮我生成一个 personalization 方向的 Map of Content
```

---

## 👀 示例展示

> [`demo/`](demo/) 目录中的所有文件都是 `paper-wiki` 的**真实输出**，不是手工制作的样例。
> 以下截图直接取自 Obsidian，展示 callout、LaTeX 公式和 wiki-link 的实际渲染效果。

### Method 示例 — [Attention Is All You Need](demo/methods/attention-is-all-you-need.md)

开启 Transformer 时代的论文。

**Dataview frontmatter + `[!abstract]` TL;DR 提示框：**

<img src="demo/screenshots/method-frontmatter-tldr.png" width="700" alt="Obsidian 中渲染的 frontmatter 属性和 TL;DR 提示框"/>

**`[[wiki-links]]` 概念词条双向链接（紫色链接）：**

<img src="demo/screenshots/method-wikilinks.png" width="700" alt="紫色 wiki-link 内链"/>

**LaTeX 公式渲染 — Attention & Multi-Head：**

<img src="demo/screenshots/method-formulas.png" width="700" alt="Scaled dot-product attention 和 multi-head attention 公式"/>

**更多公式 — FFN & Positional Encoding（sin/cos）：**

<img src="demo/screenshots/method-formulas-pe.png" width="700" alt="FFN 和 positional encoding 公式"/>

**`[!success]` 提示框 — Key Innovation（绿色）：**

<img src="demo/screenshots/method-callout-success.png" width="700" alt="绿色 success 提示框"/>

**`[!warning]` 提示框 — Critical Assessment（橙色）+ Key Concepts wiki-links：**

<img src="demo/screenshots/method-callout-warning.png" width="700" alt="橙色 warning 提示框和概念反向链接"/>

### Benchmark 示例 — [PersonaLens](demo/benchmarks/personalens.md)

ACL 2025 Findings — 面向 20 个领域的个性化对话评测。

**Dataview frontmatter + TL;DR 提示框：**

<img src="demo/screenshots/bench-frontmatter-tldr.png" width="700" alt="PersonaLens frontmatter 和 TL;DR 提示框"/>

**Quick Reference 速查表（10 个维度）：**

<img src="demo/screenshots/bench-quickref-table.png" width="700" alt="Quick Reference 表格和 wiki-links"/>

**Task Definitions + `[!example]` 提示框：**

<img src="demo/screenshots/bench-task-definitions.png" width="700" alt="任务定义和示例提示框"/>

**Evaluation Framework + `[!info]` Personalization Rubric + Baseline Results 表格：**

<img src="demo/screenshots/bench-evaluation-rubric.png" width="700" alt="评分量表和基线结果"/>

**`[!success]` Key Findings（绿色）：**

<img src="demo/screenshots/bench-baseline-results.png" width="700" alt="基线结果和关键发现"/>

**`[!tip]` Adoption Guide + `[!warning]` Critical Assessment + Key Concepts wiki-links：**

<img src="demo/screenshots/bench-adoption-guide.png" width="700" alt="使用指南和批判性评估"/>

### Concept 概念词条示例

从 Method 论文中链接出来 — 展示 Karpathy 风格的直觉优先写法：

| 词条 | 亮点 |
|------|------|
| [scaled-dot-product-attention.md](demo/concepts/scaled-dot-product-attention.md) | 核心公式 + "为什么要除以 √d_k" 的解释 |
| [multi-head-attention.md](demo/concepts/multi-head-attention.md) | 矩阵维度标注（`$W_i^Q \in \mathbb{R}^{d \times d_k}$`） |
| [positional-encoding.md](demo/concepts/positional-encoding.md) | sin/cos 公式 + RoPE / ALiBi 等变体对比 |

---

## 📁 项目结构

```
paper-research-skill/
├── skills/
│   ├── paper-research/SKILL.md    # 全流程编排器
│   ├── paper-fetch/SKILL.md       # 搜索与下载
│   ├── paper-zotero/SKILL.md      # Zotero 导入
│   ├── paper-wiki/
│   │   ├── SKILL.md               # 深度 Wiki（流程 + 视觉设计系统）
│   │   └── templates/             # 按需加载模板（节省约 60% tokens）
│   │       ├── benchmark.md
│   │       ├── method.md
│   │       ├── survey.md
│   │       ├── analysis.md
│   │       ├── framework.md
│   │       ├── technical-report.md
│   │       ├── concept.md
│   │       └── moc.md
│   └── paper-version/SKILL.md     # 版本查询
├── demo/                          # 真实 Wiki 输出示例
│   ├── methods/                   #   Method 论文示例
│   ├── benchmarks/                #   Benchmark 论文示例
│   ├── concepts/                  #   概念词条示例
│   └── screenshots/               #   Obsidian 渲染截图
├── .claude-plugin/plugin.json     # Claude Code 插件清单
├── CHANGELOG.md
├── CLAUDE.md                      # 开发规范
├── README.md                      # English
└── README.zh-CN.md                # 简体中文（本文件）
```

---

## 📄 许可证

MIT © [Geek96](https://github.com/Geek96)
