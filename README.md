# 🧠 LLM Wiki for Quick

An [Amazon Quick](https://quick.aws.dev) skill for managing a personal LLM Wiki — a compounding knowledge base that gets smarter with every source you add.

Based on [Andrej Karpathy's LLM Wiki concept](https://x.com/karpathy/status/1937537680890122369) and inspired by [sample-kiro-llm-wiki](https://github.com/aws-samples/sample-kiro-llm-wiki).

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."
> — Andrej Karpathy

## What is an LLM Wiki?

An LLM Wiki is a **structured knowledge base designed to be consumed by LLMs**. Unlike traditional documentation or notes, an LLM Wiki:

- **Compiles** raw sources into structured, interlinked pages at ingest time (not retrieval time)
- **Compounds** — every source ingested and every query answered makes the wiki richer
- **Stores what models can't know** — your decisions, pitfalls, project-specific insights
- **Uses Obsidian** as the IDE — you get graph view, backlinks, and full-text search for free

## The 3-Layer Architecture

```
┌─────────────────────────────────────────────────┐
│  Layer 3: SCHEMA.md (operating rules)           │
├─────────────────────────────────────────────────┤
│  Layer 2: wiki/ (LLM-generated, interlinked)    │
│  ├── index.md, log.md, overview.md              │
│  ├── sources/, concepts/, entities/             │
│  ├── decisions/, pitfalls/, golden/             │
│  └── syntheses/                                 │
├─────────────────────────────────────────────────┤
│  Layer 1: raw/ (immutable source documents)     │
│  ├── articles/, docs/, projects/                │
│  └── (never modified by the LLM)               │
└─────────────────────────────────────────────────┘
```

| Layer | Who Writes | Who Reads | Purpose |
|-------|-----------|-----------|---------|
| `raw/` | You | LLM | Immutable source documents |
| `wiki/` | LLM | You + LLM | Compiled, interlinked knowledge |
| `SCHEMA.md` | You + LLM | LLM | Operating rules & conventions |

## How It Works

### 📥 Ingest — "Add this to the wiki"
Drop a source (URL, article, project learnings) → Quick reads it, creates a source summary, updates relevant concept/entity pages, flags contradictions, and logs everything.

### 🔍 Query — "What does the wiki say about X?"
Quick searches the wiki first (not general LLM knowledge), synthesizes an answer with citations, and offers to file valuable answers back as new wiki pages (the compounding loop).

### 🩺 Lint — "Check wiki health"
Quick scans for orphan pages, broken links, contradictions, stale claims, and coverage gaps. Outputs a prioritized action report.

## Installation

### Prerequisites
- [Amazon Quick](https://quick.aws.dev) installed
- An Obsidian vault (or any markdown folder)

### Setup

1. **Copy the skill file** to your Quick skills directory:
   ```bash
   cp SKILL.md ~/.quickwork/skills/llm-wiki/SKILL.md
   ```

2. **Create your wiki structure** in your Obsidian vault:
   ```bash
   mkdir -p "Your Vault/LLM Wiki"/{raw/{articles,docs,projects},wiki/{sources,concepts,entities,decisions,pitfalls,golden,syntheses}}
   ```

3. **Create SCHEMA.md** at the wiki root:
   ```markdown
   # LLM Wiki Schema

   ## Configuration
   - vault_path: /path/to/your/vault/LLM Wiki
   - owner: your-name

   ## Conventions
   - Language: English (or your preference)
   - Domain: (your focus area)
   ```

4. **Add the vault folder** to Quick's allowed folders (Settings → Folders).

5. **Start using it:**
   - "Ingest this article: [URL]"
   - "What does the wiki say about [topic]?"
   - "Run a wiki lint check"
   - "Add a decision: we chose X because Y"

## What to Put in Your Wiki

### ✅ Add This
| Type | Example |
|------|---------|
| Decisions | "Why we chose OpenSearch over Elasticsearch" |
| Pitfalls | "DynamoDB hot partition caused 5xx at 2AM" |
| Project learnings | "What worked/didn't in the Q3 migration" |
| Synthesized insights | "Comparison: Lambda vs Fargate for our use case" |
| Experience-based knowledge | "In practice, cold starts matter less than..." |
| Golden examples | "This CDK structure scaled cleanly to 20 services" |

### ❌ Don't Add This
| Type | Why Not |
|------|---------|
| API documentation | The LLM already knows this |
| Tutorials/how-tos | Generic, not personal |
| Copy-pasted docs | No value added |

**The test**: *"Could the LLM make the right decision without this?"*
- YES → don't add it
- NO → add it

## Key Principles

1. **Wiki > RAG** — Compile at ingest time, not retrieval time
2. **Compounding** — Every ingest and query makes the wiki richer
3. **Always Cite** — Every claim traces back to `raw/`
4. **Flag Contradictions** — Never silently overwrite; show the evolution of beliefs
5. **Interlink** — 3-10 `[[wiki-links]]` per page; orphans = problems
6. **Wiki-First** — Answer from the wiki before reaching for general knowledge

## File Structure

```
llm-wiki-for-quick/
├── README.md           ← You are here
├── SKILL.md            ← The Quick skill file (copy to ~/.quickwork/skills/llm-wiki/)
└── examples/
    └── SCHEMA.md       ← Example schema file for your vault
```

## Comparison with Other Approaches

| | LLM Wiki (Quick) | RAG | Traditional Notes |
|---|---|---|---|
| When knowledge is processed | At ingest time | At query time | Never (by LLM) |
| Knowledge compounds | ✅ Yes | ❌ No | ❌ No |
| Contradictions handled | ✅ Flagged explicitly | ❌ Latest wins | ❌ Forgotten |
| Cross-references | ✅ Obsidian `[[links]]` | ❌ None | Maybe |
| Works offline | ✅ Local files | Depends | ✅ Yes |
| Requires embedding/vector DB | ❌ No | ✅ Yes | ❌ No |

## See Also

- [Karpathy's original LLM Wiki tweet](https://x.com/karpathy/status/1937537680890122369)
- [sample-kiro-llm-wiki](https://github.com/aws-samples/sample-kiro-llm-wiki) — Kiro-native implementation
- [Obsidian](https://obsidian.md) — Recommended wiki IDE

## License

MIT
