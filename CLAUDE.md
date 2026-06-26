# CLAUDE.md — Contribution Rules for paper-research-skill

This file tells Claude (and human contributors) how to maintain this repository consistently.

---

## ⚠️ Versioning — EVERY COMMIT MUST FOLLOW

> **MANDATORY**: Agent 每次 commit 前必须执行版本检查。不执行 = 违规。
> **禁止跳版本号**：Agent 只能递增 PATCH（最后一位），绝对不能跳版本（如 1.0.5 → 1.4.0 ❌）。

Version format: `MAJOR.MINOR.PATCH` — stored in `.claude-plugin/plugin.json` as the `version` field. This is the single source of truth.

| Component | Rule | Who controls |
|-----------|------|--------------|
| **PATCH** | +1 on every commit that changes skill/template/user-facing content | Claude (automatic) |
| **MINOR** | Auto-increment when PATCH reaches **10**, reset PATCH to 0 | Claude (automatic carry) |
| **MAJOR** | Only when user explicitly says "bump major" or "breaking change" | **Human only** |

### 🔴 Pre-Commit Checklist（每次 commit 前必做）

```
□ 1. cat .claude-plugin/plugin.json → 读取当前版本号
□ 2. 本次 commit 改了 SKILL.md / templates/ / plugin.json / README.md / CHANGELOG.md？
     → YES: PATCH +1（只 +1，不跳）
     → NO:  不 bump
□ 3. PATCH = 10？→ PATCH 归零，MINOR +1
□ 4. 在同一个 commit 中更新 plugin.json 的 version
□ 5. commit message 中包含版本号
```

**跳过 bump**: `.gitignore`, `LICENSE`, 纯注释（无行为变化）

### Examples

| Before | Action | After | OK? |
|--------|--------|-------|-----|
| `1.0.3` | patch | `1.0.4` | ✅ |
| `1.0.9` | patch (carry) | `1.1.0` | ✅ |
| `1.1.9` | patch (carry) | `1.2.0` | ✅ |
| `1.2.4` | user says "bump major" | `2.0.0` | ✅ |
| `1.0.5` | agent jumps | `1.4.0` | ❌ 禁止 |
| `1.3.0` | agent bumps major | `2.0.0` | ❌ 需用户授权 |

---

## Changelog & Release

### 三层记录体系

| 层级 | 文件 | 粒度 | 何时写 |
|------|------|------|-------|
| **Git log** | git commit messages | 每次 commit | 自动 |
| **CHANGELOG.md** | `CHANGELOG.md` | 每次 MINOR 进位 | 进位时汇总该轮所有 patch |
| **GitHub Release** | GitHub Releases | 每次 MINOR 进位 | 与 CHANGELOG 同步，`gh release create` |

### CHANGELOG 规则

> **不逐 PATCH 写 CHANGELOG**。在 MINOR 进位时（如 1.3.9 → 1.4.0），回顾该 MINOR 周期内所有 commit，汇总为一个 release entry。

**写入内容（汇总时）：**
- ✅ 新增的 skill / sub-skill / template
- ✅ Skill 输出格式或行为变化
- ✅ 模板结构修改
- ✅ Bug fix（影响 skill 输出的）
- ✅ Breaking change（标记 `⚠️ Breaking`）

**跳过：**
- ❌ `.gitignore`, `LICENSE`, `CLAUDE.md` 内部编辑
- ❌ 非模板文字的 typo 修复
- ❌ 输出行为不变的重构

**Format:** [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) style.
Group entries under: `Added`, `Changed`, `Fixed`, `Removed`.

### Release 规则

在 MINOR 进位时执行：
1. 更新 `CHANGELOG.md`（汇总本轮所有 patch）
2. `git tag v{{version}}`
3. `gh release create v{{version}}` — release notes 复用 CHANGELOG 内容
4. 不在 PATCH 级别打 tag 或发 release

---

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat(skill-name): short description
fix(skill-name): short description
chore: short description
docs: short description
```

Always include the version bump in the commit that triggers it — don't make a separate "bump version" commit.
**版本号必须出现在 commit message 末尾**，格式：`(v1.3.9)`。例如：`feat(paper-wiki): add concept template (v1.3.9)`

---

## Skill File Rules

- Every skill lives in `skills/<skill-name>/SKILL.md`
- Every skill must be listed in `.claude-plugin/plugin.json` under `skills`
- Skill names use kebab-case: `paper-wiki`, `paper-version`
- All mathematical formulas in skill templates use GitHub LaTeX syntax (`$...$` inline, `$$...$$` display)
