# Anatomy of the .claude/ Folder

**Source:** [Akshay (@akshay_pachaar)](https://x.com/akshay_pachaar) — X Article  
**Date:** 2026-03-21  
**Tags:** #claude-code #configuration #skills #agents #permissions #harness #practical

---

## TL;DR

The `.claude/` folder is the control center for how Claude behaves in your project — instructions, custom commands, permission rules, and memory. There are actually **two** folders: project-level (committed, team config) and `~/.claude/` (personal, machine-local). CLAUDE.md is the highest-leverage file — get it right first, everything else is optimization.

---

## Key Insights

### 1. Two Folders, Not One

| Folder | Purpose | Committed? |
|--------|---------|------------|
| `.claude/` (project) | Team configuration — same rules, commands, permissions for everyone | Yes |
| `~/.claude/` (home) | Personal preferences, session history, auto-memory | No |

### 2. CLAUDE.md: The Instruction Manual

> "Whatever you write in CLAUDE.md, Claude will follow."

First thing Claude reads at session start. Loaded into system prompt, kept in mind for entire conversation.

**Locations (all combined):**
- Project root `CLAUDE.md` — most common
- `~/.claude/CLAUDE.md` — global, all projects
- Subdirectory `CLAUDE.md` — folder-specific rules

**What to write:**
- Build, test, lint commands
- Key architectural decisions
- Non-obvious gotchas
- Import conventions, naming patterns, error handling
- File/folder structure for main modules

**What NOT to write:**
- Anything that belongs in linter/formatter config
- Full documentation you can link to
- Long theoretical paragraphs

> "Keep CLAUDE.md under 200 lines. Longer files eat too much context and instruction adherence drops."

**Example (20 lines, complete):**
```markdown
# Project: Acme API
## Commands
npm run dev    # Start dev server
npm run test   # Run tests (Jest)
npm run lint   # ESLint + Prettier check

## Architecture
- Express REST API, Node 20
- PostgreSQL via Prisma ORM
- Handlers in src/handlers/, types in src/types/

## Conventions
- Use zod for request validation
- Return shape always { data, error }
- Never expose stack traces to client
- Use logger module, not console.log

## Watch out for
- Tests use real local DB. Run `npm run db:test:reset` first
- Strict TypeScript: no unused imports
```

### 3. CLAUDE.local.md: Personal Overrides

Personal preferences, not team-wide. Gitignored automatically. Claude reads alongside main CLAUDE.md.

### 4. The rules/ Folder: Modular Instructions

When CLAUDE.md gets crowded, split by concern:

```
.claude/rules/
├── code-style.md
├── testing.md
├── api-conventions.md
└── security.md
```

**Path-scoped rules** — only load when working with matching files:

```markdown
---
paths:
  - "src/api/**/*.ts"
  - "src/handlers/**/*.ts"
---
# API Design Rules
- All handlers return { data, error } shape
- Use zod for request body validation
```

Rules without `paths` field load unconditionally.

### 5. Hooks: Deterministic Control

> "CLAUDE.md instructions are suggestions. Hooks are deterministic."

Shell scripts that fire at specific points. Exit codes matter:
- **Exit 0** — success
- **Exit 1** — error but non-blocking (common mistake for security!)
- **Exit 2** — STOP EVERYTHING, send stderr to Claude for self-correction

**Events:**
- `PreToolUse` — before any tool runs (security gate)
- `PostToolUse` — after tool succeeds (formatters, linters)
- `Stop` — when Claude finishes (quality gates)
- `UserPromptSubmit` — when you press enter
- `Notification` — desktop alerts
- `SessionStart/SessionEnd` — context injection/cleanup

**Example config (settings.json):**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit|MultiEdit",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
      }]
    }],
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/bash-firewall.sh"
      }]
    }]
  }
}
```

**Gotchas:**
- Hooks don't hot-reload mid-session
- PostToolUse can't undo (use PreToolUse to prevent)
- Hooks fire recursively for subagent actions
- Full user permissions, no sandboxing — validate inputs!

### 6. The skills/ Folder: Auto-Invoked Workflows

Skills watch the conversation and act when context matches the description.

```
.claude/skills/
├── security-review/
│   ├── SKILL.md
│   └── DETAILED_GUIDE.md
└── deploy/
    ├── SKILL.md
    └── templates/
```

**SKILL.md format:**
```markdown
---
name: security-review
description: Use when reviewing code for vulnerabilities, before deployments, or when user mentions security.
allowed-tools: Read, Grep, Glob
---
Analyze the codebase for security vulnerabilities...
Reference @DETAILED_GUIDE.md for our security standards.
```

**Key difference from commands:** Skills can bundle supporting files. Commands are single files. Skills are packages.

### 7. The agents/ Folder: Specialized Subagent Personas

For complex tasks benefiting from a dedicated specialist:

```markdown
---
name: code-reviewer
description: Use PROACTIVELY when reviewing PRs or validating implementations.
model: sonnet
tools: Read, Grep, Glob
---
You are a senior code reviewer...
```

- Spawns in isolated context window
- Compresses findings and reports back
- `tools` field restricts capabilities (security auditor doesn't need Write)
- `model` field can use cheaper model for focused tasks

### 8. settings.json: Permissions

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git status)",
      "Bash(git diff *)",
      "Read", "Write", "Edit"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(curl *)",
      "Read(./.env)",
      "Read(./.env.*)"
    ]
  }
}
```

- **allow** — runs without confirmation
- **deny** — blocked entirely
- **Neither** — Claude asks first (intentional safety net)

---

## The Full Structure

```
your-project/
├── CLAUDE.md                    # Team instructions (committed)
├── CLAUDE.local.md              # Personal overrides (gitignored)
│
└── .claude/
    ├── settings.json            # Permissions, hooks (committed)
    ├── settings.local.json      # Personal permissions (gitignored)
    │
    ├── hooks/
    │   ├── bash-firewall.sh     # PreToolUse: block dangerous
    │   ├── auto-format.sh       # PostToolUse: format files
    │   └── enforce-tests.sh     # Stop: tests must pass
    │
    ├── rules/                   # Modular instructions
    │   ├── code-style.md
    │   ├── testing.md
    │   └── api-conventions.md
    │
    ├── skills/                  # Auto-invoked workflows
    │   └── security-review/
    │       └── SKILL.md
    │
    └── agents/                  # Subagent personas
        ├── code-reviewer.md
        └── security-auditor.md

~/.claude/
├── CLAUDE.md                    # Global instructions
├── settings.json                # Global settings + hooks
├── skills/                      # Personal skills (all projects)
├── agents/                      # Personal agents (all projects)
└── projects/                    # Session history + auto-memory
```

---

## Getting Started (5 Steps)

1. **Run `/init`** — generates starter CLAUDE.md by reading project. Edit down to essentials.
2. **Add settings.json** — allow run commands, deny .env reads
3. **Create 1-2 commands** — code review, issue fixing
4. **Split into rules/** — when CLAUDE.md gets crowded, scope by path
5. **Add ~/.claude/CLAUDE.md** — personal preferences across all projects

> "That's genuinely all you need for 95% of projects."

---

## Connections

- **To Ramp Glass skills:** Same pattern — skills as reusable, shareable workflows. Dojo = marketplace for .claude/skills/
- **To Harrison Chase memory:** The ~/.claude/projects/ folder IS the memory store. Auto-memory persists across sessions.
- **To Vercel executable guardrails:** Hooks ARE executable guardrails — deterministic, not suggestions
- **To Anthropic permissions:** settings.json allow/deny is the implementation of permission-by-policy

---

## Notable Quotes

> "CLAUDE.md instructions are suggestions. Hooks make behaviors deterministic."

> "Keep CLAUDE.md under 200 lines. Longer files eat too much context and instruction adherence drops."

> "Skills can bundle supporting files. Commands are single files. Skills are packages."

> "The .claude folder is really a protocol for telling Claude who you are."

---

## Actionable Takeaways

1. **CLAUDE.md < 200 lines** — instruction adherence drops with length
2. **Use rules/ with path scoping** — load API rules only when editing API files
3. **Exit code 2, not 1** for blocking hooks
4. **Skills > commands** when you need supporting files
5. **Restrict agent tools** — security auditor doesn't need Write
6. **settings.local.json** for personal permissions (gitignored)
7. **~/.claude/CLAUDE.md** for cross-project preferences
