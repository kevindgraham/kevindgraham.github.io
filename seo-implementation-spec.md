# SEO Implementation Spec — kevindgraham.com

**How to use this:** Drop this file in the root of the `kevindgraham.github.io` repo, then tell Claude Code:

> Read `seo-implementation-spec.md` and work through it phase by phase. Start with Phase 0 and report what you find before making changes. Show me `git status` and `git diff` before any commit, and don't push until I say go.

---

## Phase 0 — Audit before you change anything

Report back on:

1. Is this a plain static HTML site or Jekyll? (Check for `_config.yml`, `_layouts/`, `_includes/`.)
2. List every page/route in the site and its current `<title>` and `<meta name="description">`.
3. Does `sitemap.xml` exist? `robots.txt`? A `CNAME` file?
4. Are there duplicate or missing `<h1>` tags on any page?
5. How many images lack `alt` attributes?
6. Is any structured data (JSON-LD) already present?

**Do not make changes until this audit is reported.**

---

## Phase 1 — Technical foundation

### robots.txt
Create at site root:
```
User-agent: *
Allow: /

Sitemap: https://kevindgraham.com/sitemap.xml
```

### sitemap.xml
- If Jekyll: add the `jekyll-sitemap` plugin to `_config.yml` and let it generate.
- If plain HTML: generate `sitemap.xml` manually listing every real page with `<loc>` and `<lastmod>`. Exclude 404 pages, thank-you pages, and any test files.

### Canonical URLs
Add a self-referencing canonical to every page:
```html
<link rel="canonical" href="https://kevindgraham.com/PAGE-PATH/">
```
Pick ONE canonical form (with or without `www`, with or without trailing slash) and make every internal link match it.

### 404 page
Create a branded `404.html` in the navy/gold palette that links back to the homepage, blog, and toolkit — not a dead end.

---

## Phase 2 — Per-page metadata

For every page, write **unique** values. No duplicates across the site.

| Element | Rule |
|---|---|
| `<title>` | 50–60 characters. Front-load the search term, end with `\| Kevin D. Graham` |
| `<meta name="description">` | 140–160 characters. Written as a hook, not a summary |
| `<h1>` | Exactly one per page, and it should not be identical to the title tag |
| `<html lang="en">` | Present on every page |
| Heading order | H1 → H2 → H3, no skipped levels |

### Open Graph + Twitter cards
Add to every page so links look right when shared:
```html
<meta property="og:title" content="...">
<meta property="og:description" content="...">
<meta property="og:image" content="https://kevindgraham.com/images/og-default.jpg">
<meta property="og:url" content="https://kevindgraham.com/PAGE-PATH/">
<meta property="og:type" content="website">
<meta name="twitter:card" content="summary_large_image">
```
Flag if no OG image exists at 1200×630 — that one needs to be designed, not coded.

### Images
- Add descriptive `alt` text to every image (describe the content, not "image of").
- Add `width` and `height` attributes to prevent layout shift.
- Add `loading="lazy"` to any image below the fold.
- Flag any image over 200KB and convert to WebP where possible.

---

## Phase 3 — Structured data (JSON-LD)

### Homepage — Person schema
```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Kevin D. Graham",
  "url": "https://kevindgraham.com",
  "jobTitle": "Digital Media Educator and Content Creator",
  "description": "...",
  "sameAs": ["https://instagram.com/strategicdownloads"]
}
```

### Book page — Book schema
Include `name`, `author`, `description`, `bookFormat`, and the purchase URL.

### Blog posts — Article schema
Include `headline`, `datePublished`, `dateModified`, `author`, and `image`.

### Any FAQ-style content — FAQPage schema
Only where real Q&A content exists on the page. Don't fabricate questions to game it.

**Validate every block** against schema.org's syntax before committing.

---

## Phase 4 — Internal linking + crawlability

- Every blog post should link to at least one pillar page and one other post.
- Every pillar page should link to at least two related posts.
- Use descriptive anchor text — "how to write better captions," not "click here."
- Confirm no page is orphaned (reachable only by typing the URL).
- Confirm nav links appear in the HTML source, not injected by JavaScript.

---

## Phase 5 — Performance + accessibility

- Inline critical CSS or minimize render-blocking resources.
- Preload the primary web font; add `font-display: swap`.
- Confirm color contrast meets WCAG AA against the navy/gold/cream palette.
- Confirm the site works with JavaScript disabled (static site — it should).
- Run a Lighthouse audit and report scores for Performance, Accessibility, Best Practices, and SEO. Target 90+ on all four.

---

## Phase 6 — Blog scaffolding

If a blog post template exists, make sure every new post automatically gets:
- Unique title, description, and canonical
- Article JSON-LD
- Published + modified dates in the HTML
- OG tags pulled from the post's own front matter

If it doesn't exist, create the template so future posts inherit all of this without manual work.

---

## Verification checklist

Before pushing, confirm:

- [ ] Every page has a unique title and description
- [ ] `sitemap.xml` returns valid XML and lists every live page
- [ ] `robots.txt` points to the sitemap and blocks nothing important
- [ ] Every page has exactly one H1
- [ ] Every image has alt text
- [ ] JSON-LD validates on the homepage, book page, and one blog post
- [ ] No broken internal links
- [ ] Lighthouse SEO score is 95+

Then show `git status` and `git diff`, and wait for approval before pushing to main.

---

## What Claude Code CANNOT do — Kevin's list

These require a human and an account:

1. **Google Search Console** — verify the domain, submit the sitemap, request indexing on key pages.
2. **Bing Webmaster Tools** — same, five-minute setup.
3. **Google Business Profile** — for anything local (photography, local visibility work).
4. **Keyword decisions** — what you actually want to rank for. Code can't pick this.
5. **Writing the content** — the blog posts are the engine. Nothing above matters without them.
6. **Backlinks** — podcast appearances, guest posts, directory listings, book retailer pages.
