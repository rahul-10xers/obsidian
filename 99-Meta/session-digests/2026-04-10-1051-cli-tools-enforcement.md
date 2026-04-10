---
type: session-digest
date: 2026-04-10
project: harness-engineering
status: archived
tags: [playwright, qmd, vault-graph, claude-rules, token-efficiency]
---
# Session — 2026-04-10 10:51

## What we worked on

Completed the three CLI tool upgrades from the prior session (Playwright aria mode, QMD semantic search, vault-graph backlink traversal) and hardened the Claude Code setup so these tools are enforced by rule, not just documented. The session closed with explicit enforcement gates in CLAUDE.md to prevent Claude from drifting back to expensive defaults (screenshots, grep-over-vault, inline multi-page scraping).

## Key decisions made

- **Obsidian desktop CLI is Mac-only** — can't be called from VPS. Decision: implement full backlink graph traversal natively as `vault-graph.js` using wikilink regex. Replicates all 6 Obsidian CLI graph commands.
- **QMD-first is now a hard rule** (Rule #1) — vault search must go through `qmd query` before any grep/glob. Grep on vault files is explicitly forbidden as a first move.
- **Screenshot mode is now banned by rule** (Rule #4) — `--mode screenshot` explicitly prohibited with "90K token penalty" reason. Hierarchy: aria → text → WebFetch → WebSearch.
- **5+ page analysis → researcher subagent** (Rule #4) — inline multi-page scraping burns the main context window. Subagent batches the work and returns a summary. This fires automatically for ConnectSafely 24-page analysis.
- **resolveSlug quiet=true** — internal graph build calls must return null on ambiguous wikilinks, not crash. Fixed in `cmdOrphans` and `cmdRank`.

## Files created/modified

- `scripts/vault-graph.js` *(new)* — VPS-native Obsidian backlink graph (6 commands: backlinks, outlinks, related, orphans, tags, rank)
- `.claude/commands/vault-graph.md` *(new)* — `/vault-graph` skill file with command reference and examples
- `CLAUDE.md` — Rule #1 updated (QMD-first + vault-graph enforcement), Rule #4 rewritten (aria hierarchy, screenshot ban, subagent rule)
- `MEMORY.md` — added vault-graph routing entry
- `~/workspace/obsidian-vault/VAULT-INDEX.md` — Obsidian CLI thread marked `[x]` complete, replaced by vault-graph.js note
- `~/workspace/obsidian-vault/03-Resources/tools/new-cli-tools-2026.md` — corrected Obsidian CLI status, QMD and Playwright marked done
- `~/workspace/obsidian-vault/CLAUDE.md` — added QMD search guidance to navigation rules

## QMD status at end of session

- `qmd embed` completed: **437 chunks from 60 documents** (22m 46s, GGUF model 328MB downloaded and cached)
- First `qmd query` triggered one-time CPU compile of LLM query expansion model (running in background, cached after)
- `qmd search` (BM25) works instantly with no compile needed

## Open threads (carry forward)

- [ ] ConnectSafely 24-page analysis — rule enforcement now in place, ready to run when user gives the go-ahead
- [ ] 5 competitor analyses: linkedhelper, dripify (full), engage-ai, commenter.ai, socialsonic
- [ ] SEO comparison pages: NW score 70+ gate

## Next session should start with

Run the ConnectSafely analysis using the new subagent + aria-mode pipeline — this will be the first real test of the enforcement rules under load.
