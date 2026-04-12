# LLM Knowledge Bases: Compiling Raw Data into Living Wikis

**Source:** Andrej Karpathy  
**Date:** 2026-04-12  
**Tags:** #knowledge-management #llm #obsidian #rag-alternative #personal-wiki #andrej-karpathy

---

## TL;DR

Use LLMs to **compile** raw source documents into a structured markdown wiki. The LLM writes and maintains all wiki content — humans rarely touch it directly. At small scale (~400K words), auto-maintained indexes beat fancy RAG. Queries get filed back into the wiki, so exploration "adds up." LLM linting keeps data consistent. The wiki becomes both knowledge store and LLM workspace.

---

## Key Insights

### 1. LLMs as Knowledge Compilers

> "I use an LLM to incrementally 'compile' a wiki, which is just a collection of .md files in a directory structure."

**The pattern:**
- `raw/` — source documents (articles, papers, repos, datasets, images)
- `wiki/` — LLM-generated summaries, concept articles, backlinks, categorization

The LLM transforms unstructured sources into structured, interlinked knowledge. Human role: curate inputs, ask questions. LLM role: compile, maintain, enhance.

### 2. No RAG Needed at Small Scale

> "I thought I had to reach for fancy RAG, but the LLM has been pretty good about auto-maintaining index files and brief summaries of all the documents."

At ~100 articles / ~400K words, the LLM can:
- Auto-maintain index files
- Write brief summaries of each document
- Read all relevant data for queries without vector search

**Implication:** RAG is infrastructure for scale, not a requirement. Start simple.

### 3. Queries Add Up

> "Often, I end up 'filing' the outputs back into the wiki to enhance it for further queries. So my own explorations and queries always 'add up' in the knowledge base."

Output formats: markdown, slide shows (Marp), matplotlib images — all viewable in Obsidian, all fileable back into wiki.

**Pattern:** Every query is a potential wiki enhancement. Knowledge compounds.

### 4. LLM Linting for Data Integrity

> "I've run some LLM 'health checks' over the wiki to find inconsistent data, impute missing data (with web searchers), find interesting connections for new article candidates."

LLMs are good at:
- Finding inconsistencies across articles
- Suggesting missing data to fill in
- Identifying connections between concepts
- Proposing new articles to write

**Anti-pattern:** Treating the wiki as static. LLMs should continuously improve it.

### 5. Tools as LLM Accessories

> "I vibe coded a small and naive search engine over the wiki, which I both use directly (in a web ui), but more often I want to hand it off to an LLM via CLI as a tool."

Build tools that:
1. Humans can use directly (web UI)
2. LLMs can invoke via CLI

The LLM becomes an operator of your tooling, not just a responder.

### 6. Future: Knowledge in Weights

> "As the repo grows, the natural desire is to also think about synthetic data generation + finetuning to have your LLM 'know' the data in its weights instead of just context windows."

Context windows are runtime memory. Weights are learned memory. At sufficient scale, finetuning becomes attractive.

---

## The Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Ingest | Obsidian Web Clipper | Convert web → markdown + images |
| Storage | Directory structure | raw/ + wiki/ in plain files |
| IDE | Obsidian | View, navigate, render (Marp for slides) |
| Processing | LLM (via CLI) | Compile, maintain, query, lint |
| Output | Markdown, images, slides | Filed back into wiki |

---

## Workflow

```
Sources → raw/ → LLM compile → wiki/
                     ↑            ↓
                     └── LLM Q&A ←┘
                           ↓
                     Outputs filed back
```

---

## Connections

- **To cognitive_edge:** This IS what we're building — a compiled knowledge base of AI/systems intelligence
- **To Anthropic harness pattern:** The LLM operating on wiki IS a harness — persistent, multi-turn, tool-using
- **To /loop:** Could run `/loop` to periodically lint and enhance the wiki

---

## Notable Quotes

> "You rarely ever write or edit the wiki manually, it's the domain of the LLM."

> "I think there is room here for an incredible new product instead of a hacky collection of scripts."

---

## Actionable Takeaways

1. Structure knowledge as `raw/` (sources) + `wiki/` (compiled)
2. Let LLM maintain wiki — don't manually edit
3. Skip RAG at small scale; use auto-maintained indexes
4. File query outputs back into wiki
5. Run periodic LLM health checks / linting
6. Build tools for both human and LLM use
