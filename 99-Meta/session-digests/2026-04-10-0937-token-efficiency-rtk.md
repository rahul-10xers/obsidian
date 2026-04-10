---
type: session-digest
date: 2026-04-10
project: harness-engineering
status: archived
tags: [token-efficiency, rtk, notebooklm, settings, claude-md]
---
# Session — 2026-04-10 09:37

## What we worked on

Two main threads. First, fixed a crashing NotebookLM MCP server (fakeredis v2.35.0 broke backward compatibility by renaming `FakeConnection` → `FakeAsyncRedisConnection`) — patched `docket/_redis.py` with a try/except fallback import. Second, mined the "Claude Token and Cost Optimization Guide" NotebookLM notebook for actionable settings and then implemented all recommended improvements to the Claude Code harness.

## Key decisions made

- **`settings.json` gets `"model": "sonnet"` and env vars** — adds explicit default model so Claude never silently escalates to Opus; `MAX_THINKING_TOKENS: 10000` caps runaway extended thinking; `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS: 0` disables a feature that burns tokens.
- **CLAUDE.md slimmed 342 → 72 lines** — verbose bash command blocks and vault routing tables extracted to `.claude/docs/` reference files. Saves ~2,400 tokens on every message by shrinking the always-loaded system prompt.
- **Rule #19 added (Token Awareness)** — formalizes model triage (Haiku/Sonnet/Opus routing), subagent protocol for research, compact timing, and extended thinking guard.
- **RTK (Rust Token Killer) installed** — PreToolUse hook intercepts every Bash call and compresses noisy output (git diffs, test runs, build logs) before it enters context. Expected 60–80% reduction on Bash-heavy sessions.
- **fakeredis patch is library-level** — can't be fixed via package version pinning (pydocket requires fakeredis >=2.32.1 which is already the version that broke); direct source patch is the correct long-term fix unless upstream resolves it.

## Files created/modified

- `~/.claude/settings.json` — added `"model": "sonnet"`, `MAX_THINKING_TOKENS: 10000`, `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS: 0`, RTK PreToolUse hook
- `~/.claude/hooks/rtk-rewrite.sh` — **new** — RTK hook script that rewrites Bash commands through the RTK proxy
- `~/.claude/RTK.md` — **new** — slim awareness file for RTK meta commands and usage
- `CLAUDE.md` — 342 → 72 lines (61% reduction); Rules #15–18 verbose content extracted
- `.claude/docs/task-tracker-guide.md` — **new** — extracted bash commands + label table from Rule #15
- `.claude/docs/vault-guide.md` — **new** — extracted vault routing tables + format spec from Rules #16–18
- `notebooklm-mcp` source (`docket/_redis.py`) — patched `FakeConnection` import with try/except fallback

## Open threads (carry forward)

- [ ] **Restart Claude Code** — RTK hook + settings.json changes are not live until next session launch
- [ ] **Validate vault integration** (from prev session) — open fresh session, ask "what did we work on recently?" — should answer from vault auto-log without being told anything
- [ ] **Delete `_HANDOFF_github-projects-setup.md`** — temporary handoff file, work is complete
- [ ] **Auto-log deduplication** — multiple `/save-transcript` calls per session create multiple vault entries for same hour; low priority but noisy
- [ ] Competitor analyses still pending: linkedhelper, dripify full, engage-ai, commenter.ai, socialsonic

## Next session should start with

Restart Claude Code to activate RTK + settings changes, then run a Bash-heavy command (e.g. `git diff`) to confirm RTK is intercepting and compressing output.
