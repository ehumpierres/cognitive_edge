# Long-Running Agent Patterns: 13 Days Straight

> Source: Practitioner notes (April 2026)

## Overview

Patterns for getting coding agents to work autonomously for extended periods (13+ days) without human intervention.

## The 4 Patterns

### 1. Self-Verification

The agent needs to be able to end-to-end verify everything itself.

**Implementation:**
- Design efficient testing layers that make sense for your project
- Agent must be able to loop on tests to prove correctness
- Tests should be fast enough to run frequently
- Cover the full stack: unit → integration → e2e

**Why it works:** Without self-verification, the agent either halts waiting for human confirmation or accumulates errors that compound. Self-verification keeps the loop closed.

### 2. Write Spec Documents

Work with the agent to fully specify goals, implementation details, and verification in a document.

**Implementation:**
- Iterate on the spec multiple times before starting
- Include: goals, full implementation details, verification criteria
- Location doesn't matter: Notion page, simple .md file
- The spec becomes the source of truth the agent references

**Why it works:** Ambiguity causes thrashing. A well-specified doc gives the agent a stable target. When confused, it returns to the spec rather than guessing.

### 3. Use a Running To-Do List

Break down complex work into a to-do list that you can see and edit.

**Implementation:**
- Visible to both human and agent
- Editable by both — you can add items as you think of them
- Location doesn't matter: Notion database, .md file
- Agent checks off completed items, adds new ones as discovered

**Why it works:** Complex work reveals sub-tasks. A running list captures them without losing context. Human can steer by adding/reprioritizing without interrupting flow.

### 4. Adversarial Review

As a step in the process, the agent should ask another agent to review the spec and implementation.

**Implementation:**
- Fresh agent context reviews for gaps
- Force the agent to loop until totally aligned
- Can use sub-agents if supported
- Or design simple CLI that calls another agent
- **Key: invoke a fresh agent context** (not same session)

**Why it works:** Same-session review has blind spots — the agent "knows" things it didn't write down. Fresh context catches gaps in documentation and implementation that the original agent can't see.

## The Pattern Stack

```
┌─────────────────────────────────────────┐
│  SPEC DOCUMENT                          │
│  Goals + Implementation + Verification  │
└─────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────┐
│  RUNNING TO-DO LIST                     │
│  Visible, editable, evolving            │
└─────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────┐
│  IMPLEMENTATION LOOP                    │
│  Build → Self-verify → Check off        │
└─────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────┐
│  ADVERSARIAL REVIEW                     │
│  Fresh context catches blind spots      │
└─────────────────────────────────────────┘
                    │
                    ▼
              [Next task]
```

## Scope Considerations

> "This was for a prototype of a new product. It's easier to do all of this on a small codebase, but the principles work anywhere."

**Small codebase advantages:**
- Faster test cycles
- Less context to maintain
- Easier to fully specify
- Adversarial review can cover everything

**Scaling up:**
- Modular specs (one per component)
- Hierarchical to-do lists
- Scoped self-verification (test affected areas)
- Targeted adversarial review (changed components only)

## Connection to Other Patterns

| Pattern | This Article | Related Work |
|---------|--------------|--------------|
| Self-verification | End-to-end test loops | Karpathy AutoResearch: bounded scope enables autonomy |
| Spec documents | .md or Notion page | Debugging methodology: write hypotheses before changing code |
| To-do list | Running, visible, editable | Harness architecture: externalize to files, survives context |
| Adversarial review | Fresh agent context | Agent teams: one agent one job, file-based coordination |

## Key Insight

The common thread: **externalize everything**.
- Spec → externalized goals
- To-do list → externalized state
- Tests → externalized verification
- Adversarial review → externalized critique

When everything is externalized, the agent can run indefinitely without losing context or accumulating drift.

## Related

- debugging-methodology — 4 rules for structured debugging (same externalization principle)
- karpathy-autoresearch-loop — Autonomous research with bounded scope
- autonomous-agent-team-setup — Multi-agent coordination patterns
