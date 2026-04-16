---
type: guide
date: 2026-04-16
topic: paperclip-agent-os
tags: [tools, agents, automation, claude-code, paperclip]
status: ready-to-implement
---

# Paperclip Implementation Guide — GrowReach Command Center

*Mined from NotebookLM notebook `3ec97cdb-1512-43be-aced-61e116473133` ("Command Center: Managing Business Goals Beyond the Terminal") · April 2026*

---

## Verdict: Paperclip Wins for This Use Case

**Multica** is a team collaboration tool (Linear/Jira-like Kanban with multi-user org management). Its strength is shared task visibility across multiple operators. For a solo operator doing marketing + LinkedIn work, it's the wrong mental model and adds Docker complexity for no benefit.

**Paperclip** is the correct choice. Reasons:

| Factor | Paperclip | Multica |
|--------|-----------|---------|
| Solo operator model | Board of Directors → delegate to agents | Multi-user team Kanban |
| Marketing/non-coding workflows | CMO / Content Strategist / Competitive Intel agents baked in | Engineering-first metaphors |
| 24/7 autonomous operation | Heartbeat scheduling (agents wake on interval, execute, sleep) | Manual triggering + Docker |
| Setup complexity | One-line `npx` install; VPS one-click template | Requires Docker or managed backend |
| Skills ecosystem | skills.sh marketplace + GitHub URL installs | Skill compounding (good but secondary) |
| GitHub stars / community | 40k+ | 13k |
| License | MIT | Apache 2.0 |

**Critical caveat**: The research explicitly flags "setup porn" risk — spending time organizing your org chart instead of shipping work. The setup below is intentionally minimal: 4 agents, not 20.

---

## Why Visual OS Beats the Terminal

Six pain points the research identified, and how Paperclip fixes them:

1. **Context switching kills flow** — Terminal requires remembering exact command syntax. Paperclip dashboard shows all active agents and their status at a glance.
2. **No visibility on what agents are doing** — With CLI, you run a command and wait. Paperclip shows real-time agent activity, current task, and output streaming.
3. **Goals live nowhere** — Prompts in a terminal session disappear. Paperclip's Mission file (Soul.md) persists the agent's purpose across sessions.
4. **No scheduled work** — Terminal requires you to be present. Heartbeat scheduling lets agents execute while you sleep.
5. **Ad-hoc ≠ systematic** — One-off commands in Claude Code mean no learning from prior work. Paperclip task history + skill compounding means repeated work gets faster.
6. **Multi-agent coordination is impossible in a terminal** — Running four parallel Claude Code sessions is chaos. Paperclip's org chart routes tasks to the right agent automatically.

---

## GrowReach Org Chart Design

```
YOU (Board of Directors)
└── CEO Agent — strategic coordinator, task dispatcher
    ├── CMO Agent — marketing strategy, campaign direction
    │   ├── LinkedIn Content Writer Agent — posts, carousels, hooks
    │   └── Competitive Intelligence Agent — competitor monitoring
    └── (optional) Dev Agent — code tasks, scripts, fixes
```

**Start with just 3 agents** (CEO + LinkedIn Writer + Competitive Intel). Add CMO and Dev agent once you have the first three running reliably.

### Agent Mission Files (what to write in Soul.md for each)

**CEO Agent**
```
You are the CEO of GrowReach, a LinkedIn content automation SaaS.
Mission: Coordinate marketing and content operations. When given a goal,
break it into tasks and delegate to the appropriate specialist agents.
Maintain a weekly priority list. Report to the Board of Directors (the user)
with blockers and wins. Never execute creative work yourself — always delegate.
```

**LinkedIn Content Writer Agent**
```
You are GrowReach's LinkedIn Content Strategist.
Mission: Write high-engagement LinkedIn posts and carousel scripts for
personal branding in the B2B SaaS and marketing space. Target audience:
founders, marketers, and growth operators. Voice: direct, insight-driven,
no filler. Format: hook (first line stops scroll) → story/insight → CTA.
Always write 3 variations per brief and flag the strongest one.
```

**Competitive Intelligence Agent**
```
You are GrowReach's Competitive Intelligence Analyst.
Mission: Monitor LinkedIn automation and B2B SaaS competitors (Taplio,
Reepl, Typegrow, Postiv, Supergrow, Lempod, AuthoredUp). Track pricing
changes, new feature launches, product hunt posts, and positioning shifts.
Deliver weekly summaries and flag urgent changes within 24 hours.
Sources: competitor websites, Twitter/X, Product Hunt, LinkedIn, G2.
```

---

## Installation: Step by Step

### Prerequisites

```bash
# Verify versions
node --version    # Must be 20+
pnpm --version    # Must be 9.15+

# Install pnpm if missing
npm install -g pnpm@latest
```

### Option A: Local (Testing Only)

```bash
# One-line install — boots dashboard on :3000, API on :3100
npx paperclipai AI onboard --yes
```

Open `http://localhost:3000` in browser. This is fine for testing but **stops when your laptop sleeps** — agents cannot run on heartbeat.

### Option B: VPS (Production — Recommended)

1. **Hostinger VPS** — Has a one-click Paperclip template. Cheapest plan (~$5/mo) is sufficient.
   - Go to Hostinger VPS → Create Server → Template → Search "Paperclip" → Deploy
   - SSH in, the app is already running at your server IP

2. **Manual VPS install** (any provider: DigitalOcean, Hetzner, Linode):
```bash
# On VPS (Ubuntu 22.04+)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install -g pnpm@latest

git clone https://github.com/paperclipai/paperclip
cd paperclip
pnpm install
pnpm build
pnpm start

# Keep running after SSH disconnect
nohup pnpm start > paperclip.log 2>&1 &
# Or use pm2:
npm install -g pm2
pm2 start "pnpm start" --name paperclip
pm2 startup && pm2 save
```

Dashboard accessible at `http://YOUR_VPS_IP:3000`. Add a reverse proxy (nginx) + SSL via Certbot if you want HTTPS.

---

## Agent Configuration

### Step 1: Create an Agent

Dashboard → **+ New Agent** → Fill in:
- **Name**: e.g., `LinkedIn Content Writer`
- **Adapter**: Select **Anthropic / Claude Code** ← CRITICAL: Never select Codex CLI (known broken bug as of 2026)
- **Model**: `claude-sonnet-4-6` (default for content work) or `claude-opus-4-6` (complex reasoning tasks)
- **Soul.md**: Paste the mission file text from the section above

### Step 2: Heartbeat Intervals

| Agent | Interval | Rationale |
|-------|----------|-----------|
| CEO | 600s (10 min) | Check for new user tasks, coordinate |
| LinkedIn Content Writer | 3600s (1 hour) | Runs when briefed, not continuously |
| Competitive Intel | 21600s (6 hours) | Daily monitoring cadence |
| Dev Agent | On-demand only | No heartbeat — activate manually |

Set interval in agent config under **Heartbeat** tab.

### Step 3: Enable Skip Permissions

**Critical**: In each agent's Configuration tab → **Permissions** → Enable **"Skip Permissions"**

Without this, agents pause on every tool call waiting for your approval, breaking autonomous operation. Skip permissions = agents can read/write files, run scripts, call APIs without interrupting you.

### Step 4: Set Budget Caps

Under agent Configuration → **Budget**:
- LinkedIn Writer: $2/task max
- Competitive Intel: $5/run max
- CEO: $1/coordination cycle max

This prevents runaway spend if an agent loops.

---

## API Keys to Wire

In each agent: **Configuration → Environment Variables → Add**

| Key Name | Value | Assign To |
|----------|-------|-----------|
| `ANTHROPIC_API_KEY` | Your Anthropic key | All agents |
| `BRAVE_SEARCH_API` | Brave Search key (brave.com/search/api) | Competitive Intel |
| `RESEND_API_KEY` | Resend key (resend.com) | CEO (for email digests) |

After adding each key: click **Seal** → name the secret (e.g., `anthropic-main`) → **Save**. Sealed keys are encrypted at rest and not visible in plaintext after saving.

---

## Skills to Install

From the skills.sh marketplace or GitHub URLs. Install via agent config → **Skills** tab → **+ Add Skill**:

| Skill | Purpose | Agent |
|-------|---------|-------|
| `web-search` | Brave Search integration | Competitive Intel |
| `file-manager` | Read/write local files | All |
| `linkedin-post-formatter` | Format posts to LinkedIn character limits | LinkedIn Writer |
| `notion-connector` | Push content briefs to Notion | CEO |
| `github-read` | Read code context when Dev agent needs it | Dev Agent |

Skills from GitHub: paste the repo URL in the Skills tab → Paperclip clones and installs automatically.

---

## Common Mistakes to Avoid

1. **Using Codex CLI adapter** — It's broken. Always Anthropic/Claude Code.
2. **Running local for production** — Heartbeats die when laptop sleeps. VPS required for 24/7.
3. **Not sealing API keys** — Unsealed keys are stored in plaintext. Always seal after entering.
4. **Building 10 agents before validating 2** — Start with CEO + Competitive Intel only. Prove the loop works before expanding.
5. **No budget caps** — An agent stuck in a research loop at opus-level can burn $50 before you notice. Set $5 max per run until you trust the agent.
6. **Skip permissions off** — Agents become useless if they pause every 30 seconds waiting for approval. Enable it.
7. **Setup porn trap** — Spending a week configuring the org chart instead of running actual tasks. Rule: if an agent hasn't run a real task in 48 hours, delete it. Only build what you're actively using.

---

## Integration with Existing Claude Code Setup

Paperclip **sits on top of** your existing Claude Code + skills/hooks stack. It does not replace it.

```
Paperclip (Control Plane)
  └── Orchestrates which work gets done and when
      └── Claude Code (Execution Engine)
          └── Runs your existing skills, scripts, hooks
              └── Vault, repo, CLAUDE.md rules all apply
```

**Practical integration points:**

- **Paperclip CEO agent** → creates GitHub Issues via your existing `task-tracker.js`
- **Competitive Intel agent** → calls your existing `scripts/scrape.js` for competitor page analysis
- **LinkedIn Writer agent** → reads from `knowledge/` vault and writes drafts to `_inbox/` for your review
- **Heartbeat agents** → respect your existing CLAUDE.md rules (SEO gates, vault persistence, etc.) because they execute through Claude Code which reads CLAUDE.md

**Folder structure recommendation** (add to repo):
```
.paperclip/
├── agents/
│   ├── ceo/
│   │   ├── Soul.md          (mission file)
│   │   ├── HBT.md           (heartbeat instructions)
│   │   └── tasks.md         (current task queue)
│   ├── linkedin-writer/
│   │   └── ...
│   └── competitive-intel/
│       └── ...
└── config.yaml              (heartbeat intervals, model assignments)
```

Keep these files in the repo so agent behavior is version-controlled and recoverable.

---

## Launch Sequence (Minimal Viable Setup)

1. `npx paperclipai AI onboard --yes` — boot locally to test UI
2. Create CEO agent + paste Soul.md → assign Claude Code adapter → seal ANTHROPIC_API_KEY
3. Create Competitive Intel agent → add Brave Search key → set 6hr heartbeat → enable skip permissions
4. Give CEO a real task: "Pull the latest pricing page from Taplio and Reepl, compare their plans, and save a summary to `knowledge/competitors/pricing-update-apr2026.md`"
5. Watch it run. Verify the output file was written correctly.
6. If that works → spin up VPS → migrate → add LinkedIn Writer agent
7. After 1 week of real use → decide whether CMO/Dev agents add value

---

## Source Notebooks

- NotebookLM `3ec97cdb-1512-43be-aced-61e116473133` — "Command Center: Managing Business Goals Beyond the Terminal"
- GitHub: https://github.com/paperclipai/paperclip (Paperclip)
- GitHub: https://github.com/multica-ai/multica (Multica — not selected)

*Saved: 2026-04-16*
