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

## Changelog

Update `CHANGELOG.md` for every **user-facing** change. Skip internal-only changes.

**Write a changelog entry when:**
- ✅ A new skill or sub-skill is added
- ✅ Skill output format or behavior changes
- ✅ Template structure is modified
- ✅ Formatting rules are added or changed
- ✅ A bug fix that affects skill output is made
- ✅ A breaking change is introduced (mark with `⚠️ Breaking`)

**Skip a changelog entry when:**
- ❌ `.gitignore`, `LICENSE`, `CLAUDE.md` internal edits
- ❌ Typo fixes in non-template prose
- ❌ Refactors with identical output behavior

**Format:** [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) style.
Group entries under: `Added`, `Changed`, `Fixed`, `Removed`.

```markdown
## [1.0.4] — YYYY-MM-DD

### Added
- `paper-version` skill: `/version` command reports installed version

### Changed
- `paper-wiki`: formula examples now use LaTeX `$$...$$` syntax
```

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
