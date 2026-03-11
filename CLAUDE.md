# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **skills-creator**, a meta-skill for the OpenClaw ecosystem that teaches LLMs how to create, review, and optimize agent skills. It is a content-only project (no build system, no tests, no dependencies) — all files are Markdown.

## Two Versions

The project maintains two parallel versions:

| Version | Directory | Language | Target Platform | Pushed to Remote |
|---------|-----------|----------|----------------|-----------------|
| English (public) | Root (`/`) | English | ClawHub / GitHub | Yes |
| Chinese (internal) | `MeiGen-skills-creator/` | Chinese | Internal skill marketplace (Friday) | No (gitignored) |

Key differences:
- The Chinese version does NOT contain ClawHub-specific content (e.g., `clawhub publish`, ClawHub references)
- The Chinese version references "Friday skill广场" instead of "ClawHub"
- Description and trigger phrases are in Chinese
- Both versions share the same structure and best practices, but wording/rules differ

When editing, always consider whether the change applies to one or both versions. Never push the `MeiGen-skills-creator/` directory to remote.

## File Structure

```
skills-creator/
├── SKILL.md                           # Core skill instructions (English, ~275 lines)
├── references/
│   ├── best-practices.md              # Design guidelines with real-world examples
│   └── quality-checklist.md           # 24-point checklist + retrofit process
├── assets/
│   └── skill-template.md             # Copy-paste starter template
├── MeiGen-skills-creator/             # Chinese internal version (gitignored)
│   ├── SKILL.md                       # Core instructions (Chinese)
│   ├── references/
│   │   ├── best-practices.md
│   │   └── quality-checklist.md
│   └── assets/
│       └── skill-template.md
└── README.md                          # Project documentation (English, public)
```

## Core Concepts

- **SKILL.md** is the entry point — it gets injected into the LLM's context at runtime. `{baseDir}` is resolved by the platform to the skill's installation path.
- **Complexity Tiers**: Simple (< 150 lines, no dirs) / Medium (100-300 lines, scripts/ or references/) / Complex (200-650 lines, multiple dirs)
- **Description Writing Formula**: `[Action verb] + [value proposition]. Use when [trigger phrases] or discusses [topics].` — the single most important field for skill activation
- **24-point Quality Checklist**: 6 categories (Frontmatter, Description, Content Structure, Self-Consistency, Security, Supporting Files)
- **4 Workflow Modes**: Create / Review / Retrofit / API Integration

## ClawHub Publishing

Slug: `skills-creator`，ClawHub 页面: https://clawhub.ai/jau123/skills-creator

发布流程（避免将中文版打包上去）：

```bash
# 1. 更新 SKILL.md frontmatter 中的 version 字段

# 2. 用 rsync 导出干净的英文版到临时目录
mkdir -p /tmp/skills-creator-publish
rsync -av \
  --exclude='.git' \
  --exclude='MeiGen-skills-creator' \
  --exclude='MeiGen-skills-creator.zip' \
  --exclude='CLAUDE.md' \
  --exclude='.claude' \
  --exclude='.gitignore' \
  . /tmp/skills-creator-publish/

# 3. 从临时目录发布
clawhub publish /tmp/skills-creator-publish --slug skills-creator \
  --name "Skills Creator — Build High-Quality OpenClaw Skills" \
  --version <ver> \
  --changelog "<changelog message>"

# 4. 清理
rm -rf /tmp/skills-creator-publish
```

注意事项：
- 版本号在 SKILL.md frontmatter 的 `version:` 字段，发布前先更新
- `_meta.json` 由 ClawHub 自动生成，不要手动创建
- 不要用 `clawhub publish .`，会把中文版、CLAUDE.md 等非发布文件一起打包
- CLI v0.7.0 有 `acceptLicenseTerms` bug，需 patch `/opt/homebrew/lib/node_modules/clawhub/dist/cli/commands/publish.js`，在 publish payload 中加 `acceptLicenseTerms: true`
- 发布后同步到 `openclaw/skills` GitHub 仓库可能需要数小时

## Editing Guidelines

- SKILL.md must stay under 300 lines; overflow goes to `references/`
- Metadata in frontmatter must be single-line JSON (parser limitation)
- Use `clawdbot` key in metadata, not `openclaw`
- Follow the skill's own rules (self-consistency) — if it teaches "use tables for decision logic", its own content must use tables
- When updating content in one version, evaluate if the same change should apply to the other version
