# Skills Creator

An OpenClaw skill that guides the creation, review, and optimization of agent skills following proven best practices.

## What It Does

This is a **meta-skill** — it teaches LLMs how to build high-quality skills for the OpenClaw ecosystem. Based on deep analysis of top-performing skills on ClawHub (find-skills, tavily-search, self-improving-agent, and others).

### Core Capabilities

- **Create new skills** — step-by-step workflow from requirements gathering to quality review
- **Review existing skills** — 24-point quality checklist across 6 categories
- **Retrofit old skills** — 8-step process to bring any skill up to best practices
- **Add API integrations** — patterns for wrapping external HTTP APIs with curl scripts

### Key Insights Codified

- **Description Writing Formula** — the single most important factor for reliable skill triggering
- **Complexity Tiers** (Simple / Medium / Complex) — choose the right structure for your skill's scope
- **Tables over prose** — LLMs find table rows faster than parsing paragraphs
- **LLM as execution engine** — write instructions, not code logic

## Install

```bash
clawhub install jau123/skills-creator
```

Or manually:

```bash
git clone https://github.com/jau123/skills-creator.git ~/.openclaw/skills/skills-creator
```

## File Structure

```
skills-creator/
├── SKILL.md                      # Core instructions (~275 lines)
├── references/
│   ├── best-practices.md         # Design guidelines with real-world examples
│   └── quality-checklist.md      # 24-point checklist + retrofit process
└── assets/
    └── skill-template.md         # Copy-paste starter template
```

## Usage

Once installed, the skill activates when you ask things like:

- "Create a skill for X"
- "Review my SKILL.md"
- "How do I write a good skill description?"
- "Optimize my existing skill"
- "What makes a good OpenClaw skill?"

## Quality Checklist Summary

The full checklist covers 24 checks across 6 categories:

| Category | Checks | What it covers |
|----------|--------|----------------|
| Frontmatter | 6 | name format, description quality, metadata JSON |
| Description | 4 | trigger phrases, action verbs, length |
| Content Structure | 5 | tables, workflow modes, examples |
| Self-Consistency | 3 | follows own rules, no conflicts |
| Security | 3 | VirusTotal compliance, pinned versions |
| Supporting Files | 3 | references/, scripts/, assets/ |

## License

MIT-0 (MIT No Attribution) — as required by ClawHub.
