# 🧠 Cognitive Edge

**Intelligence Collection and Organization for AI & Systems Architecture**

A curated knowledge base of insights extracted from articles, papers, and resources about AI technology, systems architecture, and deployment strategies.

## Purpose

This repo serves as a second brain for technology intelligence:
- **Collect** URLs and resources worth remembering
- **Extract** key insights succinctly but expressively
- **Organize** knowledge so both humans and AI can learn from it
- **Synthesize** a unified worldview from the best ideas

## Key Documents

### 📜 [intelligence.md](intelligence.md)
The brain of this repo. A living document that synthesizes all collected insights into **opinionated, best-in-class thinking** on AI, architecture, and deployment. When sources conflict, we pick what works best in production. This document evolves as new intelligence is added.

### 📋 [sources.md](sources.md)
Index of all processed URLs and their corresponding insight documents.

## Structure

```
cognitive_edge/
├── intelligence.md       # Living synthesis of best-in-class opinions
├── sources.md            # Index of all processed URLs
├── ai/
│   ├── agents/           # AI agents, assistants, autonomous systems
│   ├── models/           # LLMs, training, fine-tuning, benchmarks
│   ├── prompting/        # Prompt engineering, techniques, patterns
│   └── tooling/          # AI dev tools, frameworks, SDKs
├── architecture/
│   ├── distributed/      # Distributed systems, scaling, resilience
│   ├── data/             # Data pipelines, storage, processing
│   └── patterns/         # Design patterns, best practices
└── deployment/
    ├── cloud/            # Cloud platforms, services, IaC
    ├── edge/             # Edge computing, IoT, embedded
    └── ops/              # DevOps, MLOps, monitoring
```

## How It Works

1. **You share a URL** → I read and extract the important bits
2. **I create an insight doc** → Placed in the appropriate folder in the tree
3. **I update intelligence.md** → Distill new opinions, resolve conflicts, evolve the worldview
4. **Knowledge compounds** → Cross-referenced and searchable

## Document Format

Each insight document follows this structure:

```markdown
# [Title]

**Source:** [URL]
**Date Added:** YYYY-MM-DD
**Tags:** #tag1 #tag2

## TL;DR
[One-paragraph summary]

## Key Insights
- Insight 1
- Insight 2
- Insight 3

## Notable Quotes
> "Quote worth remembering"

## Connections
- Related to: [[other-document]]
- See also: [external link]
```

---

*Maintained by Nina for Ernesto* 🤖
