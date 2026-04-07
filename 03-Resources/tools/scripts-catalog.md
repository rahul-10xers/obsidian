---
type: reference
date: 2026-04-07
project: harness
tags: [scripts, tools, automation, catalog]
---

# Scripts Catalog

> Every script in `ClaudeCode2.0/scripts/`. Claude reads this to know what operational tools are available without scanning the directory.
> **Repo path:** `scripts/` | **Runtime:** `node scripts/<name>.js [args]`

---

## WordPress Publishing

| Script | Invocation | What it does |
|---|---|---|
| `wp-publish.js` | `node scripts/wp-publish.js --file path/to/post.md` | Push a single article to WordPress as draft. Validates silo URL prefix. |
| `wp-push-comparison.js` | `node scripts/wp-push-comparison.js` | Push a comparison page draft. Parent ID 890 (`/comparison/`). |
| `wp-push-post.js` | `node scripts/wp-push-post.js` | Push a `/learn/` post draft to WordPress. |
| `create-comparison-pages.js` | `node scripts/create-comparison-pages.js` | Bulk-create comparison pages (idempotent, safe to re-run). |
| `fix-permalink-structure.js` | `node scripts/fix-permalink-structure.js` | Fix WP permalink to `/%category%/%postname%/` via WP Admin. Run before publishing. |
| `set-site-noindex.js` | `node scripts/set-site-noindex.js` | Set WP `blog_public=0` (discourage search engines). Site-wide safety net. |
| `add-wpcode-snippet.js` | `node scripts/add-wpcode-snippet.js` | Add a PHP snippet via the WPCode plugin. |
| `check-wpcode-status.js` | `node scripts/check-wpcode-status.js` | Check if a WPCode snippet is active. |

---

## SEO Research — NeuronWriter

| Script | Invocation | What it does |
|---|---|---|
| `neuronwriter-research.js` | `node scripts/neuronwriter-research.js --keyword "kw"` | **Main entry point.** Create query, wait for results, output all data. Required before every SEO page. |
| `neuronwriter-create-query.js` | `node scripts/neuronwriter-create-query.js --keyword "kw"` | Create a new NeuronWriter query and wait for results. Lower-level than above. |
| `neuronwriter-read-queries.js` | `node scripts/neuronwriter-read-queries.js` | List all queries in the growreach.app NeuronWriter project. |
| `neuronwriter-read-analysis.js` | `node scripts/neuronwriter-read-analysis.js --id <query_id>` | Read a specific NeuronWriter analysis — keyword data, SERP, NLP terms. |
| `neuronwriter-keyword-research.js` | `node scripts/neuronwriter-keyword-research.js --keyword "kw"` | Explore NeuronWriter API for keyword queries and SERP data. |
| `test-neuronwriter.js` | `node scripts/test-neuronwriter.js` | Verify NeuronWriter cookie auth works. Run if auth fails. |

---

## Web Scraping

| Script | Invocation | What it does |
|---|---|---|
| `scrape.js` | `node scripts/scrape.js --url <URL> --mode text\|html\|links\|json [--wait 3000]` | **Main scraper.** Playwright headless. Use when WebFetch fails on JS pages. |
| `fetch-page.js` | `node scripts/fetch-page.js <url> [--output file.txt]` | Simpler Playwright fetcher. Saves to file. |
| `dripify-intelligence.js` | `node scripts/dripify-intelligence.js` | Full competitor intelligence scrape for dripify.com. |
| `scrape-taplio.js` | `node scripts/scrape-taplio.js` | Scrape Taplio landing pages for competitor intelligence. |
| `scrape-connectsafely.js` | `node scripts/scrape-connectsafely.js` | Scrape ConnectSafely pages. |
| `scrape-joinvalley.js` | `node scripts/scrape-joinvalley.js` | Scrape JoinValley pages. |
| `scrape-ligosocial.js` | `node scripts/scrape-ligosocial.js` | Scrape LigoSocial pages. |
| `scrape-reepl.js` | `node scripts/scrape-reepl.js` | Scrape Reepl pages. |

---

## Competitor Research

| Script | Invocation | What it does |
|---|---|---|
| `clickrank-research.js` | `node scripts/clickrank-research.js` | ClickRank AI competitor research without JS snippet. |
| `test-transparency.js` through `test-transparency5.js` | various | Google Ads Transparency scraper iterations. Use `test-transparency5.js` (latest). |
| `fill-swipe-file.js` | `node scripts/fill-swipe-file.js` | Fills GrowReach cells in `content/social-proof-swipe.html`. |

---

## SaaS Tool Auth Tests

Run these when a tool's auth fails. Each verifies cookies/tokens are valid.

| Script | Invocation | Tool it tests |
|---|---|---|
| `test-contextminds.js` | `node scripts/test-contextminds.js` | ContextMinds (cookie auth) |
| `test-replydaddy.js` | `node scripts/test-replydaddy.js` | ReplyDaddy (Supabase localStorage) |
| `test-seopilot.js` | `node scripts/test-seopilot.js` | SEOPilot (localStorage JWT) |
| `test-trendfynd.js` | `node scripts/test-trendfynd.js` | TrendFynd (better-auth session cookie) |
| `test-clickrank.js` | `node scripts/test-clickrank.js` | ClickRank (cookie auth) |
| `test-adnova.js` | `node scripts/test-adnova.js` | Adnova (localStorage auth) |
| `test-adsby.js` | `node scripts/test-adsby.js` | Adsby (Refresh-Token) |
| `test-pridesem.js` | `node scripts/test-pridesem.js` | PrideSem |
| `explore-contextminds.js` | `node scripts/explore-contextminds.js` | Explore ContextMinds UI workflow |

---

## Task & Session Management

| Script | Invocation | What it does |
|---|---|---|
| `task-tracker.js` | `node scripts/task-tracker.js start\|update\|done\|block` | GitHub Issues task tracker. Creates issues at task start, closes at end. |
| `sync-context.js` | `node scripts/sync-context.js` | Sync context between sessions. |

---

## Tool Selection Guide

```
Need to publish to WordPress?        → wp-publish.js or wp-push-post.js
Need SEO brief before writing?       → neuronwriter-research.js
Need to scrape a JS-heavy page?      → scrape.js --mode text --wait 3000
Tool auth failing?                   → run test-<toolname>.js first
Competitor landing page intel?       → scrape.js on competitor URLs
Google Ads creative spy?             → test-transparency5.js
Track a task in GitHub Issues?       → task-tracker.js start
```
