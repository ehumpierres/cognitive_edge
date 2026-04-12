# Skill Graph Content Engine

**Source**: @DeRonin_ on X  
**Date**: 2026-04-12  
**Topic**: Using interconnected markdown files as AI agent knowledge graphs for multi-platform content production

---

## Core Concept

A **skill graph** is a folder of interconnected markdown files where each file is a "knowledge node" and `[[wikilinks]]` create traversable relationships. When an AI agent receives a task, it doesn't just read one file—it follows links, builds cumulative context, and produces output informed by the entire knowledge structure.

> "One flat .md file gives you a TOOL. A graph gives you a TEAM."

The insight: **You're hiring a genius with amnesia every time you start a new chat without context.** A skill graph is the onboarding playbook that eliminates that problem.

---

## Architecture

```
/content-skill-graph
├── index.md              # Command center (entry point)
├── platforms/            # Platform-specific playbooks
│   ├── x.md
│   ├── linkedin.md
│   ├── instagram.md
│   └── ...
├── voice/                # Brand DNA and adaptations
│   ├── brand-voice.md
│   └── platform-tone.md
├── engine/               # Operational backbone
│   ├── hooks.md
│   ├── repurpose.md
│   ├── scheduling.md
│   └── content-types.md
└── audience/             # Audience segment definitions
    ├── builders.md
    └── casual.md
```

17 files, 4 folders = complete content production system.

---

## Key Design Principles

### 1. Index.md is a BRIEFING, Not a Table of Contents

The most common mistake: treating index.md as a file list. It should contain:
- **Identity**: Who you are, what the system does, niche specificity
- **Node Map**: Every link with contextual annotations (not just `[[x]]` but `[[x]] — short-form, hook-driven, 280 chars max`)
- **Execution Instructions**: Explicit workflow for the agent to follow

### 2. Wikilinks Create Traversable Context

```markdown
### Platforms
- [[x]] — short-form, hook-driven, 280 chars max, casual lowercase
- [[linkedin]] — long-form narrative, professional tone, 1500+ words
```

The inline context reduces token consumption—agent makes decisions without opening every file.

### 3. Voice DNA + Platform Adaptation

**brand-voice.md** = universal identity (tone markers, vocabulary, formatting rules)  
**platform-tone.md** = how that voice ADAPTS per platform

> "Same person, different room. You talk differently at a house party vs a business dinner vs a podcast interview."

Example adaptations:
| Platform | Tone Shift |
|----------|------------|
| X | Most casual, lowercase, "lol" ok |
| LinkedIn | Professional but human, "I" stories |
| TikTok | Most energetic, spoken not written |
| Newsletter | Most personal and intimate |

### 4. Repurposing ≠ Reformatting

The litmus test:
> "If someone followed me on ALL platforms, would they be annoyed seeing the same thing everywhere?"

**What changes between platforms:**
- **Angle**: Different entry point into the same topic
- **Hook**: Rewritten per platform
- **Tone**: Adjusted per platform-tone.md
- **Format**: Thread vs carousel vs video vs long-form
- **Depth**: 280 chars → 2,000 words

**Repurpose chain order** (each builds on previous):
1. X (forces brevity, finds core idea)
2. LinkedIn (expand with narrative)
3. Instagram (make it visual)
4. TikTok (make it raw/video)
5. YouTube (make it deep/tutorial)
6. Newsletter (make it personal0
7. Threads (conversational)
8. Facebook (community-focused)

### 5. Hook Formulas as Categorized Patterns

```markdown
## The Contrarian Hook
"You don't need [conventional thing]. You need [this instead]."
- Best on: X, Threads
- Example: "You don't need a content team. You need a skill graph."

## The Proof Hook
"[Before metric] → [after metric] in [timeframe]"
- Best on: X, LinkedIn
- Example: "$8k/mo content spend → $0 after building one system"
```

Hooks are updated weekly based on performance—the graph evolves.

---

## Implementation Methods

### Method 1: Claude Projects (Recommended)
Upload all files to project knowledge base. Every conversation has full graph context.

### Method 2: Paste Context
Copy index.md + relevant files into any AI chat. More context = better output.

### Method 3: Cursor / Claude Code (Most Powerful)
Agent reads files directly from filesystem AND can update them—graph literally evolves itself over time.

---

## Key Insights for Agent Design

1. **Context is everything**: The difference between generic output and native content is the depth of context provided upfront.

2. **Structure beats prose**: Categorized nodes (platforms/, voice/, engine/) create predictable, navigable knowledge.

3. **Wikilinks create intelligence**: The connections between nodes create emergent understanding no single prompt can match.

4. **Evolution is built-in**: hooks.md, platform-tone.md are meant to be updated based on what performs. The system learns.

5. **One input, N outputs**: The repurpose chain turns one idea into 8-10 platform-native pieces. Multiplication through adaptation.

6. **Specificity in identity**: "AI automation, SaaS building, monetizing tech skills" produces completely different content than "vegan meal prep for busy parents." Niche specificity in index.md is THE MOST IMPORTANT.

---

## Relation to Other Patterns

- **Claude folder anatomy**: Similar concept—.claude/CLAUDE.md serves as the briefing document
- **Harness memory ownership**: The skill graph IS the memory layer; owning it means owning your content DNA
- **Karpathy knowledge bases**: Same principle of retrieval-augmented context, applied to creative production

---

## Quotable

> "You're basically hiring a genius with amnesia every single time you start a new chat."

> "The connections between files create intelligence no single prompt can match."

> "Each platform version should think about the topic differently—not reformatted, RETHOUGHT."
