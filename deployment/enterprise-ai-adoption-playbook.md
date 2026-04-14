# Enterprise AI Adoption Playbook: The Ramp Model

> Source: Geoff Charles (@geoffintech), Ramp (April 2026)

## Results

| Metric | Result |
|--------|--------|
| AI usage growth | 6,300% YoY |
| Team active on AI tools | 99.5% |
| Using coding agents weekly | 84% |
| Apps shipped in 6 weeks | 1,500+ (from 800+ builders) |
| Non-engineer PRs | 12% of production codebase |

## The 8 Principles

### 1. The Second Best Time to Start Is Today

No formal change management. No mandatory training curriculum. Instead:
- Leadership clarity that AI usage is an expectation
- Dedicated AI "guild" responsive to any questions
- Slack channels for sharing what teams built
- Dedicated all hands time to celebrate builders
- Mandated AI usage and tracking for everyone

> "Teams just need to be given a chance. Everyone wants to build. And with AI, anyone can."

### 2. AI Proficiency as Learning Curve, Not Light Switch

| Level | Description | Expectation |
|-------|-------------|-------------|
| L0 | Sometimes uses ChatGPT. No workflow changes. | *"Will most likely not be at the company"* |
| L1 | Built custom GPTs, dabbled in Claude Code. Sees what's possible. | Starting point |
| L2 | Built an app that automates part of their job. Committed code. | *"Where things get real"* |
| L3 | Systems builders. Build infrastructure that levels up everyone else. | Force multipliers |

**Three things that move people up:**
1. Build tools that meet people where they are
2. Raise expectations as tools mature
3. Match the mandate to the tooling

### 3. Embrace Creative Destruction

Tools shipped in January 2026 already obsolete — replaced by better versions, often from the same builders. Shelf life of weeks, not months.

> "If your internal tools from three months ago still feel state-of-the-art, you're not moving boldly enough."

**Data democratization evolution:**
- Phase 1: Notion AI over Notion databases
- Phase 2: Slack-based Snowflake research tool
- Phase 3: Encoded Snowflake research into coding agent skills
- Phase 4: Interactive, self-improving data research

> "People aren't attached to their tools. They're attached to their problems."

### 4. Build from the Center, Drive from the Spokes

**Initial mistakes:**
- Centralized: One small team builds for whole company → demand outstripped capacity
- Decentralized: Every team builds their own → redundant re-learning

**The answer: Both**
- **Center**: Small team builds platforms, connectors, plumbing. Manages training/enablement.
- **Spokes**: Functional teams build on top, give feedback that drives central roadmap.

**Results from non-engineers:**
- Risk analyst: Automated 16 hours/month of financial modeling
- Sales ops lead: Replaced comp model across 3 orgs in 48 hours
- L&D lead: Built training simulator in 15 minutes
- Finance: Contract reviewer saving 45 min/contract

> "They didn't file a ticket. They found their own pain, prototyped a fix, and pulled engineering in when it was time to go to production."

### 5. Give People a Stage, Not Just a Mandate

- **#ramp-uses-ai Slack**: 1,000+ members, 40+ spin-off channels, 20K messages/month
- **AI office hours**: Every Friday, 40-50+ attendees
- **AI onboarding**: Rebuilt 4 times in the last year
- **Dedicated all hands**: CEO to first-line operators demo what they built

> "The biggest surprise wasn't who built the most. It was how many people had been waiting for permission to build at all."

### 6. Get People to the "Aha" Moment as Fast as Possible

Built **Glass** — their own Claude Cowork on Anthropic's Agent SDK:
- Auto-configures on install via Okta SSO
- 30+ tools light up instantly (Salesforce, Snowflake, Gong, Slack, Notion, Google, Figma)
- No setup guide, no IT ticket
- 700 DAU within a month of launch
- Built by team of 4 in under 3 months

Built **Dojo** — skills marketplace:
- 350+ skills shared company-wide
- Anyone can package a workflow and share it
- Sales rep figures out best Gong analysis → packages as skill → every rep has that superpower

> "Every skill shared raises the floor for everyone."

### 7. Make It a Competition

Internal leaderboard tracking:
- Sessions run
- Skills used
- Apps shipped
- Tools connected

**Three dynamics:**
1. **Healthy peer pressure**: Nobody wants to be at the bottom
2. **Manager accountability**: Team rankings make it impossible to ignore AI adoption
3. **Discovery through emulation**: Leaderboard is a map to what top performers are doing

**Extended to hiring:**
- Absolute requirement for AI proficiency
- PM candidates must build a working prototype in the interview
- No exceptions

### 8. Remove Every Constraint Between Your People and AI

- **Unlimited AI budget**: Treat as learning investment, not procurement
- **No token limits**: No caps, no tiered access, no "you're not an engineer"
- **Pre-connected tools**: 30+ integrations live on day one

**The cost math:**
> "We pay our employees a lot of money. Token consumption per employee today isn't even close to double-digit percentages of their salary. But if someone is 2x more productive with AI, you should be willing to spend their entire salary again in tokens."

## Key Infrastructure

### Glass (Internal Claude Cowork)
- Built on Anthropic's Claude Agent SDK
- SSO auth → 30+ tools auto-connected
- Team of 4, under 3 months to build
- 700 DAU within first month

### Dojo (Skills Marketplace)
- 350+ skills shared company-wide
- Anyone can package and share workflows
- Raises the floor for everyone

## The Meta-Insight

> "We didn't start with a better strategy than most companies... In lieu of a master plan, we just started. We kept building tools, kept raising the bar, kept investing in data and AI infrastructure, kept creating venues for people to show off. Each track compounded separately. As they reinforced each other, the curve went vertical."

## Related

- agent-operator-role — The emerging role that drives this transformation
- harness-architecture — The context/memory infrastructure underlying these tools
