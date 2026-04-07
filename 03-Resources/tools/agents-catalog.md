---
type: reference
date: 2026-04-07
project: harness
tags: [agents, automation, catalog]
---

# Agents Catalog

> Every agent in `.claude/agents/`. Claude spawns these via the Agent tool for specialized sub-tasks. Each agent has a focused context and dedicated toolset.
> **Agents path:** `.claude/agents/`

---

## How Agents Work

```
Main Claude context → Agent(subagent_type="agent-name", prompt="task") → Isolated sub-context
→ Agent uses its own tools → Returns result to main context
```

**When to spawn an agent:** Task requires 10+ tool calls, OR independent research stream, OR would pollute main context with noise.

---

## Marketing & Research Agents

| Agent | `subagent_type` | Best for |
|---|---|---|
| `competitor-analyst` | `competitor-analyst` | Deep competitor research: scraping, positioning analysis, ad creative spy, pricing research. Has access to Playwright + WebSearch. |
| `seo-writer` | `seo-writer` | Full SEO page creation: NeuronWriter research → write → score → push to WordPress as draft. Runs NeuronWriter automatically before writing. |
| `researcher` | `researcher` | General research + exploration. Read files, grep codebase, web fetch/search. Returns structured summaries. Use to protect main context from research noise. |

---

## GSD Framework Agents (Phase-Based Execution)

These agents are part of the GSD (Get Stuff Done) workflow system. Each handles one phase.

| Agent | `subagent_type` | Phase |
|---|---|---|
| `gsd-planner` | `gsd-planner` | Creates executable phase plans with task breakdown and dependency analysis. |
| `gsd-plan-checker` | `gsd-plan-checker` | Verifies plan will achieve the phase goal before execution starts. |
| `gsd-executor` | `gsd-executor` | Executes GSD plans with atomic commits, deviation handling, checkpoint protocols. |
| `gsd-verifier` | `gsd-verifier` | Verifies phase goal achievement (goal-backward analysis). Creates VERIFICATION.md. |
| `gsd-phase-researcher` | `gsd-phase-researcher` | Researches how to implement a phase. Produces RESEARCH.md for gsd-planner. |
| `gsd-project-researcher` | `gsd-project-researcher` | Researches domain ecosystem before roadmap creation. |
| `gsd-roadmapper` | `gsd-roadmapper` | Creates project roadmaps with phase breakdown and success criteria. |
| `gsd-debugger` | `gsd-debugger` | Investigates bugs using scientific method. Manages debug sessions. |
| `gsd-integration-checker` | `gsd-integration-checker` | Verifies cross-phase integration and E2E flows. |
| `gsd-nyquist-auditor` | `gsd-nyquist-auditor` | Fills validation gaps — generates tests and verifies coverage. |

---

## UI/Design Agents

| Agent | `subagent_type` | What it does |
|---|---|---|
| `gsd-ui-researcher` | `gsd-ui-researcher` | Produces UI-SPEC.md design contract for frontend phases. |
| `gsd-ui-auditor` | `gsd-ui-auditor` | Retroactive 6-pillar visual audit of implemented frontend code. Produces UI-REVIEW.md. |
| `gsd-ui-checker` | `gsd-ui-checker` | Validates UI-SPEC.md design contracts. BLOCK/FLAG/PASS verdicts. |

---

## Analysis & Advisory Agents

| Agent | `subagent_type` | What it does |
|---|---|---|
| `gsd-assumptions-analyzer` | `gsd-assumptions-analyzer` | Deeply analyzes codebase for a phase and returns structured assumptions with evidence. |
| `gsd-advisor-researcher` | `gsd-advisor-researcher` | Researches a single gray area decision. Returns structured comparison table with rationale. |
| `gsd-user-profiler` | `gsd-user-profiler` | Analyzes session messages across 8 behavioral dimensions to produce developer profile. |
| `gsd-research-synthesizer` | `gsd-research-synthesizer` | Synthesizes research outputs from parallel researcher agents into SUMMARY.md. |
| `gsd-codebase-mapper` | `gsd-codebase-mapper` | Explores codebase and writes structured analysis documents (tech, arch, quality, concerns). |
| `rule-advisor` | `rule-advisor` | Pre-task rule selector. Returns minimal set of CLAUDE.md rules relevant to a task. |

---

## Agent Selection Guide

```
Need to research a competitor?           → competitor-analyst
Need to write + publish an SEO page?     → seo-writer
Need deep research without polluting context? → researcher
Planning a multi-phase project?          → gsd-planner + gsd-plan-checker
Executing a GSD plan?                    → gsd-executor
Debugging a bug?                         → gsd-debugger
Verifying phase completion?              → gsd-verifier
Parallel research streams?               → multiple researcher agents (run_in_background=True)
```

---

## Parallel Agent Pattern

For tasks with 3+ independent research streams:
```python
# Wave 1: Parallel research
Agent(subagent_type="researcher", run_in_background=True, name="stream-1", prompt="research X")
Agent(subagent_type="researcher", run_in_background=True, name="stream-2", prompt="research Y")
Agent(subagent_type="researcher", run_in_background=True, name="stream-3", prompt="research Z")

# Wave 2: Main thread synthesizes after all complete
# Each stream commits its findings to knowledge/_stream_N.md
# Main thread reads all 3, synthesizes into final output
```
