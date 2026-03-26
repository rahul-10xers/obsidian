# Vault Constitution 
## Navigation Rules
1. Always start at VAULT-INDEX.md — never jump directly to files
2. Follow [[wikilinks]] to go deeper, max 3 hops before synthesizing 
3. Write new content to 00-INBOX only — never edit existing notes without user approval 
4. Max folder depth: 3 levels (vault/folder/file.md) 

## File Naming 
- Pattern: {project}-{topic}-{detail}.md 
- Good: `growreach-competitor-dripify-analysis.md` 
- Bad: `notes.md`, `todo.md`, `analysis.md`

## Frontmatter (every file)
---
type: project-note | area-ref | resource | session-digest | inbox
date: YYYY-MM-DD 
project: growreach | harness-engineering | meta
status: active | on-hold | archived
tags: []
---
## Session Protocol 
- Session start: read VAULT-INDEX.md + last 2 session digests
- Session end: write digest to 99-Meta/session-digests/YYYY-MM-DD-HHmm.md