# GrowReach GEO Audit — AI Search Visibility
**Tool:** ClickRank AI Toolkit | **Date:** 2026-03-22 | **URLs tested:** growreach.app

---

## 🚨 CRITICAL: All AI Crawlers Blocked by robots.txt

**AI Model Index Score: 0/100 — Needs Work**

Every major AI answer engine crawler is blocked by `robots.txt`:

| AI Crawler | Bot Name | Status |
|-----------|---------|--------|
| OpenAI ChatGPT | GPTBot | ❌ Blocked by robots.txt |
| OpenAI ChatGPT | ChatGPT-User | ❌ Blocked by robots.txt |
| Anthropic Claude | ClaudeBot | ❌ Blocked by robots.txt |
| Anthropic Claude | anthropic-ai | ❌ Blocked by robots.txt |
| Google Gemini | Google-Extended | ❌ Blocked by robots.txt |
| Perplexity AI | PerplexityBot | ❌ Blocked by robots.txt |
| CCBot | CCBot | ❌ Blocked by robots.txt |

**This means GrowReach content CANNOT appear in ChatGPT, Claude, Gemini, or Perplexity answers.** No matter how good the content is, these bots cannot crawl it.

### Fix — Edit robots.txt (2 minutes)

Add these lines to `growreach.app/robots.txt`:

```
# Allow AI crawlers for GEO visibility
User-agent: GPTBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: Google-Extended
Allow: /

User-agent: anthropic-ai
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: CCBot
Allow: /
```

**Priority: IMMEDIATE. Fix this before anything else.**

---

## AI Model Compatibility Score (How Well AI Understands growreach.app)

**Overall Score: ~65/100 — "Good"**

| AI Model | Score | Status |
|---------|-------|--------|
| ChatGPT | 60% | 🟡 Good |
| Gemini | 80% | 🟢 Excellent |
| Mistral | 80% | 🟢 Excellent |
| Cohere | 80% | 🟢 Excellent |
| Claude | 60% | 🟡 Good |
| Llama | 40% | 🔴 Fair |

### What's Working ✅
- **Brand mentions** — "GrowReach" mentioned frequently with positive sentiment
- **Contextual relevance** — content aligns well with LinkedIn automation keywords
- **Privacy compliance** — privacy policy and GDPR signals present
- **Content quality** — comprehensive, well-structured, directly addresses user questions
- **Content clustering** — thematically coherent, easy for AI to group with related topics

### What's Failing ❌ (Ordered by impact)

#### 1. No Review Platform Links (ChatGPT + Gemini fail)
**What AI sees:** "No mentions of G2, Capterra, Trustpilot, Google Reviews, or Yelp"
**Why it matters:** AI models use social proof signals to rank trustworthiness. Zero review links = lower citation probability.
**Fix:** Get listed on G2 + Capterra NOW. Add links to these profiles from the homepage and comparison pages.

#### 2. No Authoritative External Source Links (ChatGPT + Gemini fail)
**What AI sees:** "Content doesn't mention or link to authoritative sources — news outlets, industry publications, academic sources"
**Why it matters:** AI engines weight content that references credible third parties much higher for citation.
**Fix:** Add 2-3 external authority links per page. Examples:
- Link to LinkedIn's official statistics/blog when citing LinkedIn numbers
- Link to industry reports (HubSpot, LinkedIn Marketing Solutions) when making claims
- Link to news coverage of GrowReach if any exists

#### 3. No Schema Markup (Gemini fail)
**What AI sees:** "HTML doesn't contain schema markup for brand, products, or services"
**Why it matters:** Schema helps AI engines understand entity relationships (brand = GrowReach, product type = LinkedIn automation, features = commenting)
**Fix:** Add JSON-LD schema for:
- `Organization` schema on homepage (brand, logo, sameAs links to G2/LinkedIn/etc.)
- `Product` schema on pricing page
- `FAQPage` schema on pages with Q&A sections
- `Article` schema on blog/learn pages

#### 4. Business Intelligence Data Missing (Mistral)
**What AI sees:** "Lacks corporate data, KPIs, or metrics suitable for business analysis"
**Why it matters:** When someone asks ChatGPT "what results does GrowReach get?" — if there are no stats, AI can't answer confidently.
**Fix:** Add quantifiable metrics on the homepage and landing pages:
- "X customers"
- "Average X% increase in LinkedIn engagement"
- "X comments sent per month"
- Even rough stats beat zero stats

---

## Priority Action Plan

### 🔴 Do Today (zero content writing)
1. **Fix robots.txt** — allow GPTBot, ClaudeBot, Google-Extended, PerplexityBot → immediately makes GrowReach indexable by ALL AI engines
2. **Register on G2** (https://sell.g2.com) — free listing, takes 30 min → fixes review platform failure for ChatGPT
3. **Register on Capterra** (https://www.capterra.com/vendors/sign-up) — free, takes 30 min

### 🟡 This Week (1-2 hours)
4. **Add Organization schema** to growreach.app homepage — `sameAs: [G2 profile, LinkedIn page, Capterra listing]`
5. **Add Product schema** to pricing page — name, description, price range
6. **Add 3 authority links** to homepage — link to LinkedIn official stats, industry data
7. **Add quantitative claims** — "Join X LinkedIn users growing with GrowReach" (any number beats nothing)

### 🟢 Within 30 Days
8. **Connect Google Search Console** to ClickRank — this unlocks bulk title/meta optimization
9. **Add the ClickRank JS snippet** to growreach.app — enables live meta tag optimization without redeploy
10. **Run Bulk Title Optimization** for all 10 comparison pages + feature pages
11. **Set up AI Overview Tracker** for target keywords: "linkedin comment automation", "linkedin engagement tool"

---

## Why This Matters for GrowReach

GrowReach's ICP (B2B sales, LinkedIn power users) **regularly asks AI assistants** for tool recommendations:
- "What's the best LinkedIn comment automation tool?"
- "How do I grow my LinkedIn engagement?"
- "Best alternative to Expandi/Waalaxy for LinkedIn?"

Today, GrowReach **cannot appear in any of these answers** because:
1. robots.txt blocks all AI crawlers (no indexing)
2. No review platform presence (no social proof signals for citation)
3. No schema markup (AI can't confidently identify the entity)

After the 3 fixes above (robots.txt + G2/Capterra + schema), GrowReach becomes eligible to be cited. Combined with the content strategy (comparison pages, learn articles), this is how organic AI citations are built.

---

## ClickRank Tool URLs (for manual use)

| Tool | URL |
|------|-----|
| AI Model Index Checker | https://app.clickrank.ai/en/ai-toolkit/model-index |
| AI Compatibility Tool | https://app.clickrank.ai/en/ai-toolkit/model-compatibility |
| AI Overview Tracker | https://app.clickrank.ai/en/ai-toolkit/overview-tracker |
| AI Search Volume | https://app.clickrank.ai/en/ai-toolkit/search-volume |
| AI Content Writer | https://app.clickrank.ai/en/ai-toolkit/ai-writer |
| Bulk Titles | https://app.clickrank.ai/en/bulk/titles |
| Pages (GSC data) | https://app.clickrank.ai/en/pages |

**Script usage:**
```bash
node scripts/clickrank-research.js --mode ai-index        # Re-check after robots.txt fix
node scripts/clickrank-research.js --mode compatibility   # Re-run after schema added
node scripts/clickrank-research.js --mode keywords        # AI search volume research
node scripts/clickrank-research.js --mode explore         # Explore dashboard
```

<!-- progress: phase=6, iteration=3, section="GEO-AUDIT complete" -->
