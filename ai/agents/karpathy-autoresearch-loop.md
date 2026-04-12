# Karpathy AutoResearch: Autonomous AI Research Agents

**Source**: [karpathy/autoresearch](https://github.com/karpathy/autoresearch) (via DataCamp tutorial)  
**Date**: 2026-04-12  
**Topic**: Autonomous AI agents running overnight research experiments on ML training code

---

## Core Concept

Give an AI agent a small but real LLM training setup and let it experiment **autonomously overnight**. The agent:
1. Modifies the code
2. Trains for 5 minutes
3. Checks if the result improved
4. Keeps or discards
5. Repeats

> "You wake up in the morning to a log of experiments and (hopefully) a better model."

The key insight: **You're not touching Python files like you normally would as a researcher. Instead, you are programming the `program.md` Markdown files** that provide context to AI agents and set up your autonomous research org.

---

## Architecture

The repo is deliberately kept minimal — three files:

| File | Purpose | Modified By |
|------|---------|-------------|
| `prepare.py` | Fixed constants, data prep, evaluation | Nobody (read-only) |
| `train.py` | Model, optimizer, training loop | **Agent** |
| `program.md` | Agent instructions (the "skill") | **Human** |

**Design constraint:** Training runs for a **fixed 5-minute time budget** (wall clock). The metric is `val_bpb` (validation bits per byte) — lower is better, vocab-size-independent so architectural changes are fairly compared.

---

## The Experiment Loop

From `program.md`:

```
LOOP FOREVER:

1. Look at the git state: current branch/commit
2. Tune train.py with an experimental idea by directly hacking the code
3. git commit
4. Run: uv run train.py > run.log 2>&1
5. Read results: grep "^val_bpb:\|^peak_vram_mb:" run.log
6. If grep output empty → crashed. Read tail -n 50 run.log, attempt fix
7. Record results in results.tsv (untracked)
8. If val_bpb improved → "advance" the branch, keep commit
9. If val_bpb equal or worse → git reset back to start
```

**Key rule: NEVER STOP**
> "Once the experiment loop has begun, do NOT pause to ask the human if you should continue. Do NOT ask 'should I keep going?' The human might be asleep. You are autonomous. If you run out of ideas, think harder — read papers referenced in the code, re-read files for new angles, try combining previous near-misses, try more radical architectural changes. The loop runs until the human interrupts you, period."

**Expected throughput:** ~12 experiments/hour → ~100 experiments overnight while human sleeps.

---

## Key Design Principles

### 1. Fixed Time Budget = Fair Comparison

Every experiment runs exactly 5 minutes regardless of what changes. This means:
- Experiments are directly comparable (model size, batch size, architecture all compete on equal footing)
- AutoResearch finds the most optimal model for YOUR platform in that time budget
- Trade-off: Results aren't comparable across different compute platforms

### 2. Single File Modification

Agent only touches `train.py`. This keeps:
- Scope manageable
- Diffs reviewable
- Context window clean

### 3. Simplicity Criterion

> "All else being equal, simpler is better. A small improvement that adds ugly complexity is not worth it. A 0.001 val_bpb improvement that adds 20 lines of hacky code? Probably not worth it. A 0.001 val_bpb improvement from deleting code? Definitely keep. An improvement of ~0 but much simpler code? Keep."

This is a **meta-instruction** that shapes agent judgment beyond raw metrics.

### 4. Git-Based Version Control

Uses branches for experiments:
- `git checkout -b autoresearch/<tag>` for fresh runs
- Keep = advance branch
- Discard = `git reset` to previous commit
- Never commit `results.tsv` (untracked log)

### 5. Structured Logging

`results.tsv` format (tab-separated):
```
commit	val_bpb	memory_gb	status	description
a1b2c3d	0.997900	44.0	keep	baseline
b2c3d4e	0.993200	44.2	keep	increase LR to 0.04
c3d4e5f	1.005000	44.0	discard	switch to GeLU activation
d4e5f6g	0.000000	0.0	crash	double model width (OOM)
```

---

## The `program.md` Pattern

This is essentially a **lightweight skill file** for agent instructions:

1. **Setup phase** — One-time initialization (branch, read files, verify data, create results.tsv)
2. **Constraints** — What CAN and CANNOT be modified
3. **Goal** — Single clear metric (lowest val_bpb)
4. **Loop** — Explicit procedural instructions for the experiment cycle
5. **Meta-rules** — Simplicity criterion, timeout handling, crash handling, NEVER STOP

The human iterates on `program.md` to find the "research org code" that achieves fastest research progress.

---

## Insights for Agent Design

### Autonomy via Explicit Permission
The "NEVER STOP" instruction is remarkable — it explicitly grants autonomy by removing the default "check in with human" pattern. Most agents ask for confirmation; this one is told NOT to.

### Metric-Driven Decisions
Keep/discard is purely metric-driven (val_bpb improved or not). No human judgment in the loop. The simplicity criterion adds a secondary heuristic but still objective.

### Bounded Scope = Success
Single file, single metric, fixed time budget. The constraints are what make autonomy tractable.

### Logging as Memory
`results.tsv` and git history serve as external memory. Agent can reference past experiments to avoid repeating failures and combine near-misses.

### Crash Handling as Judgment
> "If it's something dumb and easy to fix (e.g. a typo, a missing import), fix it and re-run. If the idea itself is fundamentally broken, just skip it, log 'crash' as the status, and move on."

This requires agent judgment about what's fixable vs. fundamentally broken.

---

## Relation to Other Patterns

- **Skill graphs**: `program.md` is a single-file skill, not a graph. Intentionally minimal.
- **Harness memory ownership**: Git + results.tsv = external memory the agent owns
- **File-based coordination**: Results logged to file, not API calls
- **/loop pattern**: Similar to Claude Code's `/loop` but fully autonomous with explicit "never stop"
- **Heartbeat self-healing**: Crash handling is built into the loop (attempt fix, or log and move on)

---

## Quotable

> "You're not touching any of the Python files like you normally would as a researcher. Instead, you are programming the `program.md` Markdown files that provide context to the AI agents and set up your autonomous research org."

> "The loop runs until the human interrupts you, period."

> "A 0.001 val_bpb improvement from deleting code? Definitely keep."

---

## Vision

> "One day, frontier AI research used to be done by meat computers in between eating, sleeping, having other fun, and synchronizing once in a while using sound wave interconnect in the ritual of 'group meeting'. That era is long gone. Research is now entirely the domain of autonomous swarms of AI agents running across compute cluster megastructures in the skies."

—@karpathy, March 2026
