# LLM Wiki Schema

> This file defines the operating rules for your LLM Wiki.
> Place it at the root of your wiki directory (e.g., `/path/to/vault/LLM Wiki/SCHEMA.md`).

## Configuration

```yaml
vault_path: /Users/your-name/Obsidian/YourVault/LLM Wiki
owner: your-name
language: English  # or Korean, etc.
domain: Software Architecture  # your focus area
```

## Directory Structure

```
LLM Wiki/
├── SCHEMA.md           ← This file (operating rules)
├── raw/                ← Immutable sources (YOU write, LLM reads)
│   ├── articles/       Blog posts, papers, web articles
│   ├── docs/           Documentation excerpts
│   └── projects/       Post-mortems, project learnings
└── wiki/               ← LLM-generated (LLM writes, YOU read)
    ├── index.md        Master catalog
    ├── log.md          Activity timeline
    ├── overview.md     High-level synthesis
    ├── sources/        Per-source summaries
    ├── concepts/       Patterns, principles, mental models
    ├── entities/       Services, tools, people, companies
    ├── decisions/      Architecture Decision Records
    ├── pitfalls/       Bugs/issues + solutions
    ├── syntheses/      Cross-cutting analyses
    └── golden/         Best code/project structure refs
```

## Conventions

### Naming
- All filenames: `kebab-case.md`
- Source summaries: `summary-{slug}.md`
- No dates in filenames (except lint reports)

### Frontmatter
Every wiki page MUST have YAML frontmatter with:
- `title`, `type`, `created`, `updated`, `sources`, `related`, `tags`, `confidence`

### Cross-References
- Use `[[wiki-links]]` (Obsidian-compatible)
- Aim for 3-10 links per page
- No orphan pages

### Citations
- Every claim must cite a specific `raw/` file
- Format: `sources:` in frontmatter or inline `[[wiki-link]]`

## Domain-Specific Rules

<!-- Add your own rules here as your wiki evolves -->
<!-- Examples: -->
<!-- - "Always include cost comparison for service decisions" -->
<!-- - "Tag all pages related to Project X with #project-x" -->
<!-- - "Korean pages should use formal style (합니다체)" -->

## Custom Page Types

<!-- Define additional page types here if needed -->
<!-- Example: -->
<!-- - type: benchmark — for performance test results -->
<!-- - type: comparison — for A vs B analyses -->
