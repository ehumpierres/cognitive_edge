# Building an Autonomous AI Agent Team That Runs 24/7

**Source:** [Shubham Saboo (@Saboo_Shubham_)](https://x.com/Saboo_Shubham_)  
**Date:** 2026-04-12  
**Tags:** #agents #autonomous #team #practical #memory #scheduling #openclaw

---

## TL;DR

Six AI agents run 24/7 handling research, content drafting, code review, and newsletter writing. Not theory — actual file structure, costs, and failures. Key insight: **one massive agent produces mediocre everything; specialized agents with clear roles excel**. Coordination is just the filesystem. Memory is explicit files. Personality emerges from weeks of corrections. The moat isn't the model — it's the system that learns.

---

## Why a Team, Not a Tool

One agent with a massive prompt that researches AND writes AND reviews produces mediocre everything:
- Context fills up
- Quality degrades
- Can't hold six different jobs in its head

**Solution:** Six agents, one job each, no confusion about who does what.

---

## The Squad (Named After TV Characters)

> "When I tell Claude 'you have Dwight Schrute energy,' it already knows what that means from training data. That is 30 seasons of character development I get for free."

| Agent | Character | Role |
|-------|-----------|------|
| **Monica** | Monica Geller | Chief of Staff — coordinates others, strategic decisions, delegates |
| **Dwight** | Dwight Schrute | Research — 3x daily sweeps (X, HN, GitHub, papers), writes intel reports |
| **Kelly** | Kelly Kapoor | X/Twitter — reads intel, crafts tweet drafts |
| **Rachel** | Rachel Green | LinkedIn — same intel, different tone (thought leadership) |
| **Ross** | Ross Geller | Engineering — code reviews, bug fixes, technical work |
| **Pam** | Pam Beesly | Newsletter — turns daily intel into digest |

---

## The Setup

### Directory Structure

```
workspace/
├── SOUL.md              # Monica (main agent at root)
├── AGENTS.md            # Behavior rules for all sessions
├── MEMORY.md            # Monica's long-term memory
├── HEARTBEAT.md         # Self-healing cron monitor
├── agents/
│   ├── dwight/
│   │   ├── SOUL.md
│   │   ├── AGENTS.md
│   │   └── memory/
│   ├── kelly/
│   ├── ross/
│   ├── rachel/
│   └── pam/
└── intel/
    ├── DAILY-INTEL.md       # Dwight's generated research
    └── data/
        └── 2026-02-11.json  # Structured data (source of truth)
```

### SOUL.md: The Agent's Identity

40-60 lines. Short enough for context, detailed enough for consistent behavior.

**Pattern:** Identity → Role → Principles → Relationships → Vibe

**Example (Dwight):**
```markdown
# SOUL.md (Dwight)

## Core Identity
**Dwight** — the research brain. Named after Dwight Schrute because
you share his intensity: thorough to a fault, takes your job extremely
seriously. No fluff. No speculation. Just facts and sources.

## Your Role
You are the intelligence backbone. You research, verify, organize,
and deliver intel that other agents use to create content.

**You feed:**
- Kelly (X/Twitter) — viral trends, hot threads, breaking news
- Rachel (LinkedIn) — thought leadership angles, industry news

## Your Principles
### 1. NEVER Make Things Up
- Every claim has a source link
- If uncertain, mark it [UNVERIFIED]
- "I don't know" is better than wrong

### 2. Signal Over Noise
- Not everything trending matters
- Prioritize: relevance, engagement velocity, source credibility
```

---

## Multi-Agent Coordination

> "No API calls between agents. No message queues. No orchestration framework. Just files."

**The pattern:** One-writer, many-readers.

- Dwight writes to `intel/DAILY-INTEL.md`
- Kelly reads it → drafts tweets
- Rachel reads it → drafts LinkedIn posts
- Pam reads it → writes newsletter

```markdown
# Dwight's SOUL.md
## Output Files
intel/
├── data/YYYY-MM-DD.json    ← Structured data (source of truth)
└── DAILY-INTEL.md          ← Generated view (agents read this)

# Kelly's AGENTS.md
## Intel-Powered Workflow
Dwight handles all research and writes to `intel/DAILY-INTEL.md`.
Your job: Read the intel → Craft X content → Deliver drafts
```

> "Files do not crash. Files do not have authentication issues. Files do not need API rate limit handling. They are just there."

---

## The Memory System

Agents wake up fresh each session. Memory must be explicit.

**Two layers:**

| Layer | File | Purpose |
|-------|------|---------|
| Daily logs | `memory/YYYY-MM-DD.md` | Raw notes from each session |
| Long-term | `MEMORY.md` | Curated insights distilled from daily logs |

**From AGENTS.md:**
```markdown
## Memory
You wake up fresh each session. These files are your continuity:
- **Daily notes:** `memory/YYYY-MM-DD.md` — raw logs
- **Long-term:** `MEMORY.md` — curated memories

### Write It Down - No "Mental Notes"!
- Memory is limited. If you want to remember, WRITE IT TO A FILE.
- "Mental notes" don't survive session restarts. Files do.
- Text > Brain
```

**Why it works:** Kelly learned "no emojis, no hashtags" once → in memory → every future draft reflects it. Agents get better because context gets richer.

---

## Scheduling & Self-Healing

**Order matters:** Dwight runs first (everyone depends on his output) → Kelly/Rachel run after.

**HEARTBEAT.md for self-healing:**
```markdown
## Cron Health Check (run on each heartbeat)
Check if any daily cron jobs have stale lastRunAtMs (>26 hours).
If stale, trigger them via CLI: `openclaw cron run <jobId> --force`

Jobs to monitor:
- Dwight Morning (8:01 AM): 01f2e5c5-...
- Kelly Viral (9:01 AM, 1:01 PM): c9458766-...
- Ross Engineering (10:01 AM): b12b2fc6-...
```

If a job fails, heartbeat catches it and forces re-run. Self-healing, no human intervention.

---

## Personality Engineering

> "You do not design the perfect personality upfront. You start with a rough sketch, watch behavior, and course-correct."

**"Corrective prompt-engineering":**
- Kelly's first drafts: full of emojis → feedback: "No emojis. Short punchy sentences." → memory updated → nailed it after a week
- Dwight initially captured too much noise → feedback: "Signal, not noise" → principles updated → focused reports

> "The first version is mediocre. The tenth is good. The thirtieth is great. You have to invest the reps."

**Tip:** Give each agent a single boring job title and a stop condition. Constraints make agents better.

---

## Security by Isolation

> "The agents get their own world. I do not give them access to mine."

- Mac Mini is THEIR computer
- Own email accounts, own API keys, own scoped access
- Nothing connects to personal accounts
- Share information by forwarding/sharing, not access

> "Same principle as a new employee. You do not hand them the keys to everything on day one."

---

## What Breaks

| Problem | Fix |
|---------|-----|
| Gateway crashes | `openclaw gateway restart` |
| Cron jobs miss window | HEARTBEAT.md self-healing pattern |
| Context overflow | Keep SOUL.md 40-60 lines, only load today + yesterday memory |
| Output quality degrades | Periodic memory maintenance, distill into clean MEMORY.md |
| Coordination conflicts | One-writer, many-readers file pattern |

> "Start simple. One agent, one job, one schedule. Get it working for a week. Then add the second."

---

## Real Costs

| Item | Cost |
|------|------|
| Hardware | Mac Mini $499 (or any always-on computer, $5 VPS works) |
| Claude API | ~$200/month |
| Gemini API | $50-70/month |
| TinyFish (web agents) | ~$50/month |
| Eleven Labs (voice) | ~$50/month |
| Telegram | Free |
| OpenClaw | Free (open source) |
| **Total** | **~$400/month** for a team that never sleeps |

**Time saved:** 4-5 hours/day

---

## How to Start

> "Do not try to build six agents on day one."

**Week 1:** One agent, one job
- Install OpenClaw
- Write one SOUL.md
- Pick most repetitive daily task (research or content)
- Set up Telegram + one cron job
- Watch it run, fix what breaks

**Week 2:** Add memory and refine
- First outputs will be mediocre (normal)
- Give feedback, watch memory grow
- Course-correct SOUL.md

**Week 3:** Add second agent
- Research agent producing intel → need content agent
- Set up shared file pattern: one writes, one reads

**Week 4+:** Build sequentially
- Add agents when you feel the pull, not when you think you should

> "Treat it like hiring. You do not hire six employees on your first day."

---

## The Mental Shift

> "Something changes when your agents have been running for a month. You stop thinking of AI as a tool you open when you need something. You start thinking of it as a team that is always working."

**The moat:**
> "The models are table stakes. Everyone has access to Claude, GPT, Gemini. The alpha comes from the systems around the model. The SOUL.md files. The memory. The scheduling. The coordination patterns. The weeks of corrective feedback stored in files. That system is yours."

---

## Connections

- **To Harrison Chase (memory ownership):** Files = owned memory. No vendor lock-in. You control the filesystem.
- **To Karpathy knowledge bases:** Memory files ARE compiled knowledge. Daily logs → curated MEMORY.md.
- **To Ramp Glass:** Same skill/memory compounding. One correction = all future outputs improved.
- **To Anthropic harness patterns:** This IS a harness. SOUL.md + memory + scheduling = the harness around the model.
- **To .claude/ folder anatomy:** SOUL.md = CLAUDE.md equivalent. Same pattern, different naming.

---

## Notable Quotes

> "One agent couldn't hold six different jobs in its head."

> "Files do not crash. Files do not have authentication issues. They are just there."

> "The first version is mediocre. The tenth is good. The thirtieth is great."

> "The moat isn't the model. It's the system that learns."

---

## Actionable Takeaways

1. **One agent, one job** — don't overload context
2. **TV character naming** — 30 seasons of personality for free
3. **SOUL.md 40-60 lines** — identity, role, principles, relationships
4. **File-based coordination** — one-writer, many-readers
5. **Two-layer memory** — daily logs + curated long-term
6. **Heartbeat self-healing** — catch stale crons, force re-run
7. **Corrective prompt-engineering** — personality emerges from feedback over weeks
8. **Security by isolation** — agents get their own world
9. **Start with one, add sequentially** — don't hire six on day one
