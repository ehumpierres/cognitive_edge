# Multiplayer Agents: The Collaboration Gap

> Source: Melody Koh (@melodykoh), NextView VC (April 2026)

## The Problem

AI coding agents (Claude Code, Cowork) accidentally became the operating system for knowledge work. But they were built for engineers working alone.

**Current state of collaboration:**
- Copy-paste between local terminal and Google Docs
- Or: clone, branch, commit, push — full git workflow — just to collaborate on a document
- "MarkdownHub" as a joke that isn't funny because it's true

> "Nobody chose markdown and git as the collaboration medium for day-to-day knowledge work. The tooling chose it for them, because the best AI tool happened to be a developer tool and everyone else inherited the developer's infrastructure."

## Single-Player Mode Is Solved

The best AI agents right now were originally built for software engineers. Non-engineers adopted them anyway, and inherited collaboration infrastructure designed for code, not for two people who want to collaborate on a document together.

**What works (single-player):**
- Claude Code / Cowork as personal operating system
- Reads/writes files, runs commands
- Remembers context across sessions
- Connects to external tools through MCP

**What doesn't work (multiplayer):**
- Sharing "AI-collaborated" work with teammates
- Team-level memory and context
- Real-time collaboration with agent participation
- Version control for non-code artifacts

## The 4 Missing Primitives

### 1. Shared Agent Memory

Every person's agent accumulates context independently: deal history, institutional knowledge, prior research. There's no team-level memory.

> "When I switch from a deal I've been working on to a strategy document my partner started, my agent has no idea what his agent already knows. Each agent starts from zero on shared context that should inform everyone's work."

### 2. Non-Code Versioning

Collaborating on a document shouldn't require learning branch management.

> "Nobody's solved version control for non-code artifacts in a way that agents can participate in and non-engineers can use."

This is the primitive most requested by founders who've adopted Claude Code for non-engineering work.

### 3. Agent Provenance

When three people and two agents contribute to a document, you need to know who wrote what. Agent provenance needs to be built in, not bolted on.

**Current solutions:**
- Every's Proof: Green rail for human, purple for AI
- But solves one document at a time

### 4. Permission Scoping for Shared Context

When two people's agents contribute to a shared project, each person controls:
- What context crosses into the shared space
- What stays private

This connects to the "Context Gate" problem — not just what your agent shares externally, but what it shares with your teammate's agent.

## The Race

| Player | Status |
|--------|--------|
| **Anthropic** | Racing to make Cowork reach "wider market than Claude Code." But Cowork Projects still local-only, no sharing even on Enterprise. |
| **Microsoft** | Copilot has 3.3% paid adoption across 450M seats. Not working. |
| **OpenAI** | Merging ChatGPT, Codex, and Atlas browser into single desktop "super app." |
| **Google** | Workspace exists but not agent-native. |

> "A CLI tool nobody expected non-engineers to use accidentally became the most capable knowledge work environment available, and it has no multiplayer mode."

## Open vs Closed: The Critical Question

### Open (Current State)
- Claude Code sits on top of your repositories
- Your context, your memory, your institutional knowledge lives in files you control
- The value accrues to your own infrastructure
- MCP donated to Linux Foundation, co-founded with Block and OpenAI, adopted by Google, Microsoft

### Closed (Risk)
- Cowork Projects are closed: local-only, no export, no interoperability
- If Anthropic solves collaboration inside Cowork, your team's shared context moves inside their product
- The gravity shifts from your infrastructure to theirs

> "Which philosophy wins for collaboration primitives shapes whether the next era of productivity software is open or captured."

### The Independent Layer

> "Regardless of whether the labs solve this, there's room for an independent layer."

## Every Workaround Is a Spec

When power tools get adopted by users they weren't designed for, the workarounds ARE the product insight:

- Non-engineers using GitHub for markdown files
- Copy-pasting between terminals and Google Docs
- Teaching colleagues what "commit" means so they can collaborate on a memo
- Pointing Claude Code at a product repo to keep marketing messaging in sync with engineering

> "Within two hours of posting about this collaboration gap, three separate teams messaged me saying they're building the fix: a portfolio company with a working prototype, a startup that pivoted from data infrastructure, a founder building in stealth."

## Implications

### For Agent Infrastructure
The harness architecture needs to evolve from single-agent to multi-agent:

| Layer | Single-Player | Multiplayer (Missing) |
|-------|---------------|----------------------|
| Memory | claude-mem, MEMORY.md | Shared agent memory |
| Context | Personal context window | Team-level context |
| Versioning | Git (code-native) | Non-code versioning |
| Provenance | N/A | Who wrote what (human vs agent) |
| Permissions | Personal access | Context boundary control |

### For Enterprise Adoption
The Ramp playbook works for single-player adoption. Multiplayer is the next frontier:

> "Single-player = Claude eats your lunch (already happening in code). Multiplayer = the real moat. Orchestration, governance, shared memory across humans + agents. No single model owns that."

## Related

- harness-architecture — The memory/retrieval layer that needs multi-agent evolution
- claude-mem-architecture — Solves single-player memory, not shared memory
- enterprise-ai-adoption-playbook — Shows what's possible with single-player at scale
