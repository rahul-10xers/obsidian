---
type: reference
date: 2026-04-07
project: harness
tags: [hooks, automation, enforcement, catalog]
---

# Hooks Catalog

> Every hook in `.claude/hooks/`. These run automatically — Claude does not invoke them manually. They enforce CLAUDE.md rules at the system level.
> **Repo path:** `.claude/hooks/` | **Config:** `.claude/settings.json`

---

## Hook Lifecycle

```
User sends message
  → UserPromptSubmit hooks fire (check/route before Claude responds)
  → Claude responds + uses tools
  → PostToolUse hooks fire (validate after each tool call)
  → Session ends / /exit
  → PreCompact hooks fire (save state before context compression)
  → Stop hooks fire (final cleanup)
```

---

## Enforcement Hooks (run on every message)

| Hook | When it fires | What it enforces |
|---|---|---|
| `skills-router.py` | UserPromptSubmit | Scans task description, matches against installed skills. If match found → tells Claude to use the Skill tool. Prevents Claude from reimplementing skill logic inline. |
| `anti-rationalization-gate.py` | UserPromptSubmit | Blocks Claude from skipping Rule #11 (Explore phase). Detects patterns like "I'll assume...", "Based on similar tools..." and forces verification. |
| `doom-loop-guard.py` | UserPromptSubmit | Detects when Claude is in a stuck loop (same tool call 3x, circular reasoning). Forces escalation to user. |
| `gsd-prompt-guard.js` | UserPromptSubmit | GSD workflow guard — ensures phases aren't skipped in GSD-style tasks. |
| `context-monitor.py` | PostToolUse | Tracks context window usage. Warns at 70%, forces checkpoint at 85%. |
| `gsd-context-monitor.js` | PostToolUse | GSD-specific context monitor. Triggers state checkpoint before compaction. |
| `promise-checker.py` | PostToolUse | Verifies Claude followed through on stated intentions ("I will do X" → checks X was done). |
| `verify-reminder.py` | PostToolUse | After publish/deploy tool calls → reminds Claude to verify the observable result (Rule #12). |
| `output-truncator.py` | PostToolUse | Prevents excessively long tool outputs from bloating context. Truncates at threshold. |
| `content-validator.py` | PostToolUse | Validates content before WordPress publish. Checks silo prefix, Yoast fields, NW score field. |
| `gsd-workflow-guard.js` | PostToolUse | Enforces GSD phase ordering — can't run Phase 6 before Phase 4. |
| `gsd-check-update.js` | PostToolUse | Checks for GSD skill updates. |

---

## Session Lifecycle Hooks

| Hook | When it fires | What it does |
|---|---|---|
| `session-start.sh` | Session start (UserPromptSubmit #1) | Pulls latest from GitHub (repo + vault). Injects: timezone, LESSONS_LEARNED.md, HANDOFF.md, CRASH_BUFFER.md, VAULT-INDEX.md + last 2 session digests. |
| `crash-buffer-prompt.sh` | PreCompact | Instructs Claude to write CRASH_BUFFER.md before context compaction. Ensures mid-session state survives compression. |
| `save-transcript.sh` | Stop (session end) | Exports current conversation to `transcripts/YYYY-MM-DD-*.md`, git commits, pushes to GitHub. |
| `push-memory.sh` | Stop / manual via `/save-transcript` | Runs transcript export + git commit + push in one shot. Called by save-transcript.sh and manually. |
| `transcript-to-md.py` | Called by save-transcript.sh | Converts raw JSONL session to readable markdown transcript format. |

---

## Git Safety Hooks

| Hook | When it fires | What it enforces |
|---|---|---|
| `git-author-guard.py` | PreCommit (PostToolUse on Bash git commits) | Verifies `git config user.email` = `rahul@10xers.co` and `user.name` = `Rahul`. Blocks commit if wrong. Fix: `git config user.email "rahul@10xers.co" && git config user.name "Rahul"` |
| `protect-files.sh` | PreCommit | Blocks commits that would overwrite protected files (credentials, auth tokens). |
| `gsd-statusline.js` | Status display | Shows current GSD phase in Claude Code status line. |

---

## Hook Configuration

Hooks are defined in `.claude/settings.json` under the `hooks` key. Format:
```json
{
  "hooks": {
    "UserPromptSubmit": [{"matcher": "", "hooks": [{"type": "command", "command": "python3 .claude/hooks/skills-router.py"}]}],
    "PostToolUse": [...],
    "PreCompact": [...],
    "Stop": [...]
  }
}
```

**To disable a hook temporarily:** Comment it out in settings.json (never delete — you'll want it back).
**To add a hook:** Add entry in settings.json → restart Claude Code session.

---

## Debugging Hooks

If a hook is blocking something unexpectedly:
```bash
# Run hook manually to see its output
python3 .claude/hooks/skills-router.py
node .claude/hooks/gsd-workflow-guard.js
bash .claude/hooks/session-start.sh
```
