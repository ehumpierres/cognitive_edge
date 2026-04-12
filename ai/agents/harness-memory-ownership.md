# Harness Memory Ownership: Why Open Harnesses Matter

**Source:** Harrison Chase (LangChain)  
**Date:** 2026-04-12  
**Tags:** #harness #memory #lock-in #open-source #enterprise #ownership

---

## TL;DR

Agent harnesses are not going away — they're how agents interact with tools and data. Memory isn't a plugin; it's the harness. **If you use a closed harness, especially behind an API, you don't own your memory.** Model providers are incentivized to lock memory behind APIs because memory creates stickiness that models alone don't provide. For enterprise control and portability: open harnesses, open memory.

---

## Key Insights

### 1. Harnesses Are Not Going Away

> "There is sometimes sentiment that models will absorb more and more of the scaffolding. This is not true."

**What actually happens:** Old scaffolding becomes unnecessary, but new scaffolding replaces it. An agent, by definition, is an LLM interacting with tools and data. There will always be a system around the LLM to facilitate that.

**Evidence:** When Claude Code's source code leaked, there were **512k lines of code**. That code IS the harness. Even makers of the best models invest heavily in harnesses.

**Even "built-in" features are harnesses:** When web search is built into OpenAI/Anthropic APIs, it's not "part of the model" — it's a lightweight harness behind their APIs orchestrating model + search via tool calling.

### 2. Memory Isn't a Plugin — It's the Harness

Citing Sarah Wooders (Letta/MemGPT):

> "Asking to plug memory into an agent harness is like asking to plug driving into a car. Managing context, and therefore memory, is a core capability and responsibility of the agent harness."

**Memory is context.** The harness handles:
- **Short-term memory:** Messages in conversation, tool call results
- **Long-term memory:** Cross-session memory, updated and read by harness

**Harness decisions that ARE memory decisions:**
- How is AGENTS.md / CLAUDE.md loaded into context?
- How is skill metadata shown? (system prompt? system messages?)
- Can the agent modify its own system instructions?
- What survives compaction, and what's lost?
- Are interactions stored and made queryable?
- How is memory metadata presented to the agent?
- How is filesystem information exposed?

### 3. If You Don't Own Your Harness, You Don't Own Your Memory

**Levels of lock-in (mild to severe):**

| Level | Example | Problem |
|-------|---------|---------|
| **Mildly bad** | Stateful APIs (OpenAI Responses API, Anthropic server-side compaction) | State stored on their servers. Can't swap models and resume threads. |
| **Bad** | Closed harness (Claude Agent SDK → Claude Code) | Harness interacts with memory in unknown ways. Artifacts may exist client-side, but shape/usage is unknown and non-transferable. |
| **Worst** | Entire harness + long-term memory behind API | Zero ownership or visibility into memory. Don't know the harness (can't use memory). Don't own the memory. No control over what's exposed. |

**Example of worst case:** Anthropic Managed Agents — literally everything behind an API, locked to their platform.

**Even "open" can have lock-in:** Codex is open source, but generates encrypted compaction summaries not usable outside OpenAI ecosystem.

### 4. Memory Creates Lock-In That Models Don't

> "Without memory, your agents are easily replicable by anyone who has access to the same tools. With memory, you build up a proprietary dataset."

**Why memory matters:**
- Agents get better as users interact with them
- Data flywheel effect
- Personalization that molds to user desires and patterns
- Differentiated, increasingly intelligent experiences

**Why switching is easy today:** Model APIs are similar/identical. Prompts need minor tweaks, but that's manageable.

**Why switching becomes hard:** As soon as there's state. Memory matters. If you switch, you lose access to it.

**Personal story from author:** Email assistant got deleted accidentally. Tried to recreate from same template — experience was much worse. Had to reteach preferences, tone, everything. "Made me realize how powerful and sticky memory could be."

### 5. Model Providers Are Incentivized to Lock You In

> "When people say that the 'models will absorb more and more of the harness' — this is what they really mean. They mean that these memory related parts will go behind the APIs that model providers offer."

**The incentive:** Memory creates lock-in they don't get from just the model.

**It's already happening:**
- Anthropic: Claude Managed Agents (everything behind API)
- OpenAI: Codex with encrypted compaction (not portable)

---

## The Solution: Open Memory, Open Harnesses

**Requirements for memory ownership:**
- Open source harness
- Model agnostic
- Open standards (agents.md, skills)
- Pluggable storage (Mongo, Postgres, Redis)
- Self-hostable, deployable anywhere
- Bring your own database for memory store

> "In order to own your memory, you need to be using an Open Harness."

---

## Connections

- **To Anthropic Managed Agents:** This is the counter-argument. Managed Agents virtualizes everything behind APIs — convenient, but you cede memory ownership.
- **To Enterprise Readiness gaps:** This directly addresses compliance/auditability concerns. If you don't own memory, you can't audit it or guarantee retention/deletion.
- **To Karpathy knowledge bases:** Same principle — knowledge should live in files you control, not trapped in a platform.

---

## Notable Quotes

> "Memory isn't a plugin (it's the harness)." — Sarah Wooders

> "Even the makers of the best model in the world are investing heavily in harnesses."

> "This is incredibly alarming — it means that memory will become locked into a single platform, a single model."

> "Without memory, your agents are easily replicable by anyone who has access to the same tools."

---

## Enterprise Implications

| Concern | Open Harness | Closed/API Harness |
|---------|--------------|-------------------|
| **Data ownership** | You own memory data | Provider owns/controls |
| **Portability** | Switch models, keep memory | Locked to provider |
| **Compliance** | Control retention, deletion, audit | Dependent on provider policies |
| **Customization** | Full control over memory format | Limited to exposed APIs |
| **Vendor risk** | Mitigated | High |

---

## Harness Landscape

**Open harnesses:**
- Deep Agents (LangChain)
- OpenCode
- Letta Code

**Closed/partial lock-in:**
- Claude Code (open source, but ecosystem dependencies)
- Codex (open source, but encrypted compaction)
- Claude Agent SDK → Claude Code (closed)
- Claude Managed Agents (fully closed, API-only)

---

## Actionable Takeaways

1. **Audit your current stack:** Where does agent memory live? Who controls it?
2. **Prefer open harnesses** for enterprise deployments
3. **Own your storage:** Pluggable databases (Postgres, Mongo) over provider-managed state
4. **Avoid stateful provider APIs** if portability matters
5. **Watch for encrypted/proprietary formats** even in "open" tools
6. **Memory is your moat** — don't cede it to a platform
