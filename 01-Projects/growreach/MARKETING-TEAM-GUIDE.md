# GrowReach Marketing Team — Management Guide

**Created:** 2026-03-03  
**Team Lead:** Ellianto ⚡  
**Status:** ✅ Team Assembled & Ready

---

## 🎯 Your Marketing Squad

| Agent | ID | Status | What They Do |
|-------|-----|--------|--------------|
| **Content Strategist** | `sub-agent-content` | ✅ Ready | Blog posts, SEO content, thought leadership |
| **Conversion Optimizer** | `sub-agent-cro` | ✅ Ready | Landing page audits, A/B tests, signup flow optimization |
| **Social Manager** | `sub-agent-social` | ✅ Ready | LinkedIn/Twitter posts, engagement, viral content |
| **Paid Ads Specialist** | `sub-agent-ads` | ✅ Ready | Google/Meta/LinkedIn ads, CAC optimization |
| **SEO Specialist** | `sub-agent-seo` | ✅ Ready | Technical SEO, programmatic pages, schema markup |
| **Email & Automation** | `sub-agent-email` | ✅ Ready | Email sequences, automation workflows, lifecycle marketing |

---

## 🚀 How to Use Your Team

### Method 1: Talk to Ellianto (Main Agent)

Just tell me what you need in plain English:

> "I need a blog post about LinkedIn automation ROI"

I'll automatically:
1. Spawn the right agent (Content Strategist)
2. Give them the task with proper context
3. Monitor their progress
4. Deliver the result to you

**This is the easiest way.**

---

### Method 2: Direct Task Assignment (Mission Control)

For more control, assign tasks directly in Mission Control:

```bash
# Create a task
psql $MISSION_CONTROL_DB_URL -c "
INSERT INTO tasks (title, description, status, priority, assigned_to)
VALUES (
  'Write blog post: LinkedIn automation ROI',
  'Research and write 1500-word blog post targeting keyword \"linkedin automation roi\". Include data, examples, and CTA to GrowReach trial.',
  'inbox',
  'high',
  'sub-agent-content'
);"
```

Then agents will automatically pick up their assigned tasks.

---

### Method 3: Spawn Agent for Specific Task

```bash
sessions_spawn(
  agentId="content-strategist",
  task="Write blog post about LinkedIn automation best practices",
  label="blog-automation-practices"
)
```

---

## 📋 Common Tasks by Agent

### Content Strategist (`sub-agent-content`)
- "Write a blog post about [topic]"
- "Create SEO content calendar for next month"
- "Research keywords for LinkedIn automation niche"
- "Write landing page copy for [feature]"

### Conversion Optimizer (`sub-agent-cro`)
- "Audit our landing page and suggest improvements"
- "Design A/B test for signup flow"
- "Set up analytics tracking for trial signups"
- "Optimize pricing page conversion rate"

### Social Manager (`sub-agent-social`)
- "Write 5 LinkedIn posts for this week"
- "Create Twitter thread about [topic]"
- "Repurpose blog post into social content"
- "Write engaging hooks for [product feature]"

### Paid Ads Specialist (`sub-agent-ads`)
- "Create Google Ads campaign for [keyword]"
- "Write Meta ad copy for [audience]"
- "Audit our ad performance and optimize"
- "Build audience segments for LinkedIn ads"

### SEO Specialist (`sub-agent-seo`)
- "Run technical SEO audit on our site"
- "Build programmatic comparison pages"
- "Add schema markup to [page type]"
- "Research and prioritize SEO opportunities"

### Email & Automation (`sub-agent-email`)
- "Create welcome email sequence for new signups"
- "Build onboarding automation workflow"
- "Write re-engagement email for churned users"
- "Set up trial-to-paid conversion emails"

---

## 📊 Monitoring Your Team

### Check Task Status
```bash
psql $MISSION_CONTROL_DB_URL -c "
SELECT id::text, title, assigned_to, status, priority
FROM tasks
WHERE status != 'done'
ORDER BY priority DESC, created_at ASC;"
```

### Check Agent Activity
```bash
psql $MISSION_CONTROL_DB_URL -c "
SELECT name, status, last_active
FROM agents
ORDER BY last_active DESC;"
```

### See Recent Comments/Updates
```bash
psql $MISSION_CONTROL_DB_URL -c "
SELECT t.title, c.author, c.text, c.created_at
FROM comments c
JOIN tasks t ON c.task_id = t.id
ORDER BY c.created_at DESC
LIMIT 10;"
```

---

## 🎨 Customizing Your Agents

Each agent has a `SOUL.md` file in `.agents/[agent-name]/SOUL.md`.

**Edit these files to:**
- Change their communication style
- Add specific priorities or preferences
- Define their workflow and decision-making process

**Example:**
```bash
# Edit Content Strategist's personality
nano .agents/content-strategist/SOUL.md
```

---

## 🔧 Agent Configuration Files

Each agent directory contains:

```
.agents/
├── content-strategist/
│   ├── SOUL.md          ← Agent personality & style
│   ├── AGENTS.md        ← Mission Control integration
│   └── TOOLS.md         ← Tool-specific notes (optional)
├── conversion-optimizer/
├── social-manager/
├── paid-ads-specialist/
├── seo-specialist/
└── email-automation/
```

---

## 💡 Best Practices

### 1. **Start Small**
Don't spawn all agents at once. Start with 1-2 for your immediate needs.

### 2. **Give Context**
The more context you provide, the better the output. Reference:
- `.claude/product-marketing-context.md`
- Existing content/pages
- Target audience specifics

### 3. **Review & Iterate**
Agents deliver work for your review. Give feedback and they'll refine.

### 4. **Track Performance**
Monitor what works. Double down on high-performing agents/tasks.

---

## 🎯 Quick Start Suggestions

### Week 1 Focus: Content Foundation
1. **Content Strategist** → Create content calendar + first 3 blog posts
2. **SEO Specialist** → Run technical audit + fix critical issues

### Week 2 Focus: Conversion
1. **Conversion Optimizer** → Audit landing page + implement fixes
2. **Email & Automation** → Build welcome + onboarding sequences

### Week 3 Focus: Distribution
1. **Social Manager** → Create 2 weeks of LinkedIn content
2. **Paid Ads Specialist** → Set up first ad campaigns (Google + Meta)

---

## 📞 Getting Help

**Talk to Ellianto (me!):**
- Telegram: Just message me
- Mission Control: Add comments to tasks
- Webchat: Use the OpenClaw interface

I coordinate everything. You don't need to manage agents individually unless you want to.

---

## 🔄 Next Steps

1. ✅ **Team is assembled** — All agents registered in Mission Control
2. ✅ **Product context created** — All agents have GrowReach context
3. ✅ **SOUL.md files created** — Customize them to your preferences
4. 🎯 **Ready for tasks** — Just tell me what you need!

---

**Your first task:** Tell me your biggest marketing priority right now, and I'll assign the right agent to handle it.

⚡ **Let's grow GrowReach together!**
