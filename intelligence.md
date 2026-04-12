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
| Orchestration | LangGraph | State machines + persistence + human-in-loop |
| Execution | LangChain | Mature tooling, wide integrations |
| Multi-Agent | Agentic patterns | Specialized roles > generalist agents |
| Long-Running | Harness design | Anthropic-validated pattern for persistence |

---

## Anti-Patterns

- Treating AI like traditional software (buy, plug in, done)
- Single-framework absolutism
- Agents without tool access
- Complex goals without task decomposition
- Long-running agents without harness infrastructure
- Coupling reasoning and execution in the same loop

---

## Open Questions

- How to handle state across long-running agent sessions?
- Best practices for multi-agent communication protocols?
- When to use single agents vs multi-agent systems?
- Optimal harness design for different task types?

---

## Changelog

| Date | Update |
|------|--------|
| 2026-04-12 | Added harness design + brain/hands decoupling from Anthropic engineering. |
| 2026-04-12 | Initial creation. Added AI architecture beliefs from @rohit4verse insights. |

---

*Last updated: 2026-04-12*
