# Harness Architecture: Memory, Context Fragments & The Bitter Lesson

> Source: Harness Engineering / Vivek / Deep Agents (April 2026)

## Core Concept

The context window is a **computation box** — the model only reasons over what's inside it. Everything else (tools, skills, hooks, system prompts, memory) lives as **external context** that the harness must retrieve, shape, and inject as **fragments**.

## Architecture Overview

```
EXTERNAL CONTEXT                              CONTEXT WINDOW
                                              (Computation Box)
┌─────────────────┐                           
│ System prompt   │──┐                        ┌────────────────────┐
│ base + appended │  │                        │ • system prompt    │
├─────────────────┤  │   RETRIEVE             │ • tool/skill desc  │
│ Tools/Skills    │──┼──  SHAPE  ──────────▶  │ • hook injections  │
│ MCP, schemas    │  │    INJECT              │ • retrieved memory │
├─────────────────┤  │                        │ • user turn/tools  │
│ Hooks/Middleware│──┤                        │ • exec trajectory  │
│ cwd, ctx, todos │  │                        │                    │
├─────────────────┤  │                        │ MODEL: compute()   │
│ Memory store    │──┘                        │   → next action    │
│ ever-growing    │                           └────────────────────┘
└─────────────────┘
```

## Signal vs Noise

| Signal (Good) | Noise (Bad) |
|---------------|-------------|
| Targeted fragments | Conflicting fragments |
| Relevant context | Stale information |
| Clear instructions | Redundant data |

**More signal → better computation**  
**More noise → model gets confused**

## Key Principles

### 1. Context Window = Computation Box
The model only reasons over what's inside it. Everything else is an object the harness must fetch and shape before anything enters.

### 2. Memory & Search
As agents run longer and accumulate experiential memory, **retrieval — not storage — becomes the bottleneck**. The job shifts from "store it" to "reliably retrieve and inject the right fragment."

### 3. Fragments Guide Computation
Tools, hooks, skills, memories, system prompts — each contributes fragments the model reasons over. Targeted fragments steer the computation; conflicting ones create the errors you debug later.

### 4. The Bitter Lesson Applied to Context
Scale + search wins. The same lesson that dominated other AI subfields (from Rich Sutton's essay) is coming for context management.

## Implications for Agent Design

1. **Don't dump everything** — inject only relevant fragments
2. **Build retrieval, not just storage** — search quality matters more than storage capacity
3. **Minimize conflicting context** — stale/contradictory fragments corrupt reasoning
4. **Write trajectories back** — new execution results become experiential memory for future retrieval

## Related

- claude-mem-architecture — Reference implementation of memory retrieval layer
- debugging-methodology — Practical application of fragment management during debugging
