# AI Agent Debugging Methodology

> Source: Practitioner lessons from production debugging (April 2026)

## The Problem

Brute-force debugging with AI agents is catastrophically wasteful. Developers report spending "billions of tokens" on debugging sessions that could have been solved in minutes with structured methodology.

## The Breakthrough

Adding one instruction changed everything:

> "Write all assumptions and evidence to DEBUG.md."

This forces the AI to:
1. Externalize reasoning into a persistent file
2. Survive context compression
3. Create retrievable signal instead of lost noise

## The 4 Rules

### Rule 1: List Hypotheses Before Changing Code

Never start modifying code without first documenting your hypotheses. This forces structured thinking and prevents thrashing.

```markdown
## DEBUG.md

### Hypotheses
1. Race condition in async handler
2. State not being reset between calls
3. Cache invalidation timing issue
4. Event listener not being cleaned up
5. Promise resolution order dependency
```

### Rule 2: Maximum 5 Lines Per Experiment

Keep changes small. Small surface area = clear signal on what worked and what didn't.

- ✅ Change one function call
- ✅ Add one log statement
- ✅ Modify one condition
- ❌ Refactor entire module
- ❌ Change multiple systems at once

### Rule 3: Write All Evidence to Files

Prevents context compression from destroying the reasoning chain.

```markdown
### Evidence Log

#### Hypothesis 1: Race condition
- [x] Added timing logs → events fire in expected order
- [x] Added mutex → problem persists
- **RULED OUT** — not a race condition

#### Hypothesis 2: State reset
- [ ] Testing...
```

### Rule 4: Fail Twice in Same Direction → Force Switch

If you've tried two experiments on the same hypothesis without progress, **force yourself to switch to a different hypothesis**. This prevents perseverating on dead ends.

## Why This Works

Connects directly to harness architecture principles:

| Methodology | Harness Principle |
|-------------|-------------------|
| Written hypotheses | Targeted fragments |
| File-based evidence | Persistent memory (survives compression) |
| Small experiments | Clear signal, minimal noise |
| Forced switching | Prevents conflicting/stale fragments |

## The ROI

> "The tokens wasted on brute-forcing before were 1000× more than those used to finally fix the bug."

Structured debugging isn't just better — it's orders of magnitude more efficient.

## Implementation

This methodology can be encoded as an agent skill:

```yaml
name: structured-debugging
triggers:
  - "debug"
  - "investigate"
  - "troubleshoot"
instructions: |
  Before making any code changes:
  1. Create or update DEBUG.md with numbered hypotheses
  2. For each experiment, document:
     - Which hypothesis you're testing
     - What you changed (max 5 lines)
     - What you observed
     - Whether it's ruled out or confirmed
  3. If 2 experiments fail on same hypothesis, switch
  4. Never delete evidence — only append
```

## Related

- harness-architecture — Why file-based evidence survives context compression
- claude-mem-architecture — How memory retrieval prevents reasoning loss
