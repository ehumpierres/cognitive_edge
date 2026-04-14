# claude-mem: Reference Architecture for Agent Memory

> Source: Analysis of claude-mem open-source project (April 2026)
> GitHub: ~50K stars in ~2 weeks

## The Problem It Solves

Claude Code's context window (200K tokens, 1M on Opus) auto-compacts at ~95% capacity. That compaction is **lossy**:

- Instructions established early vanish
- Corrections disappear
- Patterns you spent 30 turns teaching get compressed into useless summaries like "user prefers certain coding conventions"

This is the "noise" problem from harness architecture: stale/conflicting fragments corrupt computation.

## The Solution

claude-mem sidesteps the compaction problem entirely:

1. **Background observer** captures tool usage in real time
2. **Semantic summaries** are generated from observations
3. **Storage** in local SQLite database + Chroma vector store
4. **Injection** of only relevant observations at session start

Session starts with a lightweight index. The LLM fetches full records only when it needs depth.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  LIFECYCLE HOOKS                                            │
│                                                             │
│  SessionStart ──▶ Load relevant memories from vector store │
│  UserPromptSubmit ──▶ Capture user intent                  │
│  PostToolUse ──▶ Capture tool results and observations     │
│  Summary ──▶ Generate semantic summaries                   │
│  SessionEnd ──▶ Persist new memories                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  WORKER SERVICE (port 37777)                                │
│                                                             │
│  • Receives observations from hooks                         │
│  • Generates semantic summaries                             │
│  • Manages SQLite + Chroma storage                         │
│  • Handles natural language search queries                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STORAGE LAYER                                              │
│                                                             │
│  SQLite ──▶ Structured records, metadata, relationships    │
│  Chroma ──▶ Vector embeddings for semantic search          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Key Design Decisions

### 1. Lightweight Index First
Don't dump everything into context. Start with an index, fetch depth on demand.

### 2. Semantic Summarization
Raw observations are compressed into semantic summaries that preserve meaning while reducing token count.

### 3. Relevance-Based Injection
Only inject memories relevant to the current session context. This maximizes signal, minimizes noise.

### 4. Dual Storage
- **SQLite**: Structured queries, relationships, metadata
- **Chroma**: Semantic similarity search via embeddings

## The 5 Lifecycle Hooks

| Hook | Purpose |
|------|---------|
| `SessionStart` | Load relevant memories, establish context |
| `UserPromptSubmit` | Capture user intent for later retrieval |
| `PostToolUse` | Capture tool results and observations |
| `Summary` | Generate semantic summaries |
| `SessionEnd` | Persist new memories, clean up |

## Why It Went Viral

> "The fastest path to a massive open-source project right now is finding the gap between what an AI company ships and what developers actually need in production."

Every developer who lost context to auto-compaction felt the same pain. Memory/retrieval was the gap. Someone filled it.

**Stats (first 2 weeks):**
- 49,500 stars
- 3,900 forks
- 1,662 commits
- 117 pull requests

## The Bitter Lesson Validated

This validates the harness architecture thesis:

> As experiential memory accumulates, the job shifts from "store it" to "reliably retrieve and inject the right fragment." Scale + search wins.

## Monetization Note

The creator launched a Solana token ($CMEM) alongside the project — an indicator of where open-source incentive models are heading in 2026.

## Related

- harness-architecture — Theoretical foundation for why this works
- debugging-methodology — Practical application of persistent memory during debugging
