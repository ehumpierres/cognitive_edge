# AGENTS.md Outperforms Skills in Agent Evals

**Source:** [Vercel Engineering](https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals)  
**Date:** 2026-01-27  
**Tags:** #agents #evals #skills #context #retrieval #next-js #vercel

---

## TL;DR

Vercel tested skills vs AGENTS.md for teaching agents Next.js 16 APIs. **AGENTS.md achieved 100% pass rate. Skills maxed at 79% with explicit instructions, and 53% without (same as baseline).** Skills weren't triggered in 56% of cases — agents had access but chose not to use them. Passive context beats active retrieval because there's no decision point, no ordering issues, and consistent availability every turn.

---

## The Results

| Configuration | Pass Rate | vs Baseline |
|---------------|-----------|-------------|
| Baseline (no docs) | 53% | — |
| Skill (default behavior) | 53% | +0pp |
| Skill with explicit instructions | 79% | +26pp |
| **AGENTS.md docs index** | **100%** | **+47pp** |

Detailed breakdown (Build/Lint/Test):

| Configuration | Build | Lint | Test |
|---------------|-------|------|------|
| Baseline | 84% | 95% | 63% |
| Skill (default) | 84% | 89% | 58% |
| Skill (explicit instructions) | 95% | 100% | 84% |
| **AGENTS.md** | **100%** | **100%** | **100%** |

---

## Key Findings

### 1. Skills Weren't Triggered Reliably

> "In 56% of eval cases, the skill was never invoked. The agent had access to the documentation but didn't use it."

Adding a skill produced zero improvement over baseline. The skill existed, the agent could use it, and the agent chose not to.

> "Agents not reliably using available tools is a known limitation of current models."

### 2. Explicit Instructions Helped, But Wording Was Fragile

Adding "explore project first, then invoke skill" to AGENTS.md improved trigger rate to 95%+ and pass rate to 79%.

**But wording mattered dramatically:**

| Instruction | Behavior | Outcome |
|-------------|----------|---------|
| "You MUST invoke the skill" | Reads docs first, anchors on doc patterns | Misses project context |
| "Explore project first, then invoke skill" | Builds mental model first, uses docs as reference | Better results |

> "Same skill. Same docs. Different outcomes based on subtle wording changes."

One eval: "invoke first" wrote correct `page.tsx` but missed required `next.config.ts` changes. "Explore first" got both.

> "If small wording tweaks produce large behavioral swings, the approach feels brittle for production use."

### 3. Why Passive Context Beats Active Retrieval

Three factors:

1. **No decision point.** With AGENTS.md, there's no moment where the agent must decide "should I look this up?" The information is already present.

2. **Consistent availability.** Skills load asynchronously and only when invoked. AGENTS.md content is in the system prompt for every turn.

3. **No ordering issues.** Skills create sequencing decisions (read docs first vs. explore project first). Passive context avoids this entirely.

### 4. Compression Works

Initial docs: ~40KB → Compressed to **8KB** (80% reduction) with same 100% pass rate.

Format: Pipe-delimited index pointing to retrievable files:
```
[Next.js Docs Index]|root: ./.next-docs
|IMPORTANT: Prefer retrieval-led reasoning over pre-training-led reasoning
|01-app/01-getting-started:{01-installation.mdx,02-project-structure.mdx,...}
|01-app/02-building-your-application/01-routing:{01-defining-routes.mdx,...}
```

Agent knows WHERE to find docs without full content in context. Reads specific files when needed.

---

## The Key Instruction

```markdown
IMPORTANT: Prefer retrieval-led reasoning over pre-training-led reasoning
for any Next.js tasks.
```

This tells the agent to consult docs rather than rely on potentially outdated training data.

---

## Skills vs AGENTS.md: When to Use Each

| Approach | Best For |
|----------|----------|
| **AGENTS.md** | Broad, horizontal improvements across all tasks |
| **Skills** | Vertical, action-specific workflows users explicitly trigger ("upgrade Next.js", "migrate to App Router") |

> "Skills aren't useless... The two approaches complement each other."

---

## Recommendations for Framework Authors

1. **Don't wait for skills to improve.** Results matter now, gap may close later.

2. **Compress aggressively.** Index pointing to retrievable files works as well as full docs.

3. **Test with evals.** Target APIs not in training data — that's where doc access matters.

4. **Design for retrieval.** Structure docs so agents can find and read specific files.

5. **Provide AGENTS.md snippets.** If you maintain a framework, give users a snippet to add to their projects.

---

## The Goal

> "Shift agents from pre-training-led reasoning to retrieval-led reasoning. AGENTS.md turns out to be the most reliable way to make that happen."

---

## Connections

- **To .claude/ folder anatomy:** AGENTS.md < 200 lines guidance applies here too. But a compressed index can be effective.
- **To Harrison Chase memory ownership:** AGENTS.md IS owned context — files you control, not locked in a provider.
- **To Ramp Glass:** "Product is the enablement" — AGENTS.md passively teaches agents, no decision required.
- **To autonomous agent teams:** File-based context (AGENTS.md, SOUL.md) consistently beats complex orchestration.

---

## Notable Quotes

> "The 'dumb' approach (a static markdown file) outperformed the more sophisticated skill-based retrieval."

> "If small wording tweaks produce large behavioral swings, the approach feels brittle for production use."

> "There's no moment where the agent must decide 'should I look this up?' The information is already present."

---

## Actionable Takeaways

1. **Prefer passive context (AGENTS.md) for framework knowledge** — skills for explicit workflows
2. **Compress docs to index + retrievable files** — 8KB can achieve 100% pass rate
3. **Include "prefer retrieval-led reasoning" instruction** — shift from training data to docs
4. **Test on APIs NOT in training data** — that's where it matters
5. **Avoid fragile skill triggering** — wording sensitivity is a production risk
