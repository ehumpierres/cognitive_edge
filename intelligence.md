# Intelligence

*A living synthesis of best-in-class thinking on AI, systems architecture, and deployment.*

> This document distills the collective insights from cognitive_edge into actionable opinions. It evolves as new intelligence is added. When sources conflict, we take the approach that works best in production.

---

## Core Beliefs

### On AI Architecture

**Hybrid beats monolithic.** No single framework solves everything. Production systems layer specialized tools: micro-orchestration for execution, single agents for bounded tasks, multi-agent systems for complex goals.

**The stack has layers:**
1. **Execution** — LangChain, function calling, tool use
2. **Agents** — Goal-directed, tool-using, context-aware
3. **Orchestration** — Multi-agent coordination, task decomposition
4. **Meta** — Self-improvement, learning, adaptation

**GenAI is not Agentic AI.** Base LLMs are reactive and stateless. Agents are proactive and tool-equipped. Agentic systems are collaborative ecosystems. Know what you're building.

**Harnesses enable long-running agents.** The key to autonomous work isn't just smarter models—it's the infrastructure (harness) that supports persistent, multi-turn agent execution. Anthropic validates this internally.

**Decouple brain from hands.** Separate reasoning from execution in managed agent architectures. The "brain" plans and decides; the "hands" execute. This enables better scaling and debugging.

**Task horizon is infrastructure.** Model capability isn't just benchmarks—it's durability. Opus 4.6's 14-hour task horizon isn't a feature; it's infrastructure that makes /loop, Agent Teams, and Dispatch viable. A model that drifts after 2 hours can't run overnight workflows.

**Friction kills agentic work.** The bottleneck isn't capability—it's interruption. Auto Mode (permission-by-policy) removes the interrupt tax. Every "Allow" click breaks flow.

### On Knowledge Systems

**LLMs compile knowledge, not just retrieve it.** Structure data as `raw/` (sources) → `wiki/` (compiled). The LLM transforms unstructured sources into interlinked, categorized knowledge. Human role: curate inputs, ask questions. LLM role: compile, maintain, enhance.

**RAG is scale infrastructure, not a requirement.** At small scale (~100 articles, ~400K words), auto-maintained indexes beat vector search. LLMs are good at maintaining their own summaries and finding relevant context. Start simple.

**Queries should add up.** File query outputs back into the knowledge base. Every exploration becomes a wiki enhancement. Knowledge compounds.

**LLM linting keeps data healthy.** Run periodic "health checks" to find inconsistencies, impute missing data, discover connections, suggest new articles. The wiki is never static.

**Build tools for both humans and LLMs.** Good tools have a human interface (web UI) AND an LLM interface (CLI). The LLM becomes an operator of your tooling.

### On Agentic Patterns

**Parallel execution with coordination.** Agent Teams demonstrates the pattern: a lead agent delegates to parallel teammates via shared task list. The lead agent IS the harness.

**Scheduled persistence via /loop.** Long-running agents need repeating intervals, not one-shot runs. Patterns that work:
- `/loop 5m /babysit` — auto-address code review comments
- `/loop 30m /slack-feedback` — PRs for feedback
- `/loop 1h /pr-pruner` — close stale PRs

**Connectors abstract MCP.** One-click integrations (50+) mean agents can access real workflows (Figma, Slack, Notion) without MCP boilerplate.

### On Building

*[To be developed as more insights are collected]*

### On Deployment

*[To be developed as more insights are collected]*

### On Systems Design

*[To be developed as more insights are collected]*

---

## Current Stack Opinions

| Layer | Best-in-Class | Why |
|-------|---------------|-----|
| Model (Long Tasks) | Opus 4.6 | 14-hour task horizon enables persistent agents |
| Knowledge Store | Markdown wiki | LLM-maintained, human-readable, version-controlled |
| Knowledge IDE | Obsidian | View raw + compiled + outputs in one place |
| Orchestration | LangGraph | State machines + persistence + human-in-loop |
| Execution | LangChain | Mature tooling, wide integrations |
| Multi-Agent | Agent Teams pattern | Lead agent + parallel teammates + shared task list |
| Long-Running | Harness + /loop | Anthropic-validated pattern for persistence |
| Permission Handling | Auto Mode | Two-check safety without click fatigue |
| Integrations | MCP Connectors | 50+ pre-built, no boilerplate |

---

## Anti-Patterns

- Treating AI like traditional software (buy, plug in, done)
- Single-framework absolutism
- Agents without tool access
- Complex goals without task decomposition
- Long-running agents without harness infrastructure
- Coupling reasoning and execution in the same loop
- Permission prompts for every routine action (interrupt tax)
- Models without extended task horizon for persistent work
- Reaching for RAG before trying auto-maintained indexes
- Manually editing LLM-maintained wikis
- Treating knowledge bases as static (no linting/enhancement)
- Building tools for only humans OR only LLMs, not both

---

## Open Questions

- How to handle state across long-running agent sessions?
- Best practices for multi-agent communication protocols?
- When to use single agents vs multi-agent systems?
- Optimal harness design for different task types?
- When is /loop preferable to event-driven triggers?
- How to balance Auto Mode safety with necessary human oversight?
- At what scale does RAG become necessary over auto-maintained indexes?
- When to move knowledge from context windows to fine-tuned weights?

---

## Changelog

| Date | Update |
|------|--------|
| 2026-04-12 | Added knowledge systems beliefs from Karpathy's LLM knowledge base pattern. |
| 2026-04-12 | Added task horizon, /loop patterns, Auto Mode, Agent Teams from Aakash's Claude Q1 feature analysis. |
| 2026-04-12 | Added harness design + brain/hands decoupling from Anthropic engineering. |
| 2026-04-12 | Initial creation. Added AI architecture beliefs from @rohit4verse insights. |

---

*Last updated: 2026-04-12*
