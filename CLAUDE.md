# Vault Constitution — Claude Code Operating Manual

> Read this at the start of every vault-touching session. Written for the model, not for humans.

## What This Vault Is

Rahul's second brain for GrowReach (LinkedIn automation SaaS) and Claude Code harness engineering.
Vault = knowledge persistence layer. Repo (`ClaudeCode2.0`) = operational files, code, scripts.

## Key Paths — Find Things Fast

| What you need | Where to look |
|---------------|--------------|
| Project context (GrowReach) | `01-Projects/growreach/index.md` |
| Project context (Harness) | `01-Projects/harness-engineering/index.md` |
| Competitor research | `02-Areas/competitors/` |
| SEO strategy | `02-Areas/seo/` |
| SaaS frameworks | `02-Areas/saas-frameworks/` |
| Hooks + automation | `02-Areas/hooks-and-automation/` |
| Tools catalog | `03-Resources/tools/` |
| Session digests | `99-Meta/session-digests/` |
| Session logs (auto) | `99-Meta/session-logs/` |
| Raw inbox (unprocessed) | `00-INBOX/` |
| Binary files (images, PDFs) | `_attachments/` — do NOT index these |
| Dashboard (Dataview) | `99-Meta/dashboard.md` |

## Navigation Rules

- **VAULT-INDEX.md** at root = master navigation map. Read it first, then drill down.
- **Index files** in each area/project = mini-maps. Read before scanning files.
- **Flatten where possible** — don't create deep nesting. Tokens burn on path resolution.
- **Backlinks** are the real value — a note mentioning another note forms a relationship Claude can traverse.

## File Naming Convention

Name files like sentences: `How-LinkedIn-comment-automation-scales.md` not `linkedin-notes.md`
Claude reads filenames as context signals before reading content. Make them count.

## Frontmatter Standard (every note should have this)

```yaml
---
type: meeting|project|fact|session|daily|research|competitor
date: YYYY-MM-DD
project: growreach|harness-engineering|meta
status: active|on-hold|archived
tags: [tag1, tag2]
---
```

## Immutable Rules

- NEVER overwrite a note without reading it first
- NEVER delete notes — move to `04-Archive/` instead
- ALWAYS add frontmatter when creating new notes
- Raw captures go to `00-INBOX/` — Claude processes them, never discards
- Binary files (PDFs, images) go to `_attachments/` only

## Active Context

> User updates this section before each session. Claude reads it to know what matters right now.

Current focus: GrowReach marketing — SEO comparison pages + competitor analysis
Pending: Playwright CLI upgrade, Obsidian CLI install, QMD semantic search install
Open threads: 5 competitor analyses (linkedhelper, dripify, engage-ai, commenter.ai, socialsonic)

## Memory Model (how this vault maps to Claude's memory types)

| Memory Type | This Vault | Purpose |
|-------------|-----------|---------|
| Working Memory | LLM context window | Immediate task |
| Episodic Memory | `99-Meta/session-digests/` | What we worked on, when |
| Semantic Memory | `02-Areas/` + `03-Resources/` | Stable facts and knowledge |
| Procedural Memory | `ClaudeCode2.0/CLAUDE.md` | Operating rules |
| Identity Layer | This file (vault constitution) | How to navigate + behave in vault |

## Startup Checklist (run mentally every session)

1. Read `VAULT-INDEX.md` — know the current state
2. Check `99-Meta/session-logs/` for last session summary
3. Check `00-INBOX/` — anything needing processing?
4. Check `Active Context` above — what's the focus?
