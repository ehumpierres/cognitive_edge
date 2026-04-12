# Ramp Glass: Building Every Employee Their Own AI Coworker

**Source:** [Seb Goddijn (Ramp)](https://x.com/sebgoddijn) — X Article  
**Date:** 2026-04-09  
**Tags:** #enterprise #internal-tools #productivity #skills #memory #harness #adoption

---

## TL;DR

Ramp hit 99% AI tool adoption but people were stuck — terminal windows, npm installs, MCP configs were too much. They built "Glass," an internal AI productivity suite that makes every employee a power user without configuration pain. Key innovations: pre-connected integrations via SSO, a skill marketplace ("Dojo") with 350+ shared skills, persistent memory, scheduled automations, and a workspace (not chat) interface. The insight: **the product IS the enablement** — people who installed a skill on day one learned faster than those who attended training.

---

## Key Insights

### 1. The Models Are Good Enough, The Harness Isn't

> "Most people use them like driving a Ferrari with the handbrake on. Not because they aren't smart, or lack ambition, they've just never seen what a well-configured environment looks like."

99% adoption, but stuck. The bottleneck was configuration, not capability or ambition.

### 2. Three Core Principles

**Don't limit anyone's upside.**
> "The default approach for non-technical users is to simplify: put the product on rails, offer fewer options, and make it dummy-proof. We couldn't disagree more."

Power users need: multi-window workflows, deep integrations, scheduled automations, persistent memory, reusable skills. Goal: make complexity invisible while preserving full capability.

**One person's breakthrough becomes everyone's baseline.**
> "The biggest failure mode wasn't that people couldn't figure things out. It was that everyone had to figure things out alone."

A workflow discovered by one person didn't help anyone else. Glass compounds wins into organizational capability.

**The product is the enablement.**
> "No amount of workshops can match a targeted nudge while you're already doing the work."

People improve through repetition and experimentation. The product accelerates by suggesting the right skill at the right time.

### 3. Everything Connects on Day One

- SSO sign-in → all tools available with one-click setup
- Includes internal tools (Ramp Research, Ramp Inspect, Ramp CLI)
- Sales rep asks to pull Gong call → enrich with Salesforce → draft follow-up: **it just works**

> "This is the unsexy foundation that makes everything else possible."

### 4. Dojo: The Skill Marketplace

Skills = markdown files that teach the agent how to perform specific tasks.

**The flywheel:**
- Sales figures out best way to analyze Gong calls → packages as skill → every rep gets superpower
- CX engineer builds Zendesk investigation workflow → entire support team levels up overnight
- **350+ skills shared company-wide**
- Git-backed, versioned, reviewed like code

**Sensei:** AI guide that recommends skills based on:
- Which tools you've connected
- Your role
- What you've been working on

> "A new account manager doesn't need to browse 350 skills — the Sensei surfaces the five that matter most on day one."

### 5. Memory That Actually Works

**On first open:** Builds full memory system from authenticated connections:
- People you work with
- Active projects
- References to Slack channels, Notion docs, Linear tickets

**24-hour synthesis pipeline:** Mines previous sessions and connected tools (Slack, Notion, Calendar) for updates.

> "Glass can adapt to their world without them having to re-explain things every session."

### 6. Laptop as Server

**Scheduled automations:**
- Daily, weekly, or custom cron
- Post results directly to Slack
- Finance lead pulls spend anomalies at 8am daily with a prompt that takes minutes to set up

**Slack-native assistants:**
- Listen and respond in channels
- Use full Glass setup (integrations, memory, skills)
- Ops team built vendor policy Q&A bot in an afternoon

**Headless mode:**
- Kick off task, walk away
- Approve permission requests from phone
- Results waiting when you return

### 7. Workspace, Not Chat Window

> "Most AI products give you a single conversation thread. Glass gives you a full workspace."

- Split panes, tile multiple sessions side by side
- Documents, data files, code alongside conversations
- Drag tabs, split horizontally/vertically
- Renders markdown, HTML, CSVs, images, code with syntax highlighting
- **Layout persists across sessions**

> "Real work isn't linear."

### 8. Why Build vs Buy

**Internal productivity is a moat.**
> "Using AI well is now a core business need... That makes internal AI infrastructure part of your moat, and you do not hand your moat to a vendor."

**Speed.**
> "We can ship fixes the same day someone reports a problem... most resolved in hours. You cannot do that while waiting on a vendor's roadmap."

**Informs external product.**
> "Solving these problems internally gives us conviction about what works before we ship it. Glass gives us reps on the hardest AI product problems without those reps happening at customers' expense."

---

## The Key Learning

> "The people who got the most value weren't the ones who attended our training sessions. They were the ones who installed a skill on day one and immediately got a result. The product taught them faster than we ever could."

**Reframing:**
- Every feature is secretly a lesson
- Skills show great output before you know how to ask for it
- Memory shows context is the difference between generic and useful
- Self-healing integrations show errors aren't your fault

> "When you hand someone a tool that just works, they learn by doing. And they learn fast."

---

## The Philosophy

> "We don't believe in lowering the ceiling. We believe in raising the floor."

---

## Connections

- **To Harrison Chase (memory ownership):** Ramp owns their harness, owns their memory. Explicitly chose build over buy because "you do not hand your moat to a vendor."
- **To Karpathy knowledge bases:** Dojo is the organizational version — skills as compiled knowledge, shared and versioned.
- **To Vercel executable guardrails:** Skills ARE executable guardrails. Not documentation, runnable capabilities.
- **To enterprise readiness gaps:** This is an enterprise deployment pattern — SSO, integrations, compliance-ready by design.

---

## Notable Quotes

> "The models are good enough, the harness isn't."

> "The goal isn't to remove complexity, but to make it invisible while preserving full capability."

> "A workflow discovered by one person didn't help anyone else."

> "No amount of workshops can match a targeted nudge while you're already doing the work."

> "Using AI well is now a core business need... you do not hand your moat to a vendor."

---

## Enterprise Pattern: Glass Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    GLASS WORKSPACE                       │
│  (Split panes, persistent layout, multi-session)         │
└─────────────────────────────────────────────────────────┘
                           │
┌─────────────────────────────────────────────────────────┐
│                      DOJO (Skills)                       │
│  350+ shared skills, Git-backed, versioned, Sensei AI    │
└─────────────────────────────────────────────────────────┘
                           │
┌─────────────────────────────────────────────────────────┐
│                    MEMORY SYSTEM                         │
│  Built from connections, 24h synthesis pipeline          │
└─────────────────────────────────────────────────────────┘
                           │
┌─────────────────────────────────────────────────────────┐
│               PRE-CONNECTED INTEGRATIONS                 │
│  SSO → Gong, Salesforce, Slack, Notion, Snowflake...    │
└─────────────────────────────────────────────────────────┘
                           │
┌─────────────────────────────────────────────────────────┐
│                   AUTOMATION LAYER                       │
│  Scheduled tasks, Slack bots, headless mode              │
└─────────────────────────────────────────────────────────┘
```

---

## Actionable Takeaways

1. **Pre-connect everything via SSO** — day one should just work
2. **Build a skill marketplace** — one person's breakthrough = everyone's baseline
3. **AI-guided skill discovery** (Sensei pattern) — don't make people browse catalogs
4. **Memory from connections** — build context automatically from authenticated tools
5. **24-hour synthesis** — keep memory fresh without user effort
6. **Workspace > chat** — real work isn't linear, support split panes
7. **Product = enablement** — nudges beat training sessions
8. **Own the infrastructure** — internal productivity is a moat
