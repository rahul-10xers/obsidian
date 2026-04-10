---
type: session-digest
date: 2026-04-10
project: harness-engineering
status: archived
tags: [vault, obsidian, task-tracker, github-projects, auto-sync]
---
# Session — 2026-04-10 08:03

## What we worked on

Two infrastructure improvements to the Claude Code harness. First, completed the GitHub Projects Kanban board integration that was left unfinished in the previous session — the board at `https://github.com/users/rahul-10xers/projects/1` now auto-receives every task. Second, identified and fixed a fundamental gap in the Obsidian vault setup: the vault was only ever updated when `/digest` was run manually — facts from every session between digests were silently lost. Built an automatic vault sync system so nothing is ever lost without any user action.

## Key decisions made

- **`/digest` demoted from HARD RULE to optional.** The old Rule #17 said "run /digest at end of every session." That created anxiety about forgetting. The new model: facts are auto-saved always, `/digest` is for AI narrative synthesis only — run it when a chapter closes, not every session.
- **`session-logs/` vs `session-digests/` separation.** Auto-log writes factual entries (commits + transcript URL) to `99-Meta/session-logs/`. AI-synthesized narrative still goes to `99-Meta/session-digests/`. Two different tools for two different purposes.
- **Transcript references use full GitHub URLs.** Relative paths like `transcripts/2026-04-10.md` are ambiguous from the vault — they refer to the ClaudeCode2.0 repo. Fixed to full `https://github.com/rahul-10xers/ClaudeCode2.0/blob/main/transcripts/YYYY-MM-DD.md` so links are clickable from Obsidian and GitHub.
- **`ghRaw()` helper added to task-tracker.** `gh api` does not accept `--repo` flag (unlike `gh issue`, `gh pr`). All `gh api` calls must use `ghRaw()` which runs without appending `--repo`. Documented here because this is a non-obvious gh CLI behaviour that will bite future scripts.
- **Board status auto-managed.** `task-tracker.js start` → In Progress, `done` → Done, `block` → Blocked. Board sync is wrapped in try/catch so issue tracking still works if the Projects API is unreachable.

## Files created/modified

- `scripts/task-tracker.js` — added `ghRaw()` helper, `setProjectStatus()`, auto-add to GitHub Projects board on start/done/block
- `scripts/auto-vault-log.py` — **new** — writes factual session entry to vault on every session end; called by push-memory.sh
- `.claude/hooks/push-memory.sh` — added Section 4: calls auto-vault-log.py after repo commit/push
- `CLAUDE.md` — Rule #17 rewritten: digest is optional, facts are auto-saved, Claude must not prompt user to run /digest at session end
- `CREDENTIALS.md` — GitHub classic PAT added (has `project` scope — required for Projects v2 API)
- `_HANDOFF_github-projects-setup.md` — created at end of prior session, then fully implemented and can now be deleted

## Open threads (carry forward)

- [ ] **Validate in fresh session** — open a new terminal, run `claude`, ask "what did we work on recently?" — Claude should answer from vault context without being told anything
- [ ] **Delete `_HANDOFF_github-projects-setup.md`** — it was a temporary file, work is complete
- [ ] **Auto-log runs multiple times per session** — each `/save-transcript` call triggers push-memory.sh which triggers auto-vault-log, resulting in multiple entries per session. Low priority but noisy. Could deduplicate by checking if an entry for this hour already exists.
- [ ] **Old session-log entries have relative transcript paths** — only entries from this session onward have full GitHub URLs. The 4 earlier entries today (07:50–07:56) still show `transcripts/2026-03-26.md`. Not worth fixing retroactively.
- [ ] Competitor analyses still pending: linkedhelper, dripify full, engage-ai, commenter.ai, socialsonic

## Next session should start with

Validate the vault integration end-to-end: open a fresh Claude session and ask "what did we work on recently?" to confirm session-start.sh is loading vault context correctly and Claude can answer from auto-log + digest without any manual briefing.
