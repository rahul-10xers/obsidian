# pSEO Audit Results — 4 Page Types
> Date: March 2026 | Status: Content written, pending developer template work

---

## NeuronWriter Query IDs (active — reuse to avoid re-running)

| Page Type | Keyword | Query ID | Score Before | Score After |
|-----------|---------|----------|-------------|-------------|
| Feature | linkedin auto comment | 768b37ab0fd9a15c | 20 | 43 (section alone) |
| Use-case | linkedin personal branding tool | f69c0cf561250b0c | 14 | 39 (section alone) |
| Comparison | expandi alternative | 8899ea2fa8f21f7b | 6 | 35 (section alone) |
| Free tool | linkedin engagement rate calculator | 9bb5675a40396b41 | 24 | 27 (section alone) |

Combined with existing page content: feature 48, use-case 44, comparison 36, free-tool 39

---

## Content Files Written (ready to push)

- `content/feature-auto-comment-linkedin.html` — 2,150 words
- `content/usecase-personal-branding.html` — 2,651 words
- `content/comparison-expandi-alternative.html` — 3,355 words
- `content/freetool-engagement-rate-calculator.html` — 1,332 words

---

## Key Strategic Finding

- **Use-case page** — can rank #1 immediately. Top competitor (SuperGrow.ai) scores 20, our content scores 39.
- **Feature + Comparison** — landing pages won't beat dedicated blog posts (score 69–86). Need /learn/ articles for primary keywords. Landing pages capture branded traffic.
- **Free-tool** — needs FAQ section (4 PAA questions answered). Score needs to go from 39 → 60+.

---

## Developer Tasks (pending)

1. Build Elementor Pro Single Post template for /learn/ articles (Post Content widget in center)
2. Add `<div class="seo-content">` block at bottom of each of 4 page templates (comparison, feature, use-case, free-tools)
3. Use CloneWebX to migrate existing 62 GHL pages → WordPress (already installed via SoftLite plugin)
4. Rank Math API Manager plugin — install when ready to switch from Yoast to Rank Math (optional)

---

## WordPress State

- Post 817 (CFBR article) — draft, Yoast meta set, content pushed ✅
- Post 815 — old duplicate CFBR push, delete when 817 is confirmed good
- Category 7 = "Learn" — already exists
- Yoast REST API meta: enabled via functions.php snippet ✅

---

## Pending SemRush Data

Need keyword overview screenshots for:
1. `linkedin auto comment`
2. `linkedin personal branding tool`
3. `expandi alternative`
4. `linkedin engagement rate calculator`

Use `claude --chrome` session to access directly once connected.
