---
type: reference
date: 2026-04-07
project: harness
tags: [skills, commands, automation, catalog]
---

# Skills & Commands Catalog

> Every skill in `.claude/skills/` and command in `.claude/commands/`. Claude invokes these via the Skill tool. The skills-router.py hook auto-suggests the right one.
> **Commands path:** `.claude/commands/` | **Skills path:** `.claude/skills/`

---

## How Skills Work

```
User message → skills-router.py hook matches → Claude calls Skill tool with skill name
→ Skill file loaded into context → Claude follows the protocol defined in the skill
```

**Rule:** Never re-implement skill logic inline. If a skill exists for the task → use it.

---

## Orchestration Commands (high-level workflows)

| Command | Trigger | What it does |
|---|---|---|
| `/autonomous-task` | Open-ended tasks (research, build, write, analyze) | 7-phase orchestrator: skill audit → gap ID → acquisition → inventory → gap analysis → iterative loop → handoff. The meta-command for all non-trivial work. |
| `/autoresearch` | Iterative improvement tasks | Autoresearch loop: hypothesis → action → verify → commit → decide. Powers Phase 6 of autonomous-task. |
| `/digest` | End of any substantive session | Writes session summary to `obsidian-vault/99-Meta/session-digests/`. Updates VAULT-INDEX.md. Commits + pushes vault. |
| `/learn` | After `[LEARN:category]` tags found in session | Promotes inline learning tags to skill files. |
| `/resume` | Start of session after compaction | Reconstructs session state from git log + CRASH_BUFFER.md. |
| `/save-transcript` | Mid-session checkpoint | Saves conversation to `transcripts/` and pushes to GitHub. |

---

## GSD Commands (phase-based execution framework)

| Command | What it does |
|---|---|
| `gsd/` directory | GSD workflow commands: plan-phase, execute-phase, verify-phase, debug, ui-phase |

---

## Marketing Skills

### Research & Analysis

| Skill | Trigger phrases | What it does |
|---|---|---|
| `competitor-alternatives` | "competitor analysis", "analyze [tool]" | Research competitor positioning, pricing, features. Creates structured analysis. |
| `persona-discovery` | "find our ICP", "buyer persona" | Discovers ICP using Reddit, review sites, NotebookLM. Produces persona card. |
| `customer-persona` | "persona for [segment]" | Builds detailed customer persona profile. |
| `jtbd-framework.md` | "jobs to be done", "JTBD" | Applies Jobs-to-be-Done framework to product/market research. |
| `extraction-insights.md` | "extract insights from", "analyze this data" | Extracts structured insights from raw research data. |

### Content & Copy

| Skill | Trigger phrases | What it does |
|---|---|---|
| `copywriting` | "write copy for", "landing page copy" | Writes persuasive marketing copy. |
| `copy-editing` | "edit this copy", "improve this" | Edits and improves existing marketing copy. |
| `content-strategy` | "content plan", "content strategy" | Builds content strategy aligned with SEO + buyer journey. |
| `social-content` | "LinkedIn post", "social media content" | Creates social content (LinkedIn focus for GrowReach). |
| `email-sequence` | "email sequence", "drip campaign" | Writes email sequence for nurture/onboarding. |

### SEO

| Skill | Trigger phrases | What it does |
|---|---|---|
| `programmatic-seo` | "programmatic SEO", "PSEO", "bulk pages" | Plans + executes programmatic SEO page creation. |
| `seo-audit` | "SEO audit", "check SEO" | Audits page/site SEO against technical checklist. |
| `schema-markup` | "add schema", "FAQ schema", "structured data" | Adds JSON-LD schema markup (FAQ, Product, SoftwareApplication). |
| `free-tool-strategy` | "free tool", "lead magnet" | Plans free tool strategy for organic growth. |

### CRO & Product

| Skill | Trigger phrases | What it does |
|---|---|---|
| `page-cro` | "improve conversion", "CRO audit" | Conversion rate optimization for landing pages. |
| `form-cro` | "improve form", "signup form" | CRO focused on form optimization. |
| `onboarding-cro` | "onboarding flow", "activation" | Optimizes user onboarding for activation. |
| `paywall-upgrade-cro` | "upgrade flow", "paywall" | Optimizes paywall and upgrade conversion. |
| `popup-cro` | "popup", "exit intent" | Designs/optimizes popup for conversion. |
| `signup-flow-cro` | "signup flow", "registration" | Optimizes signup/registration flow. |
| `analytics-tracking` | "tracking", "analytics setup" | Sets up analytics event tracking. |

### Strategy & Positioning

| Skill | Trigger phrases | What it does |
|---|---|---|
| `pricing-strategy` | "pricing", "price our product" | Develops pricing strategy using frameworks. |
| `launch-strategy` | "launch plan", "go to market" | GTM launch planning. |
| `paid-ads` | "Google Ads", "paid campaign" | Plans + writes paid ad campaigns. |
| `ab-test-setup` | "A/B test", "split test" | Designs A/B tests with hypothesis + metrics. |
| `referral-program` | "referral", "refer a friend" | Designs referral program mechanics. |
| `marketing-ideas` | "marketing ideas", "growth ideas" | Brainstorms marketing tactics. |
| `marketing-psychology` | "psychology", "persuasion" | Applies marketing psychology principles. |

### Personas / Mentors

| Skill | Trigger phrases | What it does |
|---|---|---|
| `hormozi-mentor` | "Alex Hormozi", "Hormozi framework" | Applies Alex Hormozi's offer/value frameworks. |
| `hormozi-money-models` | "money model", "offer design" | Applies $100M Offers money model frameworks. |
| `natasha-marketer` | "Natasha", "brand voice" | GrowReach brand voice persona. |
| `product-marketing-context` | "GrowReach context", "product positioning" | Loads full GrowReach product marketing context. |

---

## Skill Selection Guide

```
Writing SEO page?                    → /autonomous-task (triggers seo-writer agent + NeuronWriter)
Competitor research?                 → competitor-alternatives skill
Understanding our buyers?            → persona-discovery skill
Writing LinkedIn content?            → social-content skill
Improving a landing page?            → page-cro skill
Open-ended multi-step task?          → /autonomous-task (always)
End of session?                      → /digest
```
