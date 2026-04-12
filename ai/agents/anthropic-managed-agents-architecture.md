# Managed Agents: Decoupling the Brain from the Hands

**Source:** [Anthropic Engineering](https://www.anthropic.com/engineering/managed-agents)  
**Authors:** Lance Martin, Gabe Cemaj, Michael Cohen  
**Date:** 2026-04-12  
**Tags:** #anthropic #agents #architecture #harness #infrastructure #scaling

---

## TL;DR

Harnesses encode assumptions that go stale as models improve. Managed Agents virtualizes agent components—session, harness, sandbox—into stable interfaces that outlast any implementation. The key insight: **decouple the brain (Claude + harness) from the hands (sandboxes + tools)**. Each becomes `execute(name, input) → string`. Containers become cattle, not pets. Sessions live outside context windows. This enables many brains, many hands, and 60-90% TTFT improvements.

---

## Key Insights

### 1. Harnesses Encode Stale Assumptions

> "Harnesses encode assumptions about what Claude can't do on its own. However, those assumptions need to be frequently questioned because they can go stale as models improve."

**Example:** Sonnet 4.5 had "context anxiety" — wrapping up tasks prematurely near context limits. They added context resets to the harness. But Opus 4.5 didn't have this behavior. The resets became dead weight.

**Implication:** Design for harness evolution. Today's workaround is tomorrow's cruft.

### 2. Virtualize Agent Components

Inspired by OS design: operating systems virtualized hardware into abstractions (process, file) general enough for programs that didn't exist yet. The abstractions outlasted the hardware.

**Managed Agents virtualizes:**
- **Session** — append-only log of everything that happened
- **Harness** — loop that calls Claude, routes tool calls
- **Sandbox** — execution environment for code/file edits

Each can be swapped without disturbing others. Opinionated about interfaces, not implementations.

### 3. Don't Adopt a Pet

Initial design: all components in single container. Problem: the container became a "pet" — a named, hand-tended individual you can't afford to lose.

**Pet symptoms:**
- Container failure = session lost
- Unresponsive container = must nurse back to health
- Debugging required shell access to container with user data
- Harness assumed resources lived in same container

**Cattle solution:** Make components independently replaceable.

### 4. Decouple Brain from Hands

**Brain** = Claude + harness  
**Hands** = sandboxes + tools  
**Session** = event log (separate from both)

**The interface:**
```
execute(name, input) → string
provision({resources})
wake(sessionId)
getSession(id)
emitEvent(id, event)
```

**Benefits:**
- Container dies → harness catches tool-call error → Claude can retry with fresh container
- Harness crashes → new harness calls `wake(sessionId)`, resumes from last event
- No nursing failed containers back to health

### 5. Session ≠ Context Window

> "Long-horizon tasks often exceed the length of Claude's context window, and the standard ways to address this all involve irreversible decisions about what to keep."

**The problem:** Compaction, trimming, and memory tools all make irreversible decisions. Hard to know which tokens future turns need.

**The solution:** Session as durable context object outside the context window.

```
getEvents() — select positional slices of event stream
```

- Pick up from last read position
- Rewind before a specific moment
- Reread context before specific action

Transforms happen in harness before passing to Claude. Session guarantees durability; harness handles context engineering.

### 6. Security: Credentials Never in Sandbox

In coupled design, untrusted Claude-generated code ran alongside credentials. Prompt injection could read environment tokens.

**Structural fixes:**
- **Git:** Clone repo with access token during init, wire into local remote. Push/pull work without agent handling token.
- **MCP tools:** OAuth tokens in secure vault. Claude calls via proxy that fetches credentials. Harness never sees credentials.

### 7. Many Brains, Many Hands

**Many brains:** Containers provisioned via tool call only when needed. Inference starts immediately from event log.
- p50 TTFT dropped ~60%
- p95 TTFT dropped >90%

**Many hands:** Each hand is just `execute(name, input) → string`. Harness doesn't know if sandbox is a container, phone, or Pokémon emulator. Brains can pass hands to one another.

---

## The Interface Model

```
┌─────────────────────────────────────────────────────┐
│                    SESSION LOG                       │
│   (append-only, durable, getEvents() interface)      │
└─────────────────────────────────────────────────────┘
                         │
                    emitEvent()
                         │
┌─────────────────────────────────────────────────────┐
│                      HARNESS                         │
│  (stateless, restartable via wake(sessionId))        │
│  - Context engineering                               │
│  - Tool routing                                      │
│  - Error handling                                    │
└─────────────────────────────────────────────────────┘
                         │
              execute(name, input) → string
                         │
┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│ SANDBOX  │  │   MCP    │  │  CUSTOM  │  │   ...    │
│(cattle)  │  │  TOOLS   │  │  TOOLS   │  │          │
└──────────┘  └──────────┘  └──────────┘  └──────────┘
```

---

## Connections

- **To prior harness work:** This supersedes the earlier multi-agent harness doc — that was about harness patterns, this is about harness infrastructure
- **To Karpathy knowledge bases:** Session log is like wiki — append-only, durable, queryable context outside the working window
- **To /loop and Agent Teams:** These features rely on exactly this architecture — stateless harness, durable session, replaceable sandboxes

---

## Notable Quotes

> "The container became a pet... if a container failed, the session was lost."

> "The harness doesn't know whether the sandbox is a container, a phone, or a Pokémon emulator."

> "We designed the interfaces so that these can be run reliably and securely over long time horizons. But we make no assumptions about the number or location of brains or hands."

---

## Anti-Patterns Identified

- Coupling harness + sandbox in one container (creates pets)
- Credentials accessible from untrusted code execution
- Context window as only memory (irreversible losses)
- Assuming resources live alongside harness
- Provisioning containers before they're needed

---

## Actionable Takeaways

1. Design harnesses expecting they'll be replaced
2. Separate: session (durable log) / harness (stateless loop) / sandbox (cattle)
3. Use `execute(name, input) → string` as universal tool interface
4. Keep credentials in vault, never in sandbox
5. Provision resources lazily via tool calls
6. Store all events durably outside context window
