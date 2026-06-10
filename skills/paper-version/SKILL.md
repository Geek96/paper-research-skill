---
name: paper-version
description: Use when user runs /version, asks "what version is this?", "am I on the latest version?", or "check for updates". Reports the currently installed version of paper-research-skill.
---

# Paper Version Check

## What to Do

1. Read `.claude-plugin/plugin.json` from the skill's install directory
2. Extract the `version` field
3. Report in this exact format:

```
paper-research-skill v{version}

Changelog : https://github.com/Geek96/paper-research-skill/blob/main/CHANGELOG.md
Latest    : https://github.com/Geek96/paper-research-skill/releases

To update : npx skills update Geek96/paper-research-skill
```

## Version Number Meaning

`MAJOR.MINOR.PATCH`

| Part | Meaning |
|------|---------|
| MAJOR | Breaking changes — manual approval required |
| MINOR | Auto-incremented when PATCH hits 10 |
| PATCH | Incremented on every user-facing change |

If the user asks what changed in a specific version, refer them to the CHANGELOG link above.
