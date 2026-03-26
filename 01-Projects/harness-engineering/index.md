---
type: project-note
date: 2026-03-26
project: harness-engineering
status: active
tags: [claude-code, hooks, automation, obsidian]
---
# Harness Engineering — Project Index

Building a Claude Code harness: hooks, skills, and memory systems that make Claude more reliable and context-aware.

## What it is
- Claude Code settings.json hooks for mechanical enforcement of rules
- Skills system: slash commands in `.claude/commands/`
- Second brain: Obsidian vault as navigable memory offload
- Anti-patterns eliminated: doom loops, rationalization, context rot

## Active work
- [x] Obsidian vault created and on server (~/workspace/obsidian-vault)
- [x] /resume skill — loads vault index at session start
- [x] SessionStart hook — auto-injects vault index
- [ ] Session digest workflow — write digest at end of each session
- [ ] Dataview plugin setup in Obsidian
- [ ] Migrate MEMORY.md content into vault notes
- [ ] MCP bridge (future): obsidian-mcp for direct vault reads

## Key files (in ClaudeCode2.0 repo)
| What | Path |
|------|------|
| All hooks | `.claude/hooks/` |
| All skills | `.claude/commands/` |
| Settings/hook wiring | `.claude/settings.json` |
| Implementation plan | `knowledge/obsidian-second-brain-plan.md` |
| Hook enforcement rules | `CLAUDE.md` |

## Vault setup
- Mac vault path: `/Users/10xer/Documents/RahulClaude`
- GitHub: `https://github.com/rahul-10xers/obsidian`
- Server vault: `~/workspace/obsidian-vault`
- Sync: Obsidian Git plugin (Mac→GitHub) + cron pull (server←GitHub every 5min)

## Hooks wired
| Hook | Script | Purpose |
|------|--------|---------|
| SessionStart | session-start.sh | Timezone + lessons + vault index |
| Stop | push-memory.sh | Save transcript to GitHub |
| PreCompact | push-memory.sh | Save before context compression |
| UserPromptSubmit | skills-router.py | Match skills before responding |
| PostToolUse | context-monitor.py | Monitor context usage |
| PreToolUse | git-author-guard.py | Enforce commit author |
