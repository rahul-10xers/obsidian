# Obsidian Second Brain for Claude Code — Implementation Plan

**Date:** 2026-03-25
**Source:** NotebookLM notebook `4be6def3-e997-42da-a9d2-f95490c7e916` (43 sources, 6 queries)
**Problem:** ClaudeCode2.0 repo has too many files, topics, and tasks — context is unmanageable.
**Solution:** Obsidian vault as a navigational second brain. Claude starts light, traverses the vault just-in-time to load only what's relevant for the current task.

---

## The Core Insight

**Don't dump everything into context. Navigate to what's needed.**

The failure mode of the current setup: MEMORY.md, CLAUDE.md, knowledge/, and .claude/ are all loaded into context simultaneously. At 30%+ context utilization, Claude degrades silently. The vault flips this: Claude starts with a 1-page index, reads the index to find relevant nodes, loads only those nodes. 60% token reduction is achievable via QMD semantic search vs raw grep/glob.

---

## Architecture: Five-Layer Memory Model

| Layer | What it is | Lives in |
|-------|-----------|---------|
| **Working Memory** | Active context window | Claude's context (ephemeral) |
| **Episodic Memory** | Session digests, "what happened" | `vault/session-digests/YYYY-MM-DD.md` |
| **Semantic Memory** | Stable facts, project knowledge | `vault/01-Projects/`, `vault/02-Areas/`, `vault/03-Resources/` |
| **Procedural Memory** | How to operate, rules | `CLAUDE.md` + `vault/99-Meta/CONSTITUTION.md` |
| **Identity Layer** | Core persona, non-negotiables | `vault/99-Meta/CONSTITUTION.md` |

The vault houses layers 2-5. Claude only ever loads layer 1 (working memory) from the vault — the rest it navigates to on demand.

---

## Vault Structure

```
ClaudeCode2.0-vault/
├── 00-INBOX/                    # Unprocessed captures — process within 48h
│   └── README.md
├── 01-Projects/                 # One folder per active project
│   ├── growreach/
│   │   ├── index.md             # Project MOC (Map of Content)
│   │   ├── competitor-analysis-status.md
│   │   ├── seo-strategy.md
│   │   └── product-roadmap.md
│   └── harness-engineering/
│       ├── index.md
│       ├── hooks-architecture.md
│       └── skill-system.md
├── 02-Areas/                    # Ongoing responsibilities
│   ├── seo/
│   ├── competitors/
│   ├── product/
│   └── hooks-and-automation/
├── 03-Resources/                # Reference knowledge (tools, APIs, frameworks)
│   ├── tools/
│   │   ├── notebooklm-usage.md
│   │   ├── playwright-scraper.md
│   │   └── wordpress-rest-api.md
│   ├── frameworks/
│   └── credentials-index.md    # WHAT credentials exist (not the values)
├── 04-Archive/                  # Completed/inactive items
└── 99-Meta/
    ├── VAULT-INDEX.md           # Live dashboard — Claude reads this at session start
    ├── CONSTITUTION.md          # Vault-level rules + file naming conventions
    ├── templates/
    │   ├── project-note.md
    │   ├── session-digest.md
    │   └── area-index.md
    └── session-digests/         # One file per session
        └── YYYY-MM-DD-HHmm.md
```

---

## VAULT-INDEX.md — The Dashboard Claude Reads First

This is the ONE file Claude loads at session start. It's kept under 100 lines.

```markdown
# Vault Index — ClaudeCode2.0 Second Brain
Last updated: {{date}}

## Active Projects
- [[growreach/index]] — LinkedIn comment automation SaaS (ACTIVE)
- [[harness-engineering/index]] — Claude Code hooks + skill system (ACTIVE)

## Open Threads
- [ ] Competitor analysis: 5 pending (linkedhelper, dripify, engage-ai, commenter.ai, socialsonic)
- [ ] SEO comparison pages: 3 pages at NW 60, need to reach 70+
- [ ] Obsidian vault setup (this task)

## Recent Sessions
- [[session-digests/2026-03-25]] — Obsidian second brain research + plan
- [[session-digests/2026-03-24]] — Harness engineering notebook mining
- [[session-digests/2026-03-23]] — Anti-rationalization hook + WP scripts

## Quick Nav
- GrowReach product: [[growreach/index]]
- Competitor data: [[competitors/COMPETITOR_ANALYSIS]]
- SEO strategy: [[seo/strategy]]
- Hooks/harness: [[harness-engineering/index]]
- Tools access: [[resources/tools/saas-tools-access]]
- Credentials: [[resources/credentials-index]]
```

---

## MCP Bridge Setup

### Step 1: Install Obsidian
Download from obsidian.md, create vault at: `~/obsidian-vaults/ClaudeCode2-brain/`

### Step 2: Install Local REST API plugin in Obsidian
1. Obsidian → Settings → Community Plugins → Browse → search "Local REST API"
2. Install + Enable
3. Note the API key shown in plugin settings
4. Default port: 27124

### Step 3: Install obsidian-mcp
```bash
npm install -g obsidian-mcp
```

### Step 4: Add to `~/.claude/settings.json`
```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "obsidian-mcp"],
      "env": {
        "OBSIDIAN_API_KEY": "YOUR_KEY_FROM_PLUGIN",
        "OBSIDIAN_HOST": "http://localhost:27124"
      }
    }
  }
}
```

### Verify bridge works
```bash
curl -s http://localhost:27124/vault/ -H "Authorization: Bearer YOUR_KEY" | head -20
```

---

## Required Obsidian Plugins

| Plugin | Purpose | Priority |
|--------|---------|---------|
| **Local REST API** | MCP bridge (mandatory) | MUST |
| **Dataview** | Query notes like a database (`TABLE`, `LIST` queries) | MUST |
| **Templater** | Standardized note templates | HIGH |
| **Obsidian Git** | Version control safety rail | HIGH |
| **QMD Semantic Search** | 60% token reduction — semantic search in vault | HIGH |
| Terminal | Embedded terminal (run Claude Code from within Obsidian) | NICE |

---

## Session Lifecycle Hooks

Wire these into `.claude/settings.json` hooks section.

### SessionStart hook — load context light
```python
# .claude/hooks/obsidian-session-start.py
# Reads VAULT-INDEX.md + last 2 session digests
# Uses Haiku (cheap) sub-agent to summarize and inject
import subprocess, json

vault_path = os.path.expanduser("~/obsidian-vaults/ClaudeCode2-brain")
index_path = f"{vault_path}/99-Meta/VAULT-INDEX.md"

with open(index_path) as f:
    index = f.read()

# Print to stdout — Claude Code injects this into session context
print(f"=== VAULT INDEX ===\n{index}\n=== END VAULT INDEX ===")
```

### PreCompact hook — snapshot working state
```python
# .claude/hooks/obsidian-precompact.py
# Fires before context compression
# Saves: current task, touched files, git branch, uncommitted changes
import subprocess, json, datetime

snapshot = {
    "timestamp": datetime.datetime.now().isoformat(),
    "git_branch": subprocess.getoutput("git branch --show-current"),
    "git_status": subprocess.getoutput("git status --short"),
    "task": "ACTIVE_TASK_PLACEHOLDER"  # Claude injects this
}

snapshot_path = "/tmp/claude-precompact-snapshot.json"
with open(snapshot_path, "w") as f:
    json.dump(snapshot, f, indent=2)

print(f"[PreCompact] State snapshotted to {snapshot_path}")
```

### PostCompact hook — restore working state
```python
# .claude/hooks/obsidian-postcompact.py
import json, os

snapshot_path = "/tmp/claude-precompact-snapshot.json"
if os.path.exists(snapshot_path):
    with open(snapshot_path) as f:
        snapshot = json.load(f)
    print(f"=== RESTORED STATE ===")
    print(f"Branch: {snapshot['git_branch']}")
    print(f"Uncommitted: {snapshot['git_status']}")
    print(f"Last task: {snapshot['task']}")
    print(f"=== END RESTORED STATE ===")
```

### SessionEnd hook — archive digest to vault
```python
# .claude/hooks/obsidian-session-end.py
# Appends a brief digest to vault/99-Meta/session-digests/
import datetime, subprocess

date_str = datetime.datetime.now().strftime("%Y-%m-%d-%H%M")
digest_path = os.path.expanduser(
    f"~/obsidian-vaults/ClaudeCode2-brain/99-Meta/session-digests/{date_str}.md"
)

# Claude writes the summary; hook saves it
# (Claude outputs the digest; hook captures stdout and writes file)
```

---

## Startup Ritual — `/resume` Skill

Create `.claude/commands/resume.md`:

```markdown
# /resume — Vault Startup Ritual

Loads session context from the Obsidian second brain.

## Protocol
1. Read `~/obsidian-vaults/ClaudeCode2-brain/99-Meta/VAULT-INDEX.md`
2. Read last 2 session digests from `99-Meta/session-digests/` (sorted by date, newest first)
3. Print "Last Checkpoint" status: branch, open threads, last task
4. Ask: "What are we working on today?" (unless user already specified)

## Last Checkpoint format
```
=== SESSION RESTORED ===
Branch: main | Last session: 2026-03-24
Open threads: 3 (see VAULT-INDEX for details)
Active project: growreach
Ready. What are we working on?
=========================
```
```

---

## File Naming Convention

**Files named like sentences provide context before content is read.**

| Instead of | Use |
|-----------|-----|
| `analysis.md` | `growreach-competitor-positioning-strategy.md` |
| `notes.md` | `linkedin-automation-icp-pain-points.md` |
| `todo.md` | `dripify-analysis-remaining-tasks.md` |
| `seo.md` | `comparison-page-seo-requirements-nw70.md` |

Pattern: `{project}-{topic}-{specificity}.md`
Max path depth: 3 levels (vault/project/file.md) — deeper = more tokens to reference.

---

## YAML Frontmatter Standard

Every vault note gets this header:

```yaml
---
type: project-note | area-ref | resource | session-digest | inbox
date: 2026-03-25
project: growreach | harness-engineering | meta
status: active | on-hold | archived
tags: [seo, competitor, hooks, product]
---
```

The `status` + `project` fields power Dataview queries. Add once → appears in all relevant dashboards automatically.

---

## Dataview Queries (Proactive Surfacing)

Add to VAULT-INDEX.md to auto-surface active items:

```dataview
TABLE status, date
FROM "01-Projects"
WHERE status = "active"
SORT date DESC
```

```dataview
LIST
FROM "00-INBOX"
SORT date ASC
```

These update live — Claude reads the rendered output, not the query syntax.

---

## Migration Plan: Existing Repo → Vault

### Phase A: Bootstrap (no disruption to existing workflow)
1. Install Obsidian + plugins
2. Set up MCP bridge
3. Create vault folder structure + CONSTITUTION.md
4. Create VAULT-INDEX.md with current repo state

### Phase B: Migrate MEMORY.md content
- Split MEMORY.md by topic → individual vault notes
- MEMORY.md stays in repo as a thin pointer: "Full memory in Obsidian vault"
- `knowledge/competitors/` → vault `01-Projects/growreach/competitors/`
- `knowledge/seo/` → vault `01-Projects/growreach/seo/`
- `.claude/product-marketing-context.md` → vault `01-Projects/growreach/product-context.md`

### Phase C: Wire session hooks
- Add PreCompact/PostCompact hooks to `.claude/settings.json`
- Add SessionStart hook to load VAULT-INDEX at session start
- Create `/resume` skill

### Phase D: QMD Semantic Search
- Install QMD plugin in Obsidian
- Test: query a topic, confirm Claude gets relevant notes back in < 5 tool calls

---

## CONSTITUTION.md — Vault-Level Rules

Create at `99-Meta/CONSTITUTION.md`:

```markdown
# Vault Constitution — ClaudeCode2.0 Second Brain

## Navigation Rules
1. Always start at VAULT-INDEX.md
2. Follow [[wikilinks]] to go deeper — don't jump to files directly
3. Max 3 hops before synthesizing what you found
4. Write to 00-INBOX only — never edit existing notes without user approval

## File Naming
- Pattern: {project}-{topic}-{specificity}.md
- Max depth: vault/folder/file.md (3 levels)
- No generic names (notes.md, todo.md, analysis.md)

## Frontmatter Required
Every file needs: type, date, project, status, tags

## Memory Hygiene
- CLAUDE.md > 280 lines → archive completed sections to CLAUDE-Archive.md
- 00-INBOX > 5 files → process immediately before new work
- Session digest within 30 min of session end
```

---

## QMD Semantic Search — Key Benefit

QMD (Quick Markdown Search) replaces brute-force grep:

**Old pattern (expensive):**
```
grep -r "competitor" knowledge/ --include="*.md" -l   # reads ALL files
```

**New pattern (60% cheaper):**
```
# Via MCP: semantic query against vault index
# Returns: top 5 most relevant notes with excerpts
# Claude only reads those 5, not 200 files
```

The QMD plugin builds a local vector index of the vault. Queries return ranked results. Claude reads only the hits — not the whole vault.

---

## Failure Modes to Avoid

| Anti-pattern | Why it breaks | Fix |
|-------------|--------------|-----|
| Deep folder nesting (>3 levels) | Token-expensive to reference paths | Max 3 levels always |
| CLAUDE.md drift past 280 lines | 26k tokens loaded every session | PreCompact hook auto-archives |
| Note graveyards (00-INBOX piling up) | Notes never get processed, vault becomes dump | 48h processing rule enforced by hook |
| Vague file names (`notes.md`, `todo.md`) | Can't determine relevance without reading | Named-like-sentences convention |
| Destructive agent actions | Agent deletes/overwrites vault notes | Write to 00-INBOX only; no direct edits |
| Loading entire vault at session start | Defeats the purpose — same context bloat | Load VAULT-INDEX only (< 100 lines) |
| Unconstrained synthesis | Claude fills gaps from training data, not vault | Only cite sources found in vault |

---

## Implementation Sequence (Go Order)

```
Day 1 (Setup — ~2 hours)
├── 1. Install Obsidian + all plugins
├── 2. Create vault at ~/obsidian-vaults/ClaudeCode2-brain/
├── 3. Create folder structure + README files
├── 4. Install obsidian-mcp: npm install -g obsidian-mcp
├── 5. Configure Local REST API plugin → get API key
├── 6. Add MCP config to ~/.claude/settings.json
└── 7. Verify bridge: curl localhost:27124

Day 1 (Constitution — ~1 hour)
├── 8. Write 99-Meta/CONSTITUTION.md
├── 9. Write 99-Meta/VAULT-INDEX.md (initial state)
└── 10. Create templates/ folder with 3 templates

Day 2 (Migration — ~2 hours)
├── 11. Migrate MEMORY.md → split into vault notes
├── 12. Migrate knowledge/competitors/ → vault
├── 13. Migrate knowledge/seo/ → vault
└── 14. Update repo MEMORY.md to thin pointer

Day 2 (Hooks — ~1 hour)
├── 15. Write obsidian-session-start.py hook
├── 16. Write obsidian-precompact.py hook
├── 17. Write obsidian-postcompact.py hook
├── 18. Wire hooks into .claude/settings.json
└── 19. Create .claude/commands/resume.md skill

Day 3 (Verify — ~1 hour)
├── 20. Start fresh session, run /resume
├── 21. Confirm VAULT-INDEX loads
├── 22. Test a task: navigate to competitor notes via vault
├── 23. Confirm PreCompact fires and saves state
└── 24. Check token count vs old pattern
```

---

## Success Criteria

- [ ] `curl localhost:27124/vault/` returns file listing
- [ ] `/resume` at session start loads VAULT-INDEX in < 3 tool calls
- [ ] Navigating to a competitor note takes < 5 tool calls (vs 15+ grep calls)
- [ ] PreCompact hook fires and saves state — confirmed via `/tmp/claude-precompact-snapshot.json`
- [ ] Session digest written to vault after session ends
- [ ] CLAUDE.md stays under 280 lines (harness enforces)
- [ ] Token count at session start < 10k (vs current ~40k)

---

*Plan sourced from NotebookLM notebook `4be6def3-e997-42da-a9d2-f95490c7e916` — 43 sources, 6 queries.*
*Note saved to notebook: `3d6afd50-03aa-4c2d-b1c4-25efeca75031`*
