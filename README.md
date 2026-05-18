# 🧠 LLM Wiki for Quick

An [Amazon Quick](https://quick.aws.dev) skill for managing a personal LLM Wiki — a compounding knowledge base that gets smarter with every source you add.

Based on [Andrej Karpathy's LLM Wiki concept](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) and inspired by [sample-kiro-llm-wiki](https://github.com/aws-samples/sample-kiro-llm-wiki).

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."
> — Andrej Karpathy

## What is an LLM Wiki?

Most people's experience with LLMs and documents looks like RAG: upload files, retrieve chunks at query time, generate an answer. This works, but the LLM rediscovers knowledge from scratch on every question. There's no accumulation.

An LLM Wiki is different. Instead of retrieving from raw documents at query time, the LLM **incrementally compiles** your sources into a structured, interlinked wiki. Knowledge is processed once at ingest time and kept current — not re-derived on every query.

The key difference: **the wiki is a persistent, compounding artifact.** Cross-references are already built. Contradictions are already flagged. The synthesis already reflects everything you've read.

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
Drop a source (URL, article, project learnings) → Quick reads it, creates a source summary, updates relevant concept/entity pages, flags contradictions, and logs everything. A single ingest may touch 10-15 wiki pages.

### 🔍 Query — "What does the wiki say about X?"
Quick searches the wiki first (not general LLM knowledge), synthesizes an answer with citations, and offers to file valuable answers back as new wiki pages. This is the compounding loop — queries become knowledge.

### 🩺 Lint — "Check wiki health"
Quick scans for orphan pages, broken links, contradictions, stale claims, missing concepts, and coverage gaps. Outputs a prioritized action report.

## Usage Modes

### Mode 1: Wiki Management (default)
Trigger the skill explicitly:
- "Ingest this article: [URL]"
- "What does the wiki say about [topic]?"
- "Add a decision: we chose X because Y"
- "Add a pitfall: [what went wrong]"
- "Lint the wiki"

### Mode 2: Wiki-First for All Questions (optional)
If you want Quick to automatically check your wiki before answering any technical question, tell Quick once:

> "When I ask technical questions, always check my LLM Wiki first before answering."

Quick will remember this preference and automatically consult your wiki on future questions. This is optional — Mode 1 works perfectly without it.

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

3. **Create SCHEMA.md** at the wiki root (see `examples/SCHEMA.md` for a template).

4. **Add the vault folder** to Quick's allowed folders (Settings → Capabilities → Local Files).

5. **Start using it:**
   ```
   "Ingest this article: https://example.com/interesting-post"
   "What does the wiki say about database choices?"
   "Lint the wiki"
   "Add a decision: we chose Postgres over DynamoDB because..."
   ```

## What to Put in Your Wiki

The key insight from research: **don't add what the LLM already knows.** Only add what it can't know without being told.

### ✅ Add This
| Type | Example |
|------|---------|
| Decisions (ADRs) | "Why we chose OpenSearch over Elasticsearch" |
| Pitfalls | "DynamoDB hot partition caused 5xx errors at 2AM" |
| Project learnings | "What worked and didn't in the Q3 migration" |
| Synthesized insights | "Comparison: Lambda vs Fargate for our use case" |
| Experience-based knowledge | "In practice, cold starts matter less than..." |
| Golden examples | "This CDK structure scaled cleanly to 20 services" |

### ❌ Don't Add This
| Type | Why Not |
|------|---------|
| API documentation | The LLM already knows this from training |
| Tutorials / how-tos | Generic, not personal |
| Copy-pasted official docs | Zero value added |

**The test:** "Could the LLM make the right decision without this information?"
- **YES** → don't add it
- **NO** → add it

## Key Principles

1. **Compiled Knowledge > RAG** — Process at ingest time, not query time
2. **Compounding** — Every ingest and query makes the wiki richer
3. **Always Cite** — Every claim traces back to `raw/` via frontmatter or [[links]]
4. **Flag Contradictions** — Never silently overwrite; preserve the evolution of beliefs
5. **Interlink** — 3-10 `[[wiki-links]]` per page; orphans = problems
6. **Wiki-First** — Answer from the wiki before reaching for general knowledge

## Repository Structure

```
llm-wiki-for-quick/
├── README.md           ← You are here
├── SKILL.md            ← The Quick skill file (copy to ~/.quickwork/skills/llm-wiki/)
└── examples/
    └── SCHEMA.md       ← Example schema file for your vault
```

## Comparison with Other Approaches

| | LLM Wiki | RAG | Traditional Notes |
|---|---|---|---|
| When knowledge is processed | At ingest time | At query time | Never (by LLM) |
| Knowledge compounds | ✅ Yes | ❌ No | ❌ No |
| Contradictions handled | ✅ Flagged explicitly | ❌ Latest chunk wins | ❌ Forgotten |
| Cross-references | ✅ Obsidian `[[links]]` | ❌ None | Maybe manual |
| Works offline | ✅ Local markdown files | Depends on infra | ✅ Yes |
| Requires vector DB | ❌ No | ✅ Yes | ❌ No |
| Maintenance cost | Low (LLM does it) | Medium (embeddings) | High (manual) |

## How This Compares to Kiro's Implementation

[sample-kiro-llm-wiki](https://github.com/aws-samples/sample-kiro-llm-wiki) implements the same Karpathy pattern for Kiro CLI. Both do the same thing — the difference is which tool runs the operations:

| | This repo (Quick) | sample-kiro-llm-wiki (Kiro) |
|---|---|---|
| Runner | Amazon Quick | Kiro CLI |
| Invocation | Natural language in Quick chat | `kiro chat --agent llm-wiki` |
| Storage | Same Obsidian vault | Same Obsidian vault |
| Operations | Ingest / Query / Lint | Ingest / Query / Lint |

You can use both on the same vault — they're complementary, not competing.

## See Also

- [Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — The original idea
- [sample-kiro-llm-wiki](https://github.com/aws-samples/sample-kiro-llm-wiki) — Kiro CLI implementation
- [Obsidian](https://obsidian.md) — Recommended wiki IDE

## License

MIT
