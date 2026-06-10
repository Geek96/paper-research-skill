# CLAUDE.md — Contribution Rules for paper-research-skill

This file tells Claude (and human contributors) how to maintain this repository consistently.

---

## Versioning

Version format: `MAJOR.MINOR.PATCH` — stored in `.claude-plugin/plugin.json` as the `version` field. This is the single source of truth.

| Component | Rule | Who controls |
|-----------|------|--------------|
| **PATCH** | Increment by 1 on every commit that changes a skill, template, or user-facing content | Claude (automatic) |
| **MINOR** | Auto-increment when PATCH reaches **10**, then reset PATCH to 0 | Claude (automatic carry) |
| **MAJOR** | Only when user or a contributor explicitly says "bump major" or "this is a breaking change" | Human only |

**Step-by-step for every commit:**

1. Identify the current version in `.claude-plugin/plugin.json`
2. If the commit changes any `SKILL.md`, `plugin.json` (non-version fields), `README.md`, or `CHANGELOG.md` → increment PATCH
3. If new PATCH = 10 → set PATCH = 0, increment MINOR instead
4. If MAJOR change is explicitly requested → increment MAJOR, reset MINOR and PATCH to 0
5. Update the `version` field in `.claude-plugin/plugin.json` in the same commit
6. Skip version bump for: `.gitignore`, `LICENSE`, inline comments with zero behavior change

**Examples:**
- `1.0.3` → next patch → `1.0.4`
- `1.0.9` → next patch → `1.1.0` (carry)
- `1.1.9` → next patch → `1.2.0` (carry)
- User says "bump major" → `1.2.4` → `2.0.0`

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

---

## Skill File Rules

- Every skill lives in `skills/<skill-name>/SKILL.md`
- Every skill must be listed in `.claude-plugin/plugin.json` under `skills`
- Skill names use kebab-case: `paper-wiki`, `paper-version`
- All mathematical formulas in skill templates use GitHub LaTeX syntax (`$...$` inline, `$$...$$` display)
