# GrowReach SEO Design & Developer Guide
**For: Designer + Developer**
**Last updated: March 2026 (v2 — expanded with E-E-A-T, author system, sidebar TOC, GEO, IndexNow)**

---

## Overview: How Content Gets Published

Claude researches keywords → writes SEO-optimized HTML → pushes it as a **WordPress draft post** (category: Learn). The template system you build makes every draft look pixel-perfect and technically SEO-ready automatically. The moment it's published, it should be indexable without any manual work.

### The Content Pipeline

```
Claude runs NeuronWriter keyword research
        ↓
Claude writes SEO-optimized HTML (1,000–2,700 words)
  - H1 with primary keyword
  - H2 sections with secondary keywords
  - H3 FAQ questions + answers
  - Tables, lists, CTAs
        ↓
Claude pushes to WordPress as DRAFT
  - Category: 7 (Learn)
  - Slug: matches target URL exactly
  - Yoast meta: focus keyword, meta description, SEO title
        ↓
Developer template auto-applies (zero manual steps):
  ✅ Author byline + reading time + published date
  ✅ Breadcrumbs (Yoast)
  ✅ Sticky sidebar TOC (desktop) / inline collapsible (mobile)
  ✅ Tables wrapped for mobile scroll
  ✅ CTA block auto-converted to styled component
  ✅ Internal links auto-injected
  ✅ Article + FAQ + Organization + ProfilePage schema
  ✅ OG image auto-generated
  ✅ Social sharing buttons
        ↓
Designer QA (template already applied — visual review only)
        ↓
One-click Publish
        ↓
Rank Math auto-submits to Google Indexing API + IndexNow (Bing/ChatGPT)
        ↓
Page is live, indexed, and ranking-ready
```

---

## Part 1: For the Designer

The content Claude generates is clean semantic HTML. Your job is to make every HTML element look excellent without touching the content itself. Every design decision below has a SEO reason — understand it before overriding.

---

### 1.1 Page Layout: Two-Column on Desktop

This is the most important structural change from v1. Semrush, HubSpot, and every high-ranking content site use this layout for long-form content. It directly impacts SERP sitelinks.

```
DESKTOP (≥ 1024px):
┌─────────────────────────────────────────────────────────┐
│  [Header / Nav]                                          │
│  [Breadcrumbs]                                           │
│  [Post Meta: Author · Date · Reading time]               │
├─────────────────────────────┬───────────────────────────┤
│                             │  ┌─────────────────────┐  │
│  [H1]                       │  │  In this article    │  │
│                             │  │  1. Section one     │  │
│  [Content Body]             │  │  2. Section two     │  │
│   ├─ <hr> dividers          │  │  3. Section three   │  │
│   ├─ <h2> sections          │  │  ...                │  │
│   ├─ <p> paragraphs         │  └─────────────────────┘  │
│   ├─ <ul>/<ol> lists        │  (sticky — follows scroll) │
│   ├─ <table> comparisons    │                           │
│   └─ <h3> FAQ questions     │                           │
│                             │                           │
│  [CTA Block]                │                           │
│  [Author Bio Card]          │                           │
│  [Social Share Buttons]     │                           │
├─────────────────────────────┴───────────────────────────┤
│  [Footer]                                                │
└─────────────────────────────────────────────────────────┘

Content column: max 720px
Sidebar: 260px fixed, 40px gap from content
Sticky TOC: position: sticky; top: 80px (below nav)

MOBILE (< 1024px):
Single column. TOC becomes a collapsible block above the H1.
Author meta above H1. Social share at bottom.
```

---

### 1.2 Typography Hierarchy

These heading levels are deliberate — they map to NeuronWriter's NLP term requirements and Google's semantic understanding of the page. Never flatten, skip, or restyle H2s as H3s.

| Element | SEO Role | Design Spec |
|---------|----------|-------------|
| `<h1>` | Primary keyword. One per page. | 28–36px desktop, 22–26px mobile. Most prominent text on page. |
| `<h2>` | Secondary keywords. Each becomes a TOC anchor. | 20–26px. Noticeably larger than body. Enough visual weight to scan from a distance. |
| `<h3>` | Sub-sections, FAQ questions | 16–20px. Distinct from body but closer to it than H2. |
| `<p>` | Body copy | 16–18px, 1.6–1.8 line-height. Max 720px wide. Never full-bleed. |
| `<strong>` | NLP term emphasis | Bold weight only. Never a different color or underline — must look inline, not linked. |

**Font loading:** Developer implements `font-display: swap`. Designer specifies a system font fallback stack that doesn't cause layout shift (match cap-height and x-height of the web font as closely as possible).

---

### 1.3 Post Header Meta Row

Every page needs a meta row between the breadcrumbs and the H1. Semrush does this — it's an E-E-A-T signal Google quality raters look for.

**Design a compact single row (desktop) / stacked (mobile):**

```
[Author Photo 32px] Rahul · March 16, 2026 · Updated April 2, 2026 · 8 min read
```

- Author name links to their author profile page (e.g., `/author/rahul`)
- Published date: show once. "Updated" date: show only if post has been modified since publish
- Reading time: auto-calculated by developer (avg 200 words/min)
- Font: 13–14px, muted color. Should not compete with H1.
- Author photo: 32×32px circle, lazy-loaded with explicit width/height set

---

### 1.4 Author Byline Card (Bottom of Page)

A second, fuller author block appears at the bottom of the content, before the CTA. This is the E-E-A-T credibility signal Google quality raters look for when evaluating "is this content from a real expert?"

**Design an author card component:**

```
┌──────────────────────────────────────────────────────────┐
│  [Photo 64px]  Rahul                                     │
│                Founder, GrowReach                        │
│                [LinkedIn ↗]  [Author page: All articles] │
│                                                          │
│  2-3 sentence bio about expertise in LinkedIn growth,    │
│  automation, and B2B marketing. Written in third person. │
└──────────────────────────────────────────────────────────┘
```

- Subtle border or light background to visually separate from content
- Author photo: 64×64px circle
- Bio: pulled from WordPress user profile description field
- "All articles" link → author archive page (e.g., `/author/rahul`)
- LinkedIn link: opens in new tab, `rel="noopener"`

---

### 1.5 Author Profile / Archive Page

Semrush's author pages (e.g., `semrush.com/blog/user/carlos/`) are a deliberate SEO pattern. They tell Google this is a real person with a documented body of expertise. Design an author archive page layout:

```
[Author Photo 120px — circle]
[Author Name — large]
[Title/Role at GrowReach]
[LinkedIn link] [Twitter link]
[Bio — 3-4 sentences, expertise-focused]

────────────────────────────────
Articles by [Author Name]
────────────────────────────────
[Article card grid — same design as blog listing]
  - Title
  - Category tag (Use Case / Feature / Comparison)
  - Published date
  - Reading time
  - Short excerpt
```

This page is auto-generated by WordPress when author archives are enabled. The developer configures it; the designer skins it.

---

### 1.6 Sticky Sidebar Table of Contents

The sidebar TOC is how Semrush, HubSpot, and high-ranking content sites drive SERP sitelinks. Sitelinks are the expandable sub-links that appear under a search result — they increase CTR by 20–40%+.

**Design specs:**

```
┌─────────────────────┐
│  In this article    │  ← label, muted, 12px uppercase
├─────────────────────┤
│  1. Section one     │  ← active state: brand color left border
│  2. Section two     │     + slightly bolder
│  3. Section three   │  ← inactive: muted text, no border
│  4. Section four    │
│  ...                │
└─────────────────────┘
```

- Position: `sticky`, top of viewport minus nav height
- Width: 240–260px
- Active section: highlighted as user scrolls (IntersectionObserver — dev implements)
- Smooth scroll on click (CSS `scroll-behavior: smooth`)
- On mobile (< 1024px): replaced by a collapsible `<details><summary>` block inline above the H1
- TOC text: truncate long H2 titles at 45 chars with ellipsis

---

### 1.7 Section Dividers (`<hr />`)

Every H2 section is preceded by `<hr />`. Do NOT hide these.

```css
hr {
    border: none;
    border-top: 1px solid #E5E7EB;
    margin: 40px 0;
}
@media (max-width: 768px) {
    hr { margin: 28px 0; }
}
```

---

### 1.8 Lists (`<ul>` and `<ol>`)

**`<ul>` (bullet points):**
- Custom bullet via CSS `::before` — brand color dot or SVG checkmark
- `list-style: none`
- 8–12px gap between items
- `<strong>` often opens each `<li>` as the key term. It should sit inline with the following text — not styled as a sub-heading.

**`<ol>` (numbered steps):**
- Numbers visually prominent — large circle badge in brand color
- `<strong>` opens each step name
- Generous vertical padding — users scan step lists; they are high-value SEO surfaces for featured snippets

---

### 1.9 Comparison Tables (`<table>`)

Used heavily on comparison pages. Critical for SEO — Google indexes table data as structured content.

**Desktop:**
- Header row: brand color background, white text
- GrowReach column: highlighted with subtle brand tint background
- Alternating row colors: `#F9FAFB` / white
- ✅/❌ cells: center-aligned, 18px+
- `text-align: left` for first column (feature names), `center` for data columns

**Mobile (developer wraps in scroll container — see 2.8):**
- `overflow-x: auto` wrapper — never break the `<table>` structure
- Minimum column width: 120px
- Consider sticky first column on complex tables

**Never:** convert tables to CSS grid or flexbox divs. Google cannot reliably read reformatted tables.

---

### 1.10 FAQ Sections

FAQ sections use `<h3>` (question) + `<p>` (answer). They must be fully visible — never in accordions.

**Why:** Google gives partial credit to hidden accordion content. Fully visible answers score 30–50% higher in FAQ schema eligibility tests.

**Design each Q&A pair:**
- Light card background or left border accent
- `<h3>` styled with a "Q:" or question icon prefix in brand color
- `<p>` answer: normal body styling, 12px gap below `<h3>`
- 24px vertical gap between each Q&A pair

---

### 1.11 CTA Block (Bottom of Content)

Developer auto-converts this HTML pattern (see 2.6) into a styled component. Design the component:

```
┌───────────────────────────────────────────────────────────┐
│  [Brand gradient or solid background]                     │
│                                                           │
│  Start your 7-day free trial.                             │
│  Plans from $29/month.                                    │
│                                                           │
│  [Start Free Trial ←primary btn]  [See Plans ←ghost btn] │
│                                                           │
│  🔒 No credit card required  ·  ✓ Cancel anytime         │
└───────────────────────────────────────────────────────────┘
```

Centered on desktop, stacked on mobile.

---

### 1.12 Social Share Buttons

At the bottom of the page (after CTA), include share buttons for LinkedIn, Twitter/X, and a copy-link button. Small, icon-only or icon+label. Not intrusive.

**Why this matters for SEO:** Social shares generate backlinks and branded searches — both indirect ranking signals. LinkedIn shares specifically drive referral traffic that signals relevance to Google for LinkedIn-topic content.

---

### 1.13 Breadcrumbs

Above the H1. Small, muted:

```
Home › Use Cases › GrowReach for LinkedIn Influencers
```

| URL Pattern | Breadcrumb Trail |
|-------------|-----------------|
| `/use-case/*` | Home › Use Cases › [Title] |
| `/feature/*` | Home › Features › [Title] |
| `/comparison/*` | Home › Comparisons › [Title] |
| `/free-tool/*` | Home › Free Tools › [Title] |
| `/what-is-*` | Home › Learn › [Title] |

- 13–14px, muted color
- Current page: not linked, slightly darker
- Separator: `›`
- BreadcrumbList JSON-LD: auto-injected by Yoast

---

## Part 2: For the Developer

Everything here runs automatically via WordPress filters and template code. Once built, zero manual work per page.

---

### 2.1 WordPress Template Setup

All SEO content posts use **Category 7 (Learn)**. Build a template that auto-activates for this category.

```php
// In single.php or via template_include filter:
function growreach_load_learn_template( $template ) {
    if ( is_single() && in_category(7) ) {
        $custom = get_template_directory() . '/template-parts/single-learn.php';
        if ( file_exists($custom) ) return $custom;
    }
    return $template;
}
add_filter('template_include', 'growreach_load_learn_template');
```

**`single-learn.php` must render in this order:**
1. Breadcrumbs (Yoast)
2. Post meta row: author photo + name + date + updated date + reading time
3. H1 (inside `the_content()` — do NOT also call `the_title()` as H1)
4. Content column + sticky sidebar TOC (two-column flex/grid layout)
5. `the_content()` filtered through all content filters below
6. Author bio card
7. Social share buttons
8. Related posts (optional — see 2.17)

**URL structure:** Use `/%postname%/` permalink setting. Implement parent pages (`use-case`, `feature`, `comparison`) as WordPress Pages. Child content pages nest under them for clean URLs: `/use-case/growreach-for-linkedin-influencers`.

---

### 2.2 Post Meta: Author, Date, Reading Time

```php
function growreach_post_meta( $post_id = null ) {
    $post_id = $post_id ?: get_the_ID();
    $author_id = get_post_field('post_author', $post_id);

    $author_name   = get_the_author_meta('display_name', $author_id);
    $author_url    = get_author_posts_url($author_id);
    $author_avatar = get_avatar_url($author_id, ['size' => 32]);

    $pub_date  = get_the_date('F j, Y', $post_id);
    $mod_date  = get_the_modified_date('F j, Y', $post_id);
    $show_mod  = ($mod_date !== $pub_date);

    $word_count   = str_word_count(strip_tags(get_post_field('post_content', $post_id)));
    $reading_time = max(1, round($word_count / 200));

    ?>
    <div class="post-meta">
        <img src="<?= esc_url($author_avatar) ?>" width="32" height="32" alt="<?= esc_attr($author_name) ?>" class="author-avatar" loading="lazy">
        <a href="<?= esc_url($author_url) ?>" class="author-name"><?= esc_html($author_name) ?></a>
        <span class="meta-sep">·</span>
        <time datetime="<?= get_the_date('c', $post_id) ?>"><?= $pub_date ?></time>
        <?php if ($show_mod): ?>
            <span class="meta-sep">·</span>
            <span class="updated-date">Updated <time datetime="<?= get_the_modified_date('c', $post_id) ?>"><?= $mod_date ?></time></span>
        <?php endif; ?>
        <span class="meta-sep">·</span>
        <span class="reading-time"><?= $reading_time ?> min read</span>
    </div>
    <?php
}
```

---

### 2.3 Author System (E-E-A-T)

This is what Semrush does with their author pages. It's an E-E-A-T signal — Google quality raters look for named, credentialed authors with a body of work. Build it once; it applies to every page automatically.

**WordPress setup:**
1. Enable author archives: `Settings → Reading` → ensure author archives are not blocked in `robots.txt`
2. Yoast → Search Appearance → Authors → ensure author archives are indexable (do NOT set to noindex — these pages pass authority)
3. For each author, fill in WordPress user profile:
   - Display name (used in byline)
   - Biographical info (3–4 sentences, expertise-focused, third person)
   - Website URL (their LinkedIn profile)
   - Profile photo (upload via Simple Local Avatars plugin)

**Author bio card (bottom of every post):**

```php
function growreach_author_bio_card() {
    if ( ! is_single() || ! in_category(7) ) return;

    $author_id  = get_post_field('post_author', get_the_ID());
    $name       = get_the_author_meta('display_name', $author_id);
    $bio        = get_the_author_meta('description', $author_id);
    $url        = get_author_posts_url($author_id);
    $avatar     = get_avatar_url($author_id, ['size' => 64]);
    $linkedin   = get_the_author_meta('url', $author_id); // LinkedIn stored in website field

    if ( ! $bio ) return;

    echo '<div class="author-bio-card">
        <img src="' . esc_url($avatar) . '" width="64" height="64" alt="' . esc_attr($name) . '" loading="lazy" class="author-photo">
        <div class="author-bio-content">
            <p class="author-bio-name">' . esc_html($name) . '</p>
            <p class="author-bio-text">' . esc_html($bio) . '</p>
            <div class="author-bio-links">
                <a href="' . esc_url($url) . '">All articles →</a>
                ' . ($linkedin ? '<a href="' . esc_url($linkedin) . '" target="_blank" rel="noopener">LinkedIn ↗</a>' : '') . '
            </div>
        </div>
    </div>';
}
add_action('growreach_after_content', 'growreach_author_bio_card');
```

**Author archive page** is auto-generated by WordPress. Skin it with the design from section 1.5. Ensure it includes:
- Full author profile header (photo, bio, links)
- Post listing with title, category, date, excerpt
- Noindex: OFF — author pages must be indexable

---

### 2.4 Breadcrumbs

```php
<?php if ( function_exists('yoast_breadcrumb') ) {
    yoast_breadcrumb('<nav class="breadcrumbs" aria-label="Breadcrumb">', '</nav>');
} ?>
```

Configure in Yoast: Search Appearance → Breadcrumbs → Enable. Separator: `›`. Taxonomy: Category.
BreadcrumbList JSON-LD is output automatically by Yoast.

---

### 2.5 Sidebar TOC + Anchor IDs

The sidebar TOC generates SERP sitelinks — expandable sub-links under your search result. It also improves dwell time by letting users navigate to the section they care about.

```php
function growreach_build_toc( $content ) {
    if ( ! is_single() || ! in_category(7) ) return $content;

    $toc_items = [];

    // Inject IDs into every H2
    $content = preg_replace_callback(
        '/<h2>(.*?)<\/h2>/i',
        function($matches) use (&$toc_items) {
            $text = strip_tags($matches[1]);
            $id   = sanitize_title($text);
            $toc_items[] = ['id' => $id, 'text' => $text];
            return '<h2 id="' . esc_attr($id) . '">' . $matches[1] . '</h2>';
        },
        $content
    );

    if ( count($toc_items) < 3 ) return $content;

    // Inline TOC (mobile + fallback)
    $inline_toc = '<details class="toc-mobile"><summary class="toc-mobile-label">In this article</summary><ol class="toc-list">';
    foreach ($toc_items as $item) {
        $inline_toc .= '<li><a href="#' . esc_attr($item['id']) . '">' . esc_html(mb_substr($item['text'], 0, 60)) . '</a></li>';
    }
    $inline_toc .= '</ol></details>';

    // Sidebar TOC data (output as JSON for JS to build sidebar)
    $toc_json = json_encode($toc_items, JSON_UNESCAPED_UNICODE);
    $inline_toc .= '<script>window.growreachTOC = ' . $toc_json . ';</script>';

    return $inline_toc . $content;
}
add_filter('the_content', 'growreach_build_toc');
```

**Sidebar TOC (JS — renders the sticky sidebar):**

```javascript
// Runs after DOM ready. Builds sticky sidebar from window.growreachTOC
document.addEventListener('DOMContentLoaded', function() {
    if (!window.growreachTOC || window.innerWidth < 1024) return;

    const sidebar = document.querySelector('.toc-sidebar');
    if (!sidebar) return;

    const nav = document.createElement('nav');
    nav.className = 'toc-sidebar-nav';
    nav.innerHTML = '<p class="toc-sidebar-label">In this article</p>';

    const ol = document.createElement('ol');
    window.growreachTOC.forEach(item => {
        const li = document.createElement('li');
        li.innerHTML = `<a href="#${item.id}">${item.text}</a>`;
        ol.appendChild(li);
    });
    nav.appendChild(ol);
    sidebar.appendChild(nav);

    // Active section highlight on scroll
    const headings = document.querySelectorAll('h2[id]');
    const observer = new IntersectionObserver(entries => {
        entries.forEach(entry => {
            const link = sidebar.querySelector(`a[href="#${entry.target.id}"]`);
            if (link) link.parentElement.classList.toggle('active', entry.isIntersecting);
        });
    }, { rootMargin: '-20% 0px -75% 0px' });

    headings.forEach(h => observer.observe(h));
});
```

**Template layout (two-column):**

```html
<div class="learn-layout">
    <div class="learn-content">
        <!-- Breadcrumbs, meta, then the_content() here -->
    </div>
    <aside class="toc-sidebar">
        <!-- JS builds TOC here on desktop -->
    </aside>
</div>
```

```css
.learn-layout {
    display: grid;
    grid-template-columns: 1fr 260px;
    gap: 40px;
    max-width: 1060px;
    margin: 0 auto;
}
.toc-sidebar {
    position: relative;
}
.toc-sidebar-nav {
    position: sticky;
    top: 80px;
}
@media (max-width: 1023px) {
    .learn-layout { grid-template-columns: 1fr; }
    .toc-sidebar { display: none; }
    .toc-mobile { display: block; }
}
@media (min-width: 1024px) {
    .toc-mobile { display: none; }
}
```

---

### 2.6 Schema Markup (JSON-LD)

Inject all schema in `wp_head`. Four schemas per page.

#### 2.6a — Article Schema (with real author)

```php
function growreach_article_schema() {
    if ( ! is_single() || ! in_category(7) ) return;
    global $post;

    $author_id   = get_post_field('post_author', $post->ID);
    $author_name = get_the_author_meta('display_name', $author_id);
    $author_url  = get_author_posts_url($author_id);

    $schema = [
        '@context'        => 'https://schema.org',
        '@type'           => 'Article',
        'headline'        => get_the_title(),
        'description'     => get_post_meta($post->ID, '_yoast_wpseo_metadesc', true),
        'datePublished'   => get_the_date('c'),
        'dateModified'    => get_the_modified_date('c'),
        'url'             => get_permalink(),
        'mainEntityOfPage'=> get_permalink(),
        'author' => [
            '@type' => 'Person',
            'name'  => $author_name,
            'url'   => $author_url,
        ],
        'publisher' => [
            '@type' => 'Organization',
            'name'  => 'GrowReach',
            'url'   => 'https://growreach.app',
            'logo'  => [
                '@type' => 'ImageObject',
                'url'   => 'https://growreach.app/wp-content/uploads/growreach-logo.png',
            ],
        ],
        'image' => [
            '@type'  => 'ImageObject',
            'url'    => 'https://growreach.app/api/og?title=' . urlencode(get_the_title()),
            'width'  => 1200,
            'height' => 630,
        ],
    ];

    echo '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_UNESCAPED_UNICODE) . '</script>';
}
add_action('wp_head', 'growreach_article_schema');
```

#### 2.6b — FAQ Schema (auto-detected from H3+P pairs)

```php
function growreach_faq_schema() {
    if ( ! is_single() || ! in_category(7) ) return;
    global $post;

    preg_match_all('/<h3>(.*?)<\/h3>\s*<p>(.*?)<\/p>/is', $post->post_content, $matches, PREG_SET_ORDER);
    if ( empty($matches) ) return;

    $faq_items = [];
    foreach ($matches as $match) {
        $faq_items[] = [
            '@type'          => 'Question',
            'name'           => strip_tags($match[1]),
            'acceptedAnswer' => [
                '@type' => 'Answer',
                'text'  => strip_tags($match[2]),
            ],
        ];
    }

    $schema = [
        '@context'   => 'https://schema.org',
        '@type'      => 'FAQPage',
        'mainEntity' => $faq_items,
    ];

    echo '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_UNESCAPED_UNICODE) . '</script>';
}
add_action('wp_head', 'growreach_faq_schema');
```

> FAQ schema generates rich results in the SERP — Q&A pairs appear directly under the search result, expanding the page's real estate in Google.

#### 2.6c — Organization Schema with SameAs

Output site-wide (in `functions.php`, not just category 7). Tells Google what GrowReach is and links it to all verified profiles — a core E-E-A-T signal.

```php
function growreach_organization_schema() {
    if ( ! is_front_page() && ! is_single() ) return;

    $schema = [
        '@context' => 'https://schema.org',
        '@type'    => 'Organization',
        'name'     => 'GrowReach',
        'url'      => 'https://growreach.app',
        'logo'     => 'https://growreach.app/wp-content/uploads/growreach-logo.png',
        'sameAs'   => [
            'https://www.linkedin.com/company/growreach',
            'https://twitter.com/growreachapp',
            // Add Crunchbase, ProductHunt, G2, Capterra URLs as they're created
        ],
        'contactPoint' => [
            '@type'             => 'ContactPoint',
            'contactType'       => 'customer support',
            'email'             => 'support@growreach.app',
        ],
    ];

    echo '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_UNESCAPED_UNICODE) . '</script>';
}
add_action('wp_head', 'growreach_organization_schema');
```

#### 2.6d — ProfilePage Schema (on Author Archive Pages)

```php
function growreach_profilepage_schema() {
    if ( ! is_author() ) return;

    $author_id = get_queried_object_id();
    $name      = get_the_author_meta('display_name', $author_id);
    $bio       = get_the_author_meta('description', $author_id);
    $url       = get_author_posts_url($author_id);
    $linkedin  = get_the_author_meta('url', $author_id);

    $schema = [
        '@context'    => 'https://schema.org',
        '@type'       => 'ProfilePage',
        'url'         => $url,
        'mainEntity'  => [
            '@type'       => 'Person',
            'name'        => $name,
            'description' => $bio,
            'url'         => $url,
            'worksFor'    => [
                '@type' => 'Organization',
                'name'  => 'GrowReach',
                'url'   => 'https://growreach.app',
            ],
            'sameAs' => array_filter([$linkedin]),
        ],
    ];

    echo '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_UNESCAPED_UNICODE) . '</script>';
}
add_action('wp_head', 'growreach_profilepage_schema');
```

---

### 2.7 Auto-Convert CTA Block

Claude ends every page with:
```html
<p><strong>Start your 7-day free trial. Plans from $29/month. No credit card required.</strong></p>
```

Auto-replace with the styled component:

```php
function growreach_replace_cta_block( $content ) {
    if ( ! is_single() || ! in_category(7) ) return $content;

    $pattern = '/<p><strong>(Start your 7-day free trial[^<]*)<\/strong><\/p>/i';
    $replacement = '
        <div class="cta-block">
            <p class="cta-headline">$1</p>
            <div class="cta-buttons">
                <a href="https://app.growreach.app/signup" class="btn btn-primary">Start Free Trial</a>
                <a href="https://growreach.app/pricing" class="btn btn-ghost">See Plans</a>
            </div>
            <p class="cta-trust">🔒 No credit card required &nbsp;·&nbsp; ✓ 7-day free trial &nbsp;·&nbsp; Cancel anytime</p>
        </div>
    ';

    return preg_replace($pattern, $replacement, $content);
}
add_filter('the_content', 'growreach_replace_cta_block');
```

---

### 2.8 OG Image (Auto-Generated)

```php
add_action('wp_head', function() {
    if ( is_single() && in_category(7) ) {
        $title = urlencode(get_the_title());
        echo '<meta property="og:type" content="article">';
        echo '<meta property="og:image" content="https://growreach.app/api/og?title=' . $title . '">';
        echo '<meta property="og:image:width" content="1200">';
        echo '<meta property="og:image:height" content="630">';
        echo '<meta property="twitter:card" content="summary_large_image">';
        echo '<meta property="twitter:image" content="https://growreach.app/api/og?title=' . $title . '">';
    }
});
```

**Implement the `/api/og` endpoint** using Vercel OG (if on Vercel) or Cloudinary text overlay. Template: white background, GrowReach logo top-left, page title center, brand color accent strip at bottom.

---

### 2.9 Table Mobile Handling

```php
function growreach_wrap_tables( $content ) {
    if ( ! is_single() || ! in_category(7) ) return $content;
    return str_replace(
        ['<table>', '</table>'],
        ['<div class="table-scroll"><table>', '</table></div>'],
        $content
    );
}
add_filter('the_content', 'growreach_wrap_tables');
```

```css
.table-scroll { overflow-x: auto; -webkit-overflow-scrolling: touch; margin: 24px 0; }
.table-scroll table { min-width: 600px; width: 100%; border-collapse: collapse; }
.table-scroll th, .table-scroll td { padding: 12px 16px; text-align: left; border-bottom: 1px solid #E5E7EB; }
.table-scroll thead tr { background: var(--brand-color); color: white; }
.table-scroll tbody tr:nth-child(even) { background: #F9FAFB; }
```

---

### 2.10 Internal Linking (Auto-Injected)

```php
function growreach_auto_interlink( $content ) {
    if ( ! is_single() || ! in_category(7) ) return $content;

    $links = [
        'creator targeting'      => '/feature/creator-targeting-with-comments-to-grow-on-linkedin',
        'keyword targeting'      => '/feature/auto-commenting-with-keyword-targeting-to-grow-on-linkedin',
        'engagement analytics'   => '/feature/analytics-and-insights-of-comments-to-grow-on-linkedin',
        'personal branding'      => '/use-case/growreach-for-growing-on-linkedin-and-personal-branding',
        'linkedin influencers'   => '/use-case/growreach-for-linkedin-influencers',
        'linkedin marketing'     => '/use-case/growreach-for-linkedin-marketing',
        'auto comment'           => '/feature/auto-commenting-to-grow-on-linkedin',
        'expandi alternative'    => '/comparison/expandi-vs-growreach-comparison-better-tool-to-grow-your-linkedin',
        'powerin alternative'    => '/comparison/powerin-vs-growreach-comparison-better-tool-to-grow-your-linkedin',
    ];

    $current_url = get_permalink();

    foreach ($links as $phrase => $url) {
        if ( strpos($current_url, $url) !== false ) continue;
        $linked = false;
        $content = preg_replace_callback(
            '/(?<!["\'=>])(\b' . preg_quote($phrase, '/') . '\b)(?!["\'])/i',
            function($m) use ($url, &$linked) {
                if ($linked) return $m[0];
                $linked = true;
                return '<a href="' . esc_url($url) . '">' . $m[0] . '</a>';
            },
            $content
        );
    }

    return $content;
}
add_filter('the_content', 'growreach_auto_interlink');
```

> **Maintain this list.** Add a new entry every time Claude publishes a new page. This builds an internal link graph that distributes PageRank across the site — a direct ranking signal for every page.

> **Orphan page rule:** Every newly published page must appear in this list (as a link target) AND be linked from at least one other published page within 24 hours. Pages with zero inbound internal links receive dramatically less crawl priority from Google. The auto-interlink filter handles this automatically as long as the list is maintained.

---

### 2.11 Social Sharing Buttons

```php
function growreach_social_share_buttons() {
    if ( ! is_single() || ! in_category(7) ) return;

    $url   = urlencode(get_permalink());
    $title = urlencode(get_the_title());

    echo '<div class="social-share">
        <span class="share-label">Share</span>
        <a href="https://www.linkedin.com/sharing/share-offsite/?url=' . $url . '" target="_blank" rel="noopener" class="share-btn share-linkedin" aria-label="Share on LinkedIn">LinkedIn</a>
        <a href="https://twitter.com/intent/tweet?url=' . $url . '&text=' . $title . '" target="_blank" rel="noopener" class="share-btn share-twitter" aria-label="Share on Twitter">Twitter</a>
        <button class="share-btn share-copy" onclick="navigator.clipboard.writeText(window.location.href)" aria-label="Copy link">Copy link</button>
    </div>';
}
add_action('growreach_after_content', 'growreach_social_share_buttons');
```

---

### 2.12 Core Web Vitals

| Metric | Target | Implementation |
|--------|--------|----------------|
| **LCP** | < 2.5s | Inline critical CSS. Defer non-critical JS. Preload primary font. No hero images above fold. H1 is typically the LCP element — ensure it renders immediately. |
| **CLS** | < 0.1 | `font-display: swap` with matched fallback font. Explicit `width`/`height` on all `<img>`. Never inject content above the fold after load. |
| **INP** | < 200ms | No heavy JS on content pages. Use `scheduler.yield()` for any long tasks. Debounce TOC scroll events. |
| **TTFB** | < 600ms | Full-page caching via WP Rocket or LiteSpeed Cache. |

**Do not lazy-load the H1.** It is often the LCP element — it must load immediately.

---

### 2.13 Canonical URLs

Yoast sets these automatically. Verify:
- Yoast → Search Appearance → General → Canonical enabled
- During GHL→WP migration: add a canonical tag in GHL pointing to the WordPress URL so Google consolidates ranking signals to the WP version

---

### 2.14 robots.txt Configuration

```
User-agent: *
Allow: /

User-agent: Googlebot
Allow: /

# Allow ChatGPT's retrieval agent (search visibility, not training)
User-agent: OAI-SearchBot
Allow: /

# Block OpenAI training scraper (optional — does not affect ChatGPT search)
User-agent: GPTBot
Disallow: /

# Disallow WordPress admin areas
User-agent: *
Disallow: /wp-admin/
Disallow: /wp-includes/

Sitemap: https://growreach.app/sitemap_index.xml
```

**Why OAI-SearchBot matters:** This is the bot that indexes content for ChatGPT's web search. Allowing it means GrowReach content can appear as citations in ChatGPT answers. GPTBot is for model training — separate from search visibility — block it if you don't want your content used to train future models.

---

### 2.15 XML Sitemap & Bing/ChatGPT Indexing

**Google:** Yoast auto-generates the sitemap at `/sitemap_index.xml`. Submit it in GSC: Sitemaps → Add sitemap URL.

**Bing + ChatGPT (IndexNow):** IndexNow pushes URLs to Bing and Yandex (Bing feeds ChatGPT's index) instantly on publish. Install **Rank Math** → IndexNow module. Or implement directly:

```php
function growreach_indexnow_submit( $new_status, $old_status, $post ) {
    if ( $new_status !== 'publish' || $old_status === 'publish' ) return;
    if ( ! in_array(7, wp_get_post_categories($post->ID)) ) return;

    $url = get_permalink($post->ID);
    $key = 'YOUR_INDEXNOW_API_KEY'; // Generate at bing.com/indexnow

    wp_remote_post('https://api.indexnow.org/indexnow', [
        'headers' => ['Content-Type' => 'application/json; charset=utf-8'],
        'body'    => json_encode([
            'host'    => 'growreach.app',
            'key'     => $key,
            'keyLocation' => 'https://growreach.app/' . $key . '.txt',
            'urlList' => [$url],
        ]),
    ]);
}
add_action('transition_post_status', 'growreach_indexnow_submit', 10, 3);
```

**Also submit to GSC on publish** (Rank Math Instant Indexing handles both — enable both Google and IndexNow in the plugin settings).

---

### 2.16 Title Tag & Meta Description Rules

Claude sets these via Yoast meta when pushing each post. The developer should verify Yoast is configured correctly:

| Field | Rule | Config |
|-------|------|--------|
| SEO title | 50–60 characters. Keyword near the front. | Yoast → set template `%%title%% %%page%% %%sep%% %%sitename%%` |
| Meta description | 150–160 characters. Includes target keyword. Natural sentence. | Claude sets this via `_yoast_wpseo_metadesc` post meta |
| Focus keyword | One per page. No two pages share the same focus keyword. | Claude sets via `_yoast_wpseo_focuskw` |

**Keyword cannibalization prevention:** Before Claude pushes a new post, the developer should confirm no existing published page already targets the same focus keyword. Two pages competing for the same keyword split ranking signals and both rank lower. If overlap is found, consolidate content into one stronger page.

---

### 2.17 Related Posts (Optional but Recommended)

At the bottom of each page, show 3 related posts from the same category. Improves internal linking depth and reduces bounce:

```php
function growreach_related_posts() {
    if ( ! is_single() || ! in_category(7) ) return;

    $related = get_posts([
        'category'       => 7,
        'posts_per_page' => 3,
        'post__not_in'   => [get_the_ID()],
        'orderby'        => 'rand',
    ]);

    if ( empty($related) ) return;

    echo '<div class="related-posts"><h3>Related articles</h3><ul>';
    foreach ($related as $r) {
        echo '<li><a href="' . get_permalink($r->ID) . '">' . esc_html($r->post_title) . '</a></li>';
    }
    echo '</ul></div>';
}
add_action('growreach_after_content', 'growreach_related_posts');
```

---

## Part 3: Generative Engine Optimization (GEO)

As of 2026, ~18% of commercial Google queries trigger AI Overviews. Content also gets cited by ChatGPT, Perplexity, and Gemini. The template should be structured for machine parsing, not just human reading.

**These are content authoring rules for Claude, not template rules — but the developer should understand them:**

1. **BLUF (Bottom Line Up Front):** The first paragraph of every page answers the core query directly. Never bury the answer in paragraph 4.
2. **Precise `<h2>` IDs:** The TOC anchor IDs (generated by the TOC filter) should be clean, descriptive slugs — e.g., `#how-linkedin-creator-engagement-works` not `#section-2`. These are what ChatGPT and Perplexity cite as sources.
3. **Definition lists for data:** When content includes specifications or comparisons with discrete fact/value pairs, use `<dl><dt>Term</dt><dd>Value</dd></dl>`. LLMs parse definition lists 30–40% more reliably than prose.
4. **Numerical density:** Paragraphs containing specific numbers (prices, percentages, timeframes) are cited significantly more often by AI Overviews. Keep all metrics in the content accurate.

---

## Part 4: Complete Workflow (End to End)

```
Claude:
  1. NeuronWriter keyword research (target keyword, NLP terms, word count)
  2. Writes HTML content (H1, H2s, H3 FAQs, tables, lists, CTA)
  3. Pushes to WP as DRAFT — sets slug, category 7, Yoast meta

Template auto-applies (no manual work):
  ✅ Author byline + date + reading time
  ✅ Breadcrumbs (Yoast → BreadcrumbList schema)
  ✅ Sticky sidebar TOC (desktop) + collapsible inline TOC (mobile)
  ✅ H2 anchor IDs injected
  ✅ Table scroll wrappers (mobile-safe)
  ✅ CTA block converted to styled component
  ✅ Internal links auto-injected (first occurrence per phrase)
  ✅ Author bio card appended
  ✅ Social share buttons appended
  ✅ Related posts appended
  ✅ Article JSON-LD (with real author Person)
  ✅ FAQ JSON-LD (from H3+P pairs — gives SERP FAQ rich results)
  ✅ Organization JSON-LD (SameAs links — E-E-A-T signal)
  ✅ OG image + Twitter Card meta tags
  ✅ Canonical URL (Yoast)

Designer QA:
  - Visual review only (everything is already applied)
  - Check mobile layout, TOC behavior, CTA styling

Publish:
  ✅ Rank Math submits to Google Indexing API (instant GSC crawl)
  ✅ IndexNow submits to Bing/ChatGPT (instant Bing index)
  ✅ Sitemap auto-updated (Yoast)
```

---

## Part 5: HTML Elements Reference

| Element | SEO Purpose | Never Do |
|---------|-------------|----------|
| `<h1>` | Primary keyword signal | Add second H1. Override with `the_title()`. |
| `<h2>` | Section keywords + TOC anchors + SERP sitelinks | Skip to H3. Style as decorative spans. |
| `<h3>` | FAQ questions, sub-sections | Accordion the answers — keep fully visible. |
| `<hr />` | Section boundary signal | Hide or remove. |
| `<strong>` | NLP term emphasis | Replace with `<span>`. Change color. |
| `<table>` | Structured comparison data | Convert to div grid — breaks Google indexing of table data. |
| `<ul>/<ol>` | List content eligible for featured snippets | Remove structure to "clean up" design. |
| `<p>` | Content paragraphs | Merge into blocks. Paragraph count signals readability. |
| Final `<p><strong>Start...` | CTA marker — dev auto-converts | Delete it — it's the conversion hook. |

---

## Part 6: SEO Health Checks

Run these on launch and monthly thereafter.

| Check | Tool | Pass Condition |
|-------|------|----------------|
| All category 7 posts indexed | Google Search Console → Pages | Status: Indexed, not "Crawled - not indexed" |
| Core Web Vitals pass | GSC → Core Web Vitals | All URLs: Good (green) |
| No manual actions | GSC → Manual Actions | "No issues detected" |
| No duplicate content | Yoast or Semrush Site Audit | No pages sharing focus keywords |
| No orphan pages | Semrush Site Audit → "Orphaned pages" | Zero orphan pages in category 7 |
| Broken internal links | GSC → Links or Semrush | Zero broken internal links |
| Author archives indexable | GSC → inspect `/author/rahul` | Indexed, no noindex |
| Sitemap submitted | GSC → Sitemaps | Sitemap processed, URLs discovered |
| robots.txt correct | `growreach.app/robots.txt` | OAI-SearchBot allowed, admin blocked |
| Schema valid | Google Rich Results Test | Article + FAQ valid, no errors |
| No redirect chains | Semrush Site Audit | All redirects are single-hop 301s |
| HTTPS only | Site Audit or browser DevTools | No mixed content (HTTP resources on HTTPS page) |

---

## Part 7: What NOT to Do

1. **Don't accordion FAQ sections** — Google gives full credit to visible answers, partial to hidden
2. **Don't convert `<h2>` to styled divs** — heading hierarchy is semantic, not decorative
3. **Don't remove `<hr>`** — it's a section boundary signal, not just a visual rule
4. **Don't use `display:none` to hide content on mobile** — Google indexes both mobile and desktop and penalizes hidden content
5. **Don't convert comparison tables to CSS grids** — `<table>` structure is how Google reads tabular data
6. **Don't lazy-load the H1** — it's often the LCP element; must render first
7. **Don't change post slugs after publishing** — breaks URL, loses all accumulated ranking signals
8. **Don't add `noindex` to author archive pages** — these are E-E-A-T credibility signals; they must be indexable
9. **Don't set two pages to the same Yoast focus keyword** — they'll compete against each other and both rank lower
10. **Don't skip adding new pages to the auto-interlink list** — orphan pages get minimal crawl budget

---

*Content pipeline: see `content/` in the `seo-strategy` branch. Each HTML file maps to a WordPress draft post (Post IDs 819–829 as of March 2026).*
