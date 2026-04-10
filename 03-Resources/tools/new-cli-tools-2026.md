---
type: research
date: 2026-04-10
project: harness-engineering
status: active
tags: [cli, playwright, obsidian, tools, claude-code]
---
# New CLI Tools for Claude Code (2026)

> Mined from NotebookLM: "Mastering AI Agents: Advanced Claude Code Workflows and Tools"
> Key insight: CLI > MCP for almost every tool. CLIs are faster, cheaper, and more capable.

## Priority Upgrades (implement first)

### 1. Playwright CLI ← UPGRADE FROM MCP/SCREENSHOTS
**Status:** ✓ DONE — `--mode aria` added to `scripts/scrape.js`. `/playwright-cli` skill wired. Uses `locator.ariaSnapshot()` (Playwright 1.46+).
**Why upgrade now:** 90,000 fewer tokens vs MCP. Uses accessibility tree not screenshots. Spins up 5+ Chrome instances simultaneously. Free (pay tokens only).
**Current problem:** Our scrape.js uses Playwright but via screenshot path — much more expensive.

Install:
```bash
# The skill teaches Claude Code how to use it
# From Claude Code, just paste the GitHub URL and say "install this skill"
# Repo: https://github.com/microsoft/playwright (CLI docs)
# Or: npx playwright --help (already available)
```

Key commands Claude can use once skill is installed:
- Spin up Chrome: `npx playwright chromium --headed`
- Test a form: Tell Claude "open chrome, go to URL, test form submission"
- Multiple tabs: Claude can run 5 parallel instances automatically

### 2. Obsidian CLI ← NEW (enables backlink graph traversal)
**Status:** PENDING — requires Obsidian 1.12+ desktop app action (NOT an npm package).
**Why it matters:** Without this, Claude reads vault files as flat text. WITH this, Claude can traverse the backlink graph — see "this note is connected to these 5 other notes" and surface latent patterns across the vault.
**Correct install (one-time, done in Obsidian app):**
1. Open Obsidian desktop app → Settings → General
2. Scroll to "Command line interface" → Enable it
3. Follow on-screen instructions to add `obsidian` to system PATH
4. Verify: `obsidian --help` in terminal (should show 95 commands)

⚠️ The `obsidian-cli` npm package (v0.5.1, no repo/description) is NOT this — it's an unrelated package.

### 3. QMD — Semantic Search for Markdown ← NEW
**Status:** ✓ DONE — `npm install -g @tobilu/qmd` v2.1.0. Vault collection `obsidian-vault` indexed (60 files). Skill wired at `.claude/skills/qmd`. Run `qmd embed` to refresh vectors after adding notes.
**Why:** 60%+ token reduction vs grep/glob on vault. Uses BM25 + vector semantic + LLM re-ranking. Created by Shopify CEO Tobi Lutke. Runs 100% locally.
**Use:** `qmd query "your question"` — finds relevant vault notes semantically. No need to know which file to read.
**Package:** `@tobilu/qmd` (NOT the `qmd` placeholder on npm) | [github.com/tobi/qmd](https://github.com/tobi/qmd)

## Medium Priority

### 4. Firecrawl CLI
**What:** Anti-bot web scraping. Returns structured LLM-readable data.
**When to use:** When scripts/scrape.js fails on bot-protected sites.
**Status:** Not installed. Open source version available (no API key needed for basic use).

### 5. CodeEx Plugin (OpenAI Codex adversarial review)
**What:** Second-opinion code reviewer. Runs adversarial review of Claude's own code.
**Why:** Claude looks too favorably on its own code. CodeEx provides an external critic.
**Install:** From Claude Code: `/plugin` → marketplace → search "codeex"
**Requires:** OpenAI account (even free tier $7/mo works)

## Already Have (no action needed)

| Tool | Status |
|------|--------|
| NotebookLM MCP (notebooklm-mcp) | ✓ Installed and working |
| GitHub CLI (gh) | ✓ In use |
| RTK (token compression) | ✓ Installed 2026-04-10 |
| npx playwright | ✓ v1.58.2 (needs skill wired) |

## Low Priority (future)

| Tool | What | Why |
|------|------|-----|
| GWS (Google Workspace CLI) | Control Gmail, Docs, Calendar | Useful for personal assistant use cases |
| CLI Anything | Create CLIs from any GitHub repo | Meta-tool for building other CLIs |
| InsForge CLI | DB + auth + hosting in one | Replaces Vercel+Supabase for full-stack apps |
| LightRAG | Graph RAG for 1000s docs | When Obsidian breaks down at scale |
| LLM Fit | Best local model for hardware | If running local models |
| AwesomeDesign.md | Frontend design templates | When building web UI |
| Auto Research | ML optimization loop for skills | Benchmark/improve custom skills |
| Skill Creator Skill | AB test skills | Quantify skill improvements |
| Agent Browser (Vercel) | Playwright alternative | Fallback if Playwright issues |
| FFmpeg | Video/audio manipulation | If Claude needs multimedia work |
