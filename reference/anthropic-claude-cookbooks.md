# Anthropic Claude Cookbooks — Master Reference

**Source**: [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks)  
**Date**: 2026-04-12  
**Type**: Official Anthropic reference implementation  
**Status**: 🔑 KEY SOURCE — Canonical patterns for building with Claude

---

## Overview

The Claude Cookbooks are Anthropic's official code examples and guides for building with Claude. This is the **canonical source** for implementation patterns, not blog posts or third-party interpretations.

> Copy-able code snippets that you can easily integrate into your own projects.

---

## Repository Structure

```
capabilities/        # Core Claude capabilities (RAG, classification, summarization)
patterns/agents/     # Building Effective Agents reference implementation
managed_agents/      # Claude Managed Agents tutorials (hosted runtime)
skills/              # Skills feature for document generation
extended_thinking/   # Extended reasoning patterns
tool_use/            # Tool use and integration patterns
multimodal/          # Vision and image processing
third_party/         # Pinecone, Voyage, Wikipedia integrations
misc/                # Batch processing, caching, utilities
coding/              # Code-related patterns
finetuning/          # Fine-tuning workflows
observability/       # Logging and monitoring
```

---

## Key Sections

### 1. Patterns: Building Effective Agents

**Location**: `patterns/agents/`

Reference implementation for the [Building Effective Agents](https://anthropic.com/research/building-effective-agents) blog post.

| Notebook | Pattern |
|----------|---------|
| `basic_workflows.ipynb` | Prompt chaining, routing, multi-LLM parallelization |
| `evaluator_optimizer.ipynb` | Evaluate → optimize loop |
| `orchestrator_workers.ipynb` | Orchestrator-subagents pattern |

### 2. Claude Managed Agents (CMA)

**Location**: `managed_agents/`

Anthropic's hosted runtime for stateful, tool-using agents. Sandboxed environments with persistent files, tool state, and conversation.

**Applied cookbooks:**
| Notebook | Use Case |
|----------|----------|
| `data_analyst_agent.ipynb` | CSV → narrative HTML report (pandas, plotly) |
| `slack_data_bot.ipynb` | Slack bot wrapping the data analyst |
| `sre_incident_responder.ipynb` | Pager alert → investigation → PR → human approval |

**Guided tutorials (progressive):**
| Notebook | Teaches |
|----------|---------|
| `CMA_iterate_fix_failing_tests.ipynb` | Entry point. Agent/environment/session, file mounts, streaming event loop |
| `CMA_orchestrate_issue_to_pr.ipynb` | Issue → fix → PR → CI → review → merge. Multi-turn steering, mid-chain recovery |
| `CMA_explore_unfamiliar_codebase.ipynb` | Grounding in unfamiliar code, planted stale-doc trap, `sessions.resources.add` |
| `CMA_gate_human_in_the_loop.ipynb` | HITL expense approval via custom tools. `requires_action` pattern |
| `CMA_prompt_versioning_and_rollback.ipynb` | Server-side prompt versioning, regression detection, rollback |
| `CMA_operate_in_production.ipynb` | MCP toolsets, vaults, webhooks, resource lifecycle |

### 3. Skills

**Location**: `skills/`

Organized packages of instructions, code, and resources that give Claude specialized capabilities.

| Notebook | Coverage |
|----------|----------|
| `01_skills_introduction.ipynb` | Architecture, Excel/PowerPoint/PDF creation |
| `02_skills_financial_applications.ipynb` | Financial dashboards, portfolio analysis, cross-format workflows |
| `03_skills_custom_development.ipynb` | Building custom skills, best practices |

**Built-in Skills:**
| Skill | ID | Description |
|-------|-----|-------------|
| Excel | `xlsx` | Workbooks with formulas, charts, formatting |
| PowerPoint | `pptx` | Presentations with slides, charts, transitions |
| PDF | `pdf` | Formatted documents with text, tables, images |
| Word | `docx` | Rich formatted documents |

**Custom Skill Structure:**
```
my_skill/
├── SKILL.md           # Instructions for Claude
├── scripts/           # Python/JS code
└── resources/         # Templates, data
```

### 4. Extended Thinking

**Location**: `extended_thinking/`

| Notebook | Coverage |
|----------|----------|
| `extended_thinking.ipynb` | Extended reasoning patterns |
| `extended_thinking_with_tool_use.ipynb` | Reasoning + tool use |

### 5. Core Capabilities

**Location**: `capabilities/`

- **Classification**: Text and data classification techniques
- **RAG**: Retrieval augmented generation
- **Summarization**: Effective summarization techniques

### 6. Tool Use

**Location**: `tool_use/`

| Notebook | Pattern |
|----------|---------|
| `customer_service_agent.ipynb` | Customer service with tools |
| `calculator_tool.ipynb` | Calculator integration |
| SQL queries | `misc/how_to_make_sql_queries.ipynb` |

### 7. Multimodal

**Location**: `multimodal/`

- Getting started with images
- Best practices for vision
- Interpreting charts and graphs
- Extracting content from forms
- Using sub-agents (Haiku as sub-agent with Opus)

### 8. Third-Party Integrations

**Location**: `third_party/`

- Pinecone (vector DB for RAG)
- Wikipedia search
- Voyage AI embeddings
- Web page reading

---

## Model References

From `CLAUDE.md`:

**Direct API:**
- Sonnet: `claude-sonnet-4-6`
- Haiku: `claude-haiku-4-5`
- Opus: `claude-opus-4-6`
- **Never use dated model IDs** (e.g., `claude-sonnet-4-6-20250514`)

**Bedrock:**
- Opus 4.6: `anthropic.claude-opus-4-6-v1`
- Sonnet 4.5: `anthropic.claude-sonnet-4-5-20250929-v1:0`
- Haiku 4.5: `anthropic.claude-haiku-4-5-20251001-v1:0`
- Prepend `global.` for global endpoints (recommended)

---

## Key API Patterns

### Skills Beta Headers
```python
client = Anthropic(
    api_key="your-api-key",
    default_headers={
        "anthropic-beta": "code-execution-2025-08-25,files-api-2025-04-14,skills-2025-10-02"
    }
)
```

### Files API (for skill-generated documents)
```python
# Download
content = client.beta.files.download(file_id="file_abc123...")
with open("output.xlsx", "wb") as f:
    f.write(content.read())

# Metadata
info = client.beta.files.retrieve_metadata(file_id="file_abc123...")

# List
files = client.beta.files.list()

# Delete
client.beta.files.delete(file_id="file_abc123...")
```

---

## Related Resources

- [Anthropic Developer Docs](https://docs.claude.com)
- [Building Effective Agents Blog](https://anthropic.com/research/building-effective-agents)
- [Skills Documentation](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
- [Equipping Agents with Skills (Engineering Blog)](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- [Managed Agents Architecture (Engineering Blog)](https://www.anthropic.com/engineering/managed-agents)
- [Claude API Fundamentals Course](https://github.com/anthropics/courses/tree/master/anthropic_api_fundamentals)

---

## Why This Matters

This is **the canonical implementation reference** for:
1. Agent patterns (orchestration, routing, evaluation)
2. Managed Agents (Anthropic's hosted runtime)
3. Skills (document generation, custom capabilities)
4. Tool use patterns
5. Extended thinking
6. Multimodal capabilities

When in doubt about "how Anthropic intends X to work", look here first.
