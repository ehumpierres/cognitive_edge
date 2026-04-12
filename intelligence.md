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

**Harnesses encode stale assumptions.** Every harness workaround is a bet that the model can't do X. Models improve; workarounds become dead weight. Design for harness evolution.

**Virtualize agent components.** Like OS abstractions (process, file) outlasted hardware, agent interfaces should outlast implementations. Three components: session (event log), harness (orchestration loop), sandbox (execution). Each independently replaceable.

**Task horizon is infrastructure.** Model capability isn't just benchmarks—it's durability. Opus 4.6's 14-hour task horizon isn't a feature; it's infrastructure that makes /loop, Agent Teams, and Dispatch viable.

**Friction kills agentic work.** The bottleneck isn't capability—it's interruption. Auto Mode (permission-by-policy) removes the interrupt tax.

### On Agent Infrastructure

**Decouple brain from hands.** Brain = Claude + harness. Hands = sandboxes + tools. Universal interface: `execute(name, input) → string`. The harness doesn't know if the sandbox is a container, phone, or Pokémon emulator.

**Cattle, not pets.** Never couple all components into one container. Pet = named, hand-tended, can't afford to lose. Cattle = interchangeable, replaceable. Container dies? Catch the error, spin up a new one.

**Session ≠ context window.** Context window decisions (compaction, trimming) are irreversible. Session log is durable, append-only, queryable via `getEvents()`. Store everything; transform in harness before passing to Claude.

**Credentials never in sandbox.** Untrusted code + accessible credentials = prompt injection wins. Clone repos with tokens at init, wire into remotes. MCP tokens in vault, fetched by proxy. Harness never sees credentials.

**Provision lazily.** Don't pay container setup cost until you need a container. Inference can start from session log immediately. TTFT drops 60-90% with lazy provisioning.

**Many brains, many hands.** Decoupling enables horizontal scale. Stateless harnesses connect to sandboxes only when needed. Brains can pass hands to one another.

### On Knowledge Systems

**LLMs compile knowledge, not just retrieve it.** Structure data as `raw/` (sources) → `wiki/` (compiled). Human curates inputs + asks questions. LLM compiles, maintains, enhances.

**RAG is scale infrastructure, not a requirement.** At ~400K words, auto-maintained indexes beat vector search. Start simple.

**Queries should add up.** File outputs back into wiki. Knowledge compounds.

**LLM linting keeps data healthy.** Periodic health checks for inconsistencies, missing data, connections, new articles.

**Build tools for both humans and LLMs.** Web UI + CLI. LLM becomes operator of your tooling.

### On Agentic Patterns

**Parallel execution with coordination.** Agent Teams: lead agent + parallel teammates via shared task list. Lead agent IS the harness.

**Scheduled persistence via /loop.** Long-running agents need repeating intervals:
- `/loop 5m /babysit` — code review comments
- `/loop 30m /slack-feedback` — PRs for feedback
- `/loop 1h /pr-pruner` — close stale PRs

**Connectors abstract MCP.** 50+ one-click integrations without boilerplate.

### On Deployment

**Green CI ≠ proof of safety.** Passing CI is the agent's ability to persuade your pipeline, not proof the change is safe at scale. A query that scans every row, retry logic that thundering-herds, a cache with no TTL—all pass tests.

**Implementation is abundant; judgment is scarce.** The inflection point: writing code is no longer the bottleneck. Knowing what's safe to ship is. All infrastructure must match this reality.

**Leverage, don't rely.** Relying = agent + tests = ship it. Leveraging = using agents to iterate while maintaining ownership. Litmus test: would you own an incident from this PR?

**Make the right thing easy to do.** Don't wrap development in red tape. Build closed-loop systems where agents act with high autonomy because environment is standardized, verification is easy, deployment is safe by default.

**Self-driving deployments.** Gated pipelines, automatic rollback on canary degradation. No engineer babysitting dashboards. Problems caught, contained, reversed—in isolation, not globally.

**Continuous validation.** Infrastructure tests itself continuously, not just at deploy. Load tests, chaos experiments, DR exercises. Systems hold up under pressure because they've been deliberately stressed.

**Executable guardrails > documentation.** Encode operational knowledge as runnable tools. A `safe-rollout` skill wires flags, generates rollout plans with rollback conditions, specifies verification. Agents follow executable guardrails autonomously.

**Separate generative from verification agents.** Read-only agents continuously verify system invariants. Specialized agents audit assumptions made by generative agents. Different agents for doing vs. checking.

### On Building

*[To be developed as more insights are collected]*

### On Systems Design

*[To be developed as more insights are collected]*

---

## Current Stack Opinions

| Layer | Best-in-Class | Why |
|-------|---------------|-----|
| Model (Long Tasks) | Opus 4.6 | 14-hour task horizon enables persistent agents |
| Session | Append-only event log | Durable, queryable, survives harness crashes |
| Harness | Stateless orchestrator | Restartable via wake(sessionId), no state to lose |
| Sandbox | Cattle containers | execute(name, input) → string, independently replaceable |
| Deployment | Self-driving pipelines | Gated rollouts, auto-rollback, continuous validation |
| Guardrails | Executable skills | Runnable tools > documentation |
| Knowledge Store | Markdown wiki | LLM-maintained, human-readable, version-controlled |
| Knowledge IDE | Obsidian | View raw + compiled + outputs in one place |
| Orchestration | LangGraph | State machines + persistence + human-in-loop |
| Execution | LangChain | Mature tooling, wide integrations |
| Multi-Agent | Agent Teams pattern | Lead agent + parallel teammates + shared task list |
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
- Coupling harness + sandbox in one container (creates pets)
- Credentials accessible from untrusted code execution
- Context window as only memory (irreversible losses)
- Assuming resources live alongside harness
- Provisioning containers before they're needed
- **Assuming agent + passing tests = ready to ship**
- **Shipping PRs you can't explain the production impact of**
- **Operational knowledge trapped in documentation**
- **Manual dashboard babysitting for deployments**
- **Testing only at deploy time**
- **Using same agents for generation and verification**

---

## Open Questions

- ~~How to handle state across long-running agent sessions?~~ → Session log with getEvents()
- Best practices for multi-agent communication protocols?
- When to use single agents vs multi-agent systems?
- ~~Optimal harness design for different task types?~~ → Virtualize components, make harness stateless
- When is /loop preferable to event-driven triggers?
- ~~How to balance Auto Mode safety with necessary human oversight?~~ → Self-driving deployments + executable guardrails
- At what scale does RAG become necessary over auto-maintained indexes?
- When to move knowledge from context windows to fine-tuned weights?

---

## Changelog

| Date | Update |
|------|--------|
| 2026-04-12 | Added deployment beliefs from Vercel: green CI ≠ safety, leverage vs rely, self-driving deployments, executable guardrails, verification agents. |
| 2026-04-12 | Added Managed Agents architecture: virtualization, cattle not pets, session model, credential isolation, lazy provisioning. |
| 2026-04-12 | Added knowledge systems beliefs from Karpathy's LLM knowledge base pattern. |
| 2026-04-12 | Added task horizon, /loop patterns, Auto Mode, Agent Teams from Aakash's Claude Q1 feature analysis. |
| 2026-04-12 | Added harness design + brain/hands decoupling from Anthropic engineering. |
| 2026-04-12 | Initial creation. Added AI architecture beliefs from @rohit4verse insights. |

---

*Last updated: 2026-04-12*
