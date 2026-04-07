# pSEO Notebook Research — Building and Scaling Programmatic SEO Systems
> Source: NotebookLM notebook c46a44df-8628-4fcf-9a0a-5a4918a736b5 (50 sources)
> Queried: March 2026 | Status: PARTIAL — 3 of ~8 planned queries completed
> Next session: re-query remaining topics AND save each as NotebookLM note

---

## Q1: URL Structure — /blog vs /learn vs flat

**Decision: Use /learn/ for GrowReach** (not /blog/)

- Flat structure DISCOURAGED — dilutes authority, Google can't map topic relationships
- Hub-and-spoke nested structure wins: central hub → multiple spokes → cross-links back
- /learn/ or /resources/ signals educational hub — better topical authority positioning than /blog/
- Successful SaaS directories: /use-cases/, /integrations/, /templates/, /learn/

**Recommended GrowReach URL silos:**
- /learn/[slug] — informational SEO articles (CFBR, engagement guides, etc.)
- /free-tools/[tool-name] — free tool pages
- /use-cases/[slug] — use case pages
- /compare/[growreach-vs-competitor] — comparison pages
- /features/[slug] — product feature pages

**Technical requirements:**
- Every page reachable within 3 clicks of homepage (crawl efficiency)
- Breadcrumb schema reinforces hierarchy
- Segmented sitemaps by category (not one giant sitemap)
- Canonical tags prevent duplicate URL penalties
- llms.txt at root guides AI crawlers (ChatGPT, Perplexity, Claude)

---

## Q2: Complete pSEO Workflow — 7 Phases

### Phase 1: Keyword Research
- Find scalable PATTERNS: head term + modifiers
- GrowReach pattern: "linkedin comment" + [automation / generator / examples / for sales / for founders]
- Tools: SemRush Keyword Magic Tool (user-operated) + NeuronWriter for SERP depth
- Output: keyword matrix — each row = one page

### Phase 2: Unique Data Collection
- Pure AI content without unique data = Google penalty risk
- GrowReach unique data: LinkedIn algorithm stats, engagement benchmarks, comment examples by industry/role
- This is our competitive moat — competitors can't replicate our data layer

### Phase 3: Keyword Matrix (Google Sheets)
- Columns: keyword, slug, H1, meta title, meta description, intro variable, template type
- Each row = one published page
- Airtable for relational data if needed (update one field → updates all pages)

### Phase 4: Content Generation
- NeuronWriter API → NLP terms per keyword → passed as mandatory inclusions to Claude
- Claude writes unique content per page using NLP terms + matrix data
- Dynamic prompts (not copy-paste) = genuinely unique content

### Phase 5: Publishing to WordPress
- REST API: POST /wp-json/wp/v2/posts
- Fields: title, content (HTML), slug, status=draft, categories, meta (via Rank Math API Manager)
- 1-2 second delays between posts (server bottleneck protection)
- Always draft first — human review before publish

### Phase 6: SEO + GEO Optimization (every page)
- JSON-LD schema: FAQPage + Article + Organization
- llms.txt at root
- Rank Math meta fields via Rank Math API Manager plugin (extends REST API)
- All text in initial HTML — no JS-rendered content (AI crawlers can't read it)

### Phase 7: Monitoring & Scaling
- Indexation ratio target: >60%
- Engagement within 30% of site average
- Scale gradually: 20-30% more pages/month
- Tools: Google Search Console + GA4

---

## Q3: Claude Code + WordPress REST API Automation

**Core insight:** Claude Code is not just a writer — it's a terminal-based orchestrator that can manage the entire pSEO pipeline end-to-end via MCP and REST API.

### Authentication
- WordPress Application Password: Users > Profile > Application Passwords
- Already configured in CREDENTIALS.md: username=rahul, password=HQss dyby ZbFv Qz6p hQ7h jEHi
- REST API base: https://growreach.app/wp-json/wp/v2/

### WordPress fields set per published page
**Standard fields (REST API native):**
- title: SEO-friendly H1
- content: AI-generated HTML body
- status: draft (always — human review gate)
- slug: clean keyword-rich URL
- categories: numerical IDs for topic silos

**SEO plugin fields (require Rank Math API Manager plugin):**
- rank_math_title → SEO title tag
- rank_math_description → meta description
- rank_math_focus_keyword → primary keyword
- rank_math_canonical_url → canonical

### The automation script pattern (scripts/wp-publish.js — TO BUILD)
```
1. Read keyword matrix row (from Google Sheets or local CSV)
2. Get NeuronWriter NLP terms for the keyword
3. Claude generates HTML article using NLP terms
4. POST to /wp-json/wp/v2/posts with all fields
5. POST to /wp-json/rank-math-api/v1/update-meta with SEO fields
6. Wait 1-2 seconds
7. Next row
```

### Key warning from sources
- Server bottleneck is the #1 failure point — Claude generates faster than WordPress can accept
- Implement 1-2 second delays between API calls
- Test with 1 page first before running bulk
- One case study: Claude Code successfully pushed 460 posts in bulk using this pattern

---

## REMAINING QUERIES TO RUN NEXT SESSION (save each as note)

1. Topical authority + hub-and-spoke content model for SaaS — how many pages needed?
2. GEO (Generative Engine Optimization) — how to get cited by ChatGPT/Perplexity/AI Overviews
3. WordPress template structure for pSEO — technical requirements, schema, Core Web Vitals
4. Internal linking strategy at scale — how to automate it
5. NeuronWriter API deep integration with Claude Code workflow

---

## NEXT SESSION INSTRUCTIONS

1. Open notebook c46a44df-8628-4fcf-9a0a-5a4918a736b5
2. Run each remaining query above ONE AT A TIME
3. IMMEDIATELY save each response as a NotebookLM note (mcp__notebooklm-mcp__note action=create)
4. After all queries done, synthesize into GrowReach pSEO Master Plan
5. Present plan to user for review before any execution
