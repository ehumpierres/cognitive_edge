# Agent Economy Primitives: Infrastructure for AI Coworkers

**Source:** Industry observation (Twitter/X compilation)  
**Date:** 2026-04-12  
**Tags:** #infrastructure #agents #economy #primitives #tooling #startups

---

## TL;DR

Companies are building primitives for an economy where **AI agents are the primary users**, not humans. Email, phone, browser, memory, payments, voice — each capability that makes humans functional in the digital economy is being rebuilt as agent-accessible infrastructure. Stitch them together and you get a digital coworker that looks more human than AI.

---

## The Thesis

> "Lots of companies are now building primitives for an economy where AI agents are the primary users instead of humans. They're betting on an economy of AI coworkers."

The pattern: every capability humans use to operate in the digital economy needs an agent-native equivalent.

---

## The Primitives Map

### Communication

| Capability | Human Version | Agent Primitive | Company |
|------------|---------------|-----------------|---------|
| Email | Gmail, Outlook | Agent email accounts | [AgentMail](https://agentmail.to) |
| Phone | Phone number | Agent phone numbers | [AgentPhone](https://agentphone.ai) |
| WhatsApp | Personal WhatsApp | Agent WhatsApp numbers | [Kapso](https://kapso.ai) |
| Voice | Human voice | Synthetic voice | [ElevenLabs](https://elevenlabs.io), [Vapi](https://vapi.ai) |

### Compute & Environment

| Capability | Human Version | Agent Primitive | Company |
|------------|---------------|-----------------|---------|
| Computer | Laptop, desktop | Sandboxed environments | [Daytona](https://daytona.io), [E2B](https://e2b.dev) |
| Browser | Chrome, Firefox | Headless browser control | [Browserbase](https://browserbase.com), [Browser Use](https://browser-use.com), [Hyperbrowser](https://hyperbrowser.ai) |
| Web crawling | Manual browsing | Structured web extraction | [Firecrawl](https://firecrawl.dev) |

### Memory & Knowledge

| Capability | Human Version | Agent Primitive | Company |
|------------|---------------|-----------------|---------|
| Memory | Human memory | Persistent agent memory | [Mem0](https://mem0.ai) |
| Web search | Google | Agent-optimized search | [Exa](https://exa.ai) |
| People/company search | LinkedIn, ZoomInfo | Agent-accessible data | [Sixtyfour](https://sixtyfour.ai) |

### Actions & Integration

| Capability | Human Version | Agent Primitive | Company |
|------------|---------------|-----------------|---------|
| SaaS tools | Manual login + use | Tool orchestration | [Composio](https://composio.dev) |
| API access | Developer integration | Simplified API layer | [Orthogonal](https://orthogonal.sh) |
| Payments | Credit card, bank | Agent payment rails | [Kite](https://gokite.ai), [Sponge](https://paysponge.com) |

---

## Key Insight

> "If you stitch all of these together, you get a digital coworker that looks more human than AI."

**The composition pattern:**
```
Agent + Email + Phone + Browser + Memory + Payments + Voice + Tools
= Digital coworker with human-equivalent capabilities
```

Each primitive solves one capability gap. Together they enable agents to:
- Communicate (email, phone, WhatsApp, voice)
- Operate (computer, browser, web)
- Remember (persistent memory)
- Discover (search web, people, companies)
- Act (use SaaS tools, call APIs)
- Transact (make payments)

---

## Connections

- **To MCP Connectors:** Composio, Orthogonal are doing at startup scale what MCP Connectors do for Claude — abstracting tool access
- **To Anthropic sandbox architecture:** Daytona, E2B are the "hands" in brain/hands decoupling — cattle execution environments
- **To Karpathy knowledge bases:** Mem0 is productized agent memory; same pattern of durable context outside the model
- **To Vercel deployment:** These primitives enable the autonomous agent workflows that need self-driving deployment infrastructure

---

## The Bet

These companies are betting that:
1. Agents will be primary users of digital infrastructure, not edge cases
2. Human-oriented UX (web forms, OAuth flows, visual interfaces) doesn't work for agents
3. Each human capability needs an agent-native API equivalent
4. The composition of primitives creates emergent capability

**Implication for builders:** If you're building agent infrastructure, think "what does a human use to do X?" then build the agent-native version.

---

## Notable Quote

> "Google doesn't work for agents."

Search engines are optimized for humans scanning results. Agents need structured, programmatic access to web knowledge.

---

## Landscape Categories

1. **Identity & Communication** — AgentMail, AgentPhone, Kapso, ElevenLabs, Vapi
2. **Compute & Environment** — Daytona, E2B, Browserbase, Browser Use, Hyperbrowser
3. **Knowledge & Memory** — Mem0, Exa, Sixtyfour, Firecrawl
4. **Action & Integration** — Composio, Orthogonal, Kite, Sponge

---

## Actionable Takeaways

1. When building agents, compose from these primitives rather than building custom integrations
2. Evaluate agent capability gaps: can it email? call? browse? pay? remember?
3. Watch for consolidation — some categories (browser, compute) may commoditize
4. The payment primitives (Kite, Sponge) unlock autonomous economic agents
