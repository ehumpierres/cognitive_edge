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

**The models are good enough, the harness isn't.** Most people use AI like driving a Ferrari with the handbrake on — not because they lack ambition, but because they've never seen what a well-configured environment can do. Configuration is the bottleneck, not capability.

**Harnesses are not going away.** Old scaffolding becomes unnecessary, but new scaffolding replaces it. Claude Code has 512k lines of harness code. Even "built-in" features (web search in APIs) are lightweight harnesses behind the API. Design for harness evolution, not harness elimination.

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

### On Memory & Ownership
### On Context & Memory Architecture

**Context window = computation box.** The model only reasons over what's inside it. Everything else (tools, skills, hooks, system prompts, memory) lives as external context that the harness must retrieve, shape, and inject as fragments.

**Signal vs noise in fragments.** Targeted fragments = signal → better computation. Conflicting/stale fragments = noise → model gets confused. The job isn't to dump everything in; it's to inject only what's relevant.

**The bitter lesson applied to context.** Scale + search wins. As experiential memory accumulates, the bottleneck shifts from "store it" to "reliably retrieve and inject the right fragment." Same lesson that ate every other AI subfield is coming for context.

**Memory retrieval architecture.** The pattern that works:
1. Background observer captures tool usage in real time
2. Semantic summaries generated from observations  
3. Storage in SQLite + vector store (Chroma)
4. Injection of only relevant observations at session start
5. Lightweight index first, fetch depth on demand

**5 lifecycle hooks for memory:** SessionStart (load memories), UserPromptSubmit (capture intent), PostToolUse (capture observations), Summary (generate semantic summaries), SessionEnd (persist new memories).

**Auto-compaction is lossy.** Context windows auto-compact at ~95% capacity. That compaction destroys early instructions, corrections, and learned patterns. External memory that survives compaction is essential infrastructure.

**Write evidence to files.** During debugging or research, externalize reasoning to persistent files. This survives context compression and creates retrievable signal. "Write all assumptions and evidence to DEBUG.md" — one instruction that 1000x's debugging efficiency.

### On Structured Agent Work

**4 rules for AI debugging:**
1. List hypotheses before changing code — forces structured thinking
2. Maximum 5 lines per experiment — small surface area = clear signal
3. Write all evidence to files — survives context compression
4. Fail twice in same direction → force switch — prevents perseverating on dead ends

**Hypothesis-driven, not brute-force.** Tokens wasted on brute-forcing can be 1000x more than structured approaches. The agent that writes hypotheses to a file and systematically tests them beats the agent that just tries things.

### On Enterprise AI Transformation

**AI proficiency levels:**
| Level | Description | Expectation |
|-------|-------------|-------------|
| L0 | Sometimes uses ChatGPT. No workflow changes. | *Will not survive* |
| L1 | Built custom GPTs, dabbled in Claude Code. | Starting point |
| L2 | Built an app that automates part of their job. | *Where things get real* |
| L3 | Systems builders who level up everyone else. | Force multipliers |

**Get everyone up the ladder.** Build tools that meet people where they are (L0→L1). Raise expectations as tools mature (L1→L2). Match the mandate to the tooling.

**Creative destruction is the norm.** Tools shipped in January are obsolete by April. Shelf life of weeks, not months. If your internal tools from three months ago still feel state-of-the-art, you're not moving boldly enough.

**Build from center, drive from spokes.** Central team builds platforms/connectors/plumbing. Functional teams build on top and give feedback. Neither fully centralized nor fully decentralized — both.

**Stage > mandate.** Mandates decay. Culture remains. Create venues: Slack channels, office hours, all-hands demos. Make early converts visible. The competitive dynamic does more than any memo.

**Leaderboards create accountability.** Track sessions, skills used, apps shipped. Three dynamics: healthy peer pressure, manager accountability, discovery through emulation.

**Remove every constraint.** Unlimited AI budget (learning investment, not procurement). No token limits. Pre-connected tools. Kill IT bottlenecks on connectors.

**The cost math:** If someone is 2x more productive with AI, you should be willing to spend their entire salary again in tokens. Token consumption per employee is nowhere near double-digit percentages of salary.

**AI in hiring is mandatory.** Absolute requirement for AI proficiency. PM candidates must build working prototypes in interviews.

### On Multiplayer Agents

**Single-player is solved; multiplayer isn't.** AI agents (Claude Code, Cowork) accidentally became the operating system for knowledge work. But they were built for engineers working alone. The collaboration layer doesn't exist.

**4 missing primitives for multiplayer:**
1. **Shared agent memory** — No team-level memory. Each agent accumulates context independently.
2. **Non-code versioning** — Document collaboration shouldn't require branch management.
3. **Agent provenance** — When humans and agents contribute, you need to know who wrote what.
4. **Permission scoping** — Control what context crosses into shared space vs stays private.

**The workarounds ARE the spec.** Non-engineers using GitHub for markdown files. Copy-paste between terminals and Google Docs. Teaching colleagues what "commit" means. Every workaround reveals what's missing.

**Open vs closed collaboration.** If labs solve collaboration inside closed products, your team's shared context moves inside their infrastructure. The gravity shifts from your ownership to theirs. There's room for an independent layer regardless.

**Multiplayer is the moat.** Single-player = Claude eats your lunch. Multiplayer = orchestration, governance, shared memory across humans + agents. No single model owns that.

**Memory isn't a plugin — it's the harness.** Managing context IS managing memory. The harness decides: how is AGENTS.md loaded? What survives compaction? Are interactions queryable? How is memory metadata presented? You can't separate memory from harness.

**If you don't own your harness, you don't own your memory.** Closed harnesses (especially behind APIs) mean you cede control of agent memory to a third party. Memory is what makes agents sticky and differentiated. Don't give that away.

**Levels of lock-in:**
- **Mild:** Stateful APIs (OpenAI Responses, Anthropic server-side compaction) — can't swap models and resume threads
- **Bad:** Closed harness (Claude Agent SDK) — memory format unknown, non-transferable
- **Worst:** Entire harness + long-term memory behind API — zero ownership or visibility

**Memory creates lock-in that models don't.** Without memory, agents are replicable by anyone with the same tools. With memory, you build a proprietary dataset. Model providers are incentivized to lock memory behind APIs for exactly this reason.

**Open harnesses for enterprise.** Requirements: open source, model agnostic, open standards (agents.md, skills), pluggable storage (Postgres, Mongo, Redis), self-hostable, bring your own database.

**Build memory from connections.** Don't require users to re-explain their world. Build context automatically from authenticated tools (Slack, Calendar, Notion, etc.). Run synthesis pipelines to keep memory fresh without user effort.

### On Enterprise Adoption

**Internal AI productivity is a moat.** Using AI well is now a core business need. The companies that make every employee effective with AI will compound advantages competitors cannot match. You do not hand your moat to a vendor.

**Raise the floor, don't lower the ceiling.** The default approach for non-technical users is to simplify — put on rails, fewer options, dummy-proof. Wrong. The goal isn't to remove complexity, but to make it invisible while preserving full capability.

**One person's breakthrough becomes everyone's baseline.** The biggest failure mode: everyone has to figure things out alone. A workflow discovered by one person doesn't help anyone else. Skills must compound into organizational capability.

**The product is the enablement.** People who got the most value weren't those who attended training sessions — they were those who installed a skill on day one and immediately got a result. The product taught faster than training ever could. Every feature is secretly a lesson.

**Pre-connect everything on day one.** SSO sign-in → all tools available with one click. Sales rep asks to pull Gong + enrich with Salesforce + draft follow-up: it just works. This unsexy foundation makes everything else possible.

**Skill marketplaces with AI-guided discovery.** Don't make people browse catalogs of 350+ skills. AI "Sensei" looks at connected tools, role, recent work, and surfaces the 5 skills that matter most right now.

**Laptop as server.** Scheduled automations (daily, weekly, cron), Slack-native assistants, headless mode for long tasks. Kick off work, walk away, approve from phone, results waiting when you return.

**Workspace > chat.** Real work isn't linear. Split panes, multiple sessions, documents alongside conversations. Layout persists across sessions.

**Chat era → agent era.** Enterprises are moving from "let a thousand flowers bloom" experimentation to targeted automation applied to specific workflows. The interface isn't the product anymore; the automation is.

**Change management is the real bottleneck.** Most workflows aren't set up to drop agents in. One pattern: head of AI in every business unit rolling up to a central team.

**Tokenmaxxing: compute budgeting is strategic.** With strict OpEx budgets locked for the year, enterprises are having real trade-off discussions on token allocation. "Shark tank" for compute budget. Hierarchy of needs for use-case rationing.

**Legacy systems block agent access.** Decades of on-prem or cloud-but-not-modernized systems. Agents can't tap in. Data access is the blocker, not model capability.

**Agents = new capabilities, not job cuts.** Major use-cases are things companies couldn't do before: software upgrades, automating back-office blockers, processing documents for insights. More emphasis on making money vs. cutting costs.

**Headless software is non-negotiable.** Enterprises will kick out vendors that don't make agent integration technically or economically easy. If your product doesn't have agent APIs, you'll be replaced.

**Multi-agent interoperability is paramount.** No one wants to get locked into the wrong architecture. Companies realize they're in a multi-agent world — interoperability across systems matters more than picking the "best" agent.

**Technical diffusion takes time.** Skills, MCP, CLIs may be simple for tech; they're esoteric in enterprise. Engineers won't be writing software, but they'll be setting up and operating the systems that automate work.


### On Agent Economy

**Agents are becoming primary users.** The economy is being rebuilt with AI agents as the primary users, not humans. Every capability humans use to operate digitally needs an agent-native equivalent.

**Human UX doesn't work for agents.** Web forms, OAuth flows, visual interfaces — all optimized for humans. Agents need programmatic, structured APIs. Google doesn't work for agents.

**The primitives stack:**
- **Communication:** Email (AgentMail), Phone (AgentPhone), WhatsApp (Kapso), Voice (ElevenLabs, Vapi)
- **Compute:** Sandboxes (Daytona, E2B), Browsers (Browserbase, Browser Use), Web crawling (Firecrawl)
- **Memory:** Persistent memory (Mem0), Web search (Exa), People/company search (Sixtyfour)
- **Action:** Tool orchestration (Composio), API access (Orthogonal), Payments (Kite, Sponge)

**Compose primitives, don't build custom.** When building agents, stitch together existing primitives rather than building custom integrations for each capability.

**Payment unlocks autonomy.** Agents that can pay for things (Kite, Sponge) become economically autonomous. This is a phase shift in agent capability.

### On Knowledge Systems

**LLMs compile knowledge, not just retrieve it.** Structure data as `raw/` (sources) → `wiki/` (compiled). Human curates inputs + asks questions. LLM compiles, maintains, enhances.

**RAG is scale infrastructure, not a requirement.** At ~400K words, auto-maintained indexes beat vector search. Start simple.

**Queries should add up.** File outputs back into wiki. Knowledge compounds.

**LLM linting keeps data healthy.** Periodic health checks for inconsistencies, missing data, connections, new articles.

**Build tools for both humans and LLMs.** Web UI + CLI. LLM becomes operator of your tooling.

**Skill graphs: one file = tool, linked files = team.** A single .md file is a reference document. A folder of interconnected files with `[[wikilinks]]` creates emergent intelligence no single prompt can match. The connections ARE the intelligence.

**Index as briefing, not table of contents.** The entry point (index.md, AGENTS.md) should be a command center: identity, node map with contextual annotations, execution instructions. Not a file list. Context inline with links saves tokens and enables faster decisions.

**Inline context beats traversal.** Don't just link `[[x]]` — link `[[x]] — short-form, hook-driven, 280 chars max`. The extra context helps agents make decisions without opening every file.

**Knowledge graphs evolve.** The skill graph isn't static. Update nodes based on what performs (hooks.md updated weekly). The system gets smarter because you're encoding lessons into the files themselves. Compound interest for knowledge.


### On Agentic Patterns

**Parallel execution with coordination.** Agent Teams: lead agent + parallel teammates via shared task list. Lead agent IS the harness.

**Scheduled persistence via /loop.** Long-running agents need repeating intervals:
- `/loop 5m /babysit` — code review comments
- `/loop 30m /slack-feedback` — PRs for feedback
- `/loop 1h /pr-pruner` — close stale PRs

**Connectors abstract MCP.** 50+ one-click integrations without boilerplate.

**Autonomous research loops.** Give agents bounded scope (single file, single metric, fixed time budget) and explicit permission to run indefinitely. "NEVER STOP" is a valid instruction — don't ask if you should continue, think harder if stuck.

**Bounded scope enables autonomy.** AutoResearch works because constraints are tight: one file to modify (train.py), one metric to optimize (val_bpb), fixed time budget (5 min). The constraints are what make autonomy tractable.

**Simplicity as meta-heuristic.** Beyond raw metrics, instruct agents to prefer simplicity. "A 0.001 improvement that adds 20 lines of hacky code? Not worth it. A 0.001 improvement from deleting code? Definitely keep."

**Git as experiment memory.** Use branches for experiment state (keep = advance, discard = reset). Never commit the experiment log (results.tsv) — it's untracked external memory. Agent references past experiments to avoid repeating failures.

**program.md is a lightweight skill.** Setup phase → constraints → goal → loop → meta-rules. Human iterates on the skill file to find the "research org code" that achieves fastest progress.


### On Agent Configuration

**CLAUDE.md < 200 lines.** Instruction adherence drops with length. Keep it focused: commands, architecture, conventions, gotchas. Don't duplicate what linters already enforce.

**Instructions are suggestions; hooks are deterministic.** CLAUDE.md tells Claude what to do. Hooks MAKE it happen — shell scripts that fire at PreToolUse, PostToolUse, Stop, etc. Use hooks for security gates and quality gates.

**Exit code 2 blocks, exit code 1 doesn't.** Common mistake: using exit 1 for security hooks. Exit 1 = error but non-blocking. Exit 2 = stop everything. Know the difference.

**Path-scoped rules.** Don't load API conventions when editing React components. Use rules/ folder with YAML frontmatter `paths:` to scope instructions to relevant files.

**Skills are packages, commands are files.** Skills can bundle supporting files (@DETAILED_GUIDE.md). Commands are single markdown files. Use skills when workflows need reference material.

**Two-level config: project + personal.** Project `.claude/` is committed (team-wide). `~/.claude/` is personal (global preferences, auto-memory). CLAUDE.local.md and settings.local.json are gitignored for personal overrides.

**Restrict agent tools explicitly.** A security auditor agent doesn't need Write access. Define `tools: Read, Grep, Glob` in agent markdown frontmatter.

**Passive context beats active retrieval.** AGENTS.md achieved 100% pass rate on Vercel evals; skills maxed at 79% with explicit triggers, 53% without (same as baseline). Skills weren't invoked 56% of the time. Why passive wins: no decision point ("should I look this up?"), consistent availability every turn, no ordering issues.

**Compress docs to index + retrieval.** 8KB compressed index pointing to retrievable files achieved same 100% as full docs. Include instruction: "Prefer retrieval-led reasoning over pre-training-led reasoning."

**Skills for vertical, AGENTS.md for horizontal.** Skills work for explicit action workflows ("upgrade Next.js"). AGENTS.md works for broad framework knowledge. They complement, but for general knowledge, passive context wins.

### On Agent Teams

**One agent, one job.** A single agent with a massive prompt produces mediocre everything — context fills up, quality degrades. Specialized agents with clear roles excel. Give each agent a boring job title and a stop condition.

**File-based coordination beats orchestration frameworks.** No API calls between agents. No message queues. Just files. One-writer, many-readers pattern. Dwight writes `DAILY-INTEL.md`, Kelly/Rachel/Pam read it. Files don't crash, don't need auth, don't rate limit.

**Two-layer memory.** Daily logs (`memory/YYYY-MM-DD.md`) = raw notes. Long-term (`MEMORY.md`) = curated insights distilled from logs. Agents get better because context gets richer, not because models improve.

**Corrective prompt-engineering.** First version is mediocre. Tenth is good. Thirtieth is great. Personality emerges from weeks of corrections stored in memory. TV character naming gives instant personality baseline.

**Heartbeat self-healing.** Cron jobs fail. HEARTBEAT.md checks for stale jobs (>26 hours) and forces re-run. Self-healing, no human intervention.

**The moat is the system, not the model.** Everyone has access to the same models. The alpha is the SOUL.md files, the memory, the scheduling, the weeks of corrective feedback. That system is yours.

### On Deployment

**Green CI ≠ proof of safety.** Passing CI is the agent's ability to persuade your pipeline, not proof the change is safe at scale. A query that scans every row, retry logic that thundering-herds, a cache with no TTL—all pass tests.

**Implementation is abundant; judgment is scarce.** The inflection point: writing code is no longer the bottleneck. Knowing what's safe to ship is. All infrastructure must match this reality.

**Leverage, don't rely.** Relying = agent + tests = ship it. Leveraging = using agents to iterate while maintaining ownership. Litmus test: would you own an incident from this PR?

**Make the right thing easy to do.** Don't wrap development in red tape. Build closed-loop systems where agents act with high autonomy because environment is standardized, verification is easy, deployment is safe by default.

**Self-driving deployments.** Gated pipelines, automatic rollback on canary degradation. No engineer babysitting dashboards. Problems caught, contained, reversed—in isolation, not globally.

**Continuous validation.** Infrastructure tests itself continuously, not just at deploy. Load tests, chaos experiments, DR exercises. Systems hold up under pressure because they've been deliberately stressed.

**Executable guardrails > documentation.** Encode operational knowledge as runnable tools. A `safe-rollout` skill wires flags, generates rollout plans with rollback conditions, specifies verification. Agents follow executable guardrails autonomously.

**Separate generative from verification agents.** Read-only agents continuously verify system invariants. Specialized agents audit assumptions made by generative agents. Different agents for doing vs. checking.

**AI optimizes for "works" not "works safely."** Vibe-coded apps compile and pass tests but ship without: rate limiting, input sanitization, session expiry, database indexing, connection pooling, CORS, logging, health checks, backups. These are opt-in — AI won't include them unless you ask. Make production requirements explicit in CLAUDE.md.

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
| Harness | Open, model-agnostic | Own your memory; avoid API lock-in |
| Sandbox | Daytona, E2B | Cattle execution environments for agents |
| Browser | Browserbase, Browser Use | Agent-native browser control |
| Memory | Self-hosted + auto-synthesis | Own data; build from connections; 24h refresh |
| Skills | Marketplace + AI discovery | One person's breakthrough = everyone's baseline |
| Deployment | Self-driving pipelines | Gated rollouts, auto-rollback, continuous validation |
| Guardrails | Executable skills | Runnable tools > documentation |
| Knowledge Store | Markdown wiki | LLM-maintained, human-readable, version-controlled |
| Knowledge IDE | Obsidian | View raw + compiled + outputs in one place |
| Orchestration | LangGraph | State machines + persistence + human-in-loop |
| Tool Integration | SSO + pre-connected | Day one everything works |
| Search | Exa | Agent-optimized web search (not Google) |
| Multi-Agent | Agent Teams pattern | Lead agent + parallel teammates + shared task list |
| Permission Handling | Auto Mode | Two-check safety without click fatigue |
| Interface | Workspace (split panes) | Real work isn't linear |

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
- Assuming agent + passing tests = ready to ship
- Shipping PRs you can't explain the production impact of
- Operational knowledge trapped in documentation
- Manual dashboard babysitting for deployments
- Testing only at deploy time
- Using same agents for generation and verification
- Building custom integrations when primitives exist
- Using human-oriented UX/search for agent workflows
- Ceding memory to closed harnesses or provider APIs
- Using stateful provider APIs when portability matters
- Treating memory as separate from harness (it's not)
- Encrypted/proprietary formats in "open" tools (e.g., Codex compaction)
- **Lowering the ceiling instead of raising the floor**
- **Training sessions instead of product-as-enablement**
- **Making people browse skill catalogs instead of AI-guided discovery**
- **Requiring users to re-explain their world (vs. building memory from connections)**
- **Chat windows instead of workspaces**
- **Shipping vibe-coded apps without security checklist** (rate limiting, auth, indexing, logging, backups)
- **Auth tokens in localStorage** (use httpOnly cookies)
- **Sessions that never expire**
- **No TypeScript on AI-generated code**
- **Treating index files as tables of contents** (they're briefings with inline context)
- **Hiring a genius with amnesia** (new chat, no context, every time)
- **Reformatting instead of rethinking** (same content copy-pasted across platforms)
- **Asking "should I continue?" in autonomous loops** (grant explicit permission to run indefinitely)
- **Unbounded scope for autonomous agents** (single file + single metric + fixed time = tractable autonomy)
- **Pitching agents as job cutters** (enterprises want new capabilities, not headcount reduction)
- **Assuming AI makes enterprise adoption easy** (Skills, MCP, CLIs are esoteric outside tech)
- **Building software without agent APIs** (headless is non-negotiable; vendors get kicked out)
- **Ignoring token economics** (tokenmaxxing is a board-level conversation)

---

## Gaps: Enterprise Readiness 🚨

*Critical areas where we lack authoritative sources. Actively hunting for content.*

Enterprise adoption of agentic systems requires capabilities we don't yet have strong opinions on:

### Calculation Traceability
- **The problem:** How do you audit *what* an agent computed and *why*?
- **What we need:** Chain of reasoning, intermediate steps, decision justification
- **Partial coverage:** Anthropic's session log is append-only and durable — infrastructure exists, but no stance on *how* to build auditable agent outputs
- **Related:** Extended thinking / reasoning traces may help, but need enterprise patterns

### Output Validation
- **The problem:** Beyond "tests pass" — how do you verify correctness for business logic?
- **What we need:** Validation frameworks for agent-generated calculations, decisions, recommendations
- **Partial coverage:** Vercel's verification agents are a start, but focused on system invariants not business correctness

### Compliance & Auditability
- **The problem:** SOC2, audit trails, regulatory requirements for agent actions
- **What we need:** Patterns for logging, retention, access control, audit response
- **Current coverage:** None

### Enterprise Trust Models
- **The problem:** Who approved this action? What's the approval chain for autonomous agents?
- **What we need:** Approval workflows, delegation policies, human-in-loop patterns for high-stakes decisions
- **Partial coverage:** Auto Mode has two-check safety, but no enterprise governance patterns

### Determinism & Reproducibility
- **The problem:** Can you replay an agent's work and get the same result?
- **What we need:** Patterns for reproducible agent execution, seed management, version pinning
- **Current coverage:** None

### Security Posture
- **What we have:** Credential isolation (Anthropic), brain/hands separation prevents prompt injection escalation
- **What we're missing:** Threat modeling for agentic systems, adversarial robustness, data exfiltration prevention

### ~~Data Portability & Ownership~~ ✅
- **Resolved:** Open harnesses with pluggable storage (Postgres/Mongo/Redis), model-agnostic, self-hostable
- **Key insight:** Memory isn't a plugin — it's the harness. Own your harness = own your memory.

### ~~Enterprise Rollout Pattern~~ ✅
- **Resolved:** Ramp Glass pattern — SSO + pre-connected integrations, skill marketplace with AI discovery, memory from connections, workspace interface.
- **Key insight:** Product is the enablement. Raise the floor, don't lower the ceiling.

---

## Open Questions

### Resolved
- ~~How to handle state across long-running agent sessions?~~ → Session log with getEvents()
- ~~Optimal harness design for different task types?~~ → Virtualize components, make harness stateless
- ~~How to balance Auto Mode safety with necessary human oversight?~~ → Self-driving deployments + executable guardrails
- ~~How to maintain data portability across providers?~~ → Open harnesses, own your memory, pluggable storage
- ~~How to roll out AI tools to non-technical users?~~ → Pre-connect everything, skill marketplace, AI-guided discovery, product = enablement

### Architecture & Patterns
- Best practices for multi-agent communication protocols?
- When to use single agents vs multi-agent systems?
- When is /loop preferable to event-driven triggers?
- At what scale does RAG become necessary over auto-maintained indexes?
- When to move knowledge from context windows to fine-tuned weights?

### Agent Economy
- Which agent primitives will commoditize vs. differentiate?
- How do payment-enabled agents change trust models?

### Enterprise Readiness (Priority Hunt)
- How to implement calculation traceability for auditable agent outputs?
- What validation frameworks exist for agent-generated business logic?
- Patterns for SOC2/compliance-ready agent architectures?
- Enterprise governance models for autonomous agent approval chains?
- How to achieve deterministic/reproducible agent execution?
- Threat models and security frameworks specific to agentic systems?

---

## Changelog

| Date | Update |
| 2026-04-14 | Added multiplayer agents gap analysis: 4 missing primitives (shared memory, non-code versioning, provenance, permission scoping), open vs closed collaboration. |
| 2026-04-14 | Added structured agent work patterns: 4 debugging rules, hypothesis-driven not brute-force. |
| 2026-04-14 | Added context & memory architecture: computation box model, signal vs noise, bitter lesson applied to context, memory retrieval patterns, lifecycle hooks. |
| 2026-04-14 | Expanded enterprise adoption: AI proficiency levels (L0-L3), creative destruction, center+spokes org design, leaderboards, cost math for tokens. |
|------|--------|
| 2026-04-12 | Added AGENTS.md vs skills eval results: passive context (100%) beats active retrieval (79% max). Compress to index + retrieval. |
| 2026-04-12 | Added autonomous agent team setup: one agent one job, file-based coordination, two-layer memory, corrective prompt-engineering, heartbeat self-healing. |
| 2026-04-12 | Added vibe-coding security checklist: 20 ways AI-generated apps fail in production. AI optimizes for "works" not "works safely." |
| 2026-04-12 | Added agent configuration beliefs from .claude/ folder anatomy: CLAUDE.md < 200 lines, hooks vs instructions, exit codes, path-scoped rules, skills vs commands. |
| 2026-04-12 | Added enterprise adoption beliefs from Ramp Glass: raise floor not lower ceiling, product = enablement, skill marketplace, memory from connections, workspace > chat. Resolved enterprise rollout gap. |
| 2026-04-12 | Added memory ownership beliefs from Harrison Chase: memory is the harness, open harnesses for enterprise, lock-in levels. Resolved data portability gap. |
| 2026-04-12 | Added enterprise readiness gaps: traceability, validation, compliance, trust models, determinism, security. Flagged as priority hunt. |
| 2026-04-12 | Added agent economy primitives: agents as primary users, primitive categories (communication, compute, memory, action). |
| 2026-04-12 | Added deployment beliefs from Vercel: green CI ≠ safety, leverage vs rely, self-driving deployments, executable guardrails, verification agents. |
| 2026-04-12 | Added Managed Agents architecture: virtualization, cattle not pets, session model, credential isolation, lazy provisioning. |
| 2026-04-12 | Added knowledge systems beliefs from Karpathy's LLM knowledge base pattern. |
| 2026-04-12 | Added task horizon, /loop patterns, Auto Mode, Agent Teams from Aakash's Claude Q1 feature analysis. |
| 2026-04-12 | Added harness design + brain/hands decoupling from Anthropic engineering. |
| 2026-04-12 | Added enterprise ground-truth from Levie: tokenmaxxing, legacy systems, headless software, technical diffusion, agents = new capabilities not job cuts. |
| 2026-04-12 | Added autonomous research loop patterns from Karpathy AutoResearch: NEVER STOP, bounded scope, simplicity heuristic, git as memory. |
| 2026-04-12 | Added skill graph knowledge patterns: linked files = team, index as briefing, inline context, evolving graphs. |
| 2026-04-12 | Initial creation. Added AI architecture beliefs from @rohit4verse insights. |

---

*Last updated: 2026-04-14*
