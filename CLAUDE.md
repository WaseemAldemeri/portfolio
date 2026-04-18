# CLAUDE.md — Portfolio Development Guidelines

This is a Hugo portfolio + blog site for **Waseem Al-Dmeiri** using the **Blowfish v2** theme.
Base URL: `https://dmeiri.dev/` | Installed via Hugo Modules (`github.com/nunocoracao/blowfish/v2`).

---

## PRIME DIRECTIVE: Blowfish-First Development

**Always use Blowfish's built-in features before writing any custom HTML/CSS/JS.**

Decision order:
1. **Blowfish config param** — can it be done via `params.toml` or front matter?
2. **Blowfish shortcode** — is there a shortcode that does this?
3. **Blowfish partial override** — can an existing partial be extended/overridden?
4. **`assets/css/custom.css`** — CSS-only tweak that doesn't need markup changes?
5. **Custom HTML/CSS** — only when none of the above can achieve the goal, OR the user explicitly asks for something custom.

Never write custom solutions for things Blowfish already provides. Check this file before reaching for raw HTML.

---

## Project Structure

```
config/_default/
  hugo.toml          # baseURL, title, module imports
  params.toml        # Theme parameters (main config)
  languages.en.toml  # Author info, language settings
  menus.en.toml      # Navigation menus (main + footer)
  markup.toml        # Markdown/syntax highlighting

content/
  _index.md          # Homepage content
  about/index.md     # About page
  blog/              # Blog posts (leaf bundles: index.md + assets)
  projects/          # Project pages (leaf bundles)

layouts/             # Local overrides (higher priority than theme)
  partials/
    footer.html           # Custom footer
    home/background.html  # Custom homepage (background layout, layout = "background")

assets/css/custom.css  # Custom CSS (create if needed)
static/                # Static files (fonts, favicon, etc.)
```

---

## Configuration Reference

### Active Settings (params.toml)

```toml
colorScheme = "blowfish"
defaultAppearance = "dark"
autoSwitchAppearance = true
enableSearch = true
enableCodeCopy = true
mainSections = ["projects", "blog"]

[homepage]
  layout = "profile"
  showRecent = true
  showRecentItems = 5
  showMoreLink = true
  showMoreLinkDest = "/blog"
  cardView = true

[article]
  showDate = true
  showAuthor = true
  showBreadcrumbs = true
  showTableOfContents = true
  showTaxonomies = true

[list]
  showBreadcrumbs = true
  groupByYear = true
  cardView = true
  showSummary = true
```

### All Available params.toml Options

**Appearance:**
- `colorScheme` — one of 16 built-in schemes (see Color Schemes section)
- `defaultAppearance` — `"light"` or `"dark"`
- `autoSwitchAppearance` — respects OS dark/light preference
- `enableA11y` — accessibility enhancements

**Features:**
- `enableSearch` — Fuse.js client-side search
- `enableCodeCopy` — copy button on code blocks
- `disableImageZoom` — disable medium-zoom on images
- `disableImageOptimization` — skip Hugo image processing
- `hotlinkFeatureImage` — load feature images from external CDN
- `replyByEmail` — email reply functionality
- `smartTOC` — intelligent TOC generation
- `enableStructuredBreadcrumbs` — Schema.org breadcrumb markup

**Header:**
- `header.layout` — `"basic"`, `"fixed"`, `"fixed-fill"`, `"fixed-fill-blur"`
- `header.showMenu` — show main nav
- `header.showAppearanceSwitcher` — dark/light toggle
- `header.showLanguageSwitch` — language selector

**Footer:**
- `footer.showMenu` — show footer nav
- `footer.showCopyright` — copyright text
- `footer.showThemeAttribution` — theme credit
- `footer.showAppearanceSwitcher` — appearance toggle
- `footer.showScrollToTop` — scroll-to-top button

**Homepage layouts** (`homepage.layout`):
- `"profile"` — author photo, bio, social links, recent content ← currently active
- `"page"` — standard static page
- `"hero"` — author info + hero background image
- `"background"` — full background image variant
- `"card"` — card-based featured image layout
- `"custom"` — use `layouts/partials/home/custom.html`

**Article display** (global defaults, overridable per-page in front matter):
- `article.showDate` / `showDateUpdated`
- `article.showAuthor`
- `article.showHero` — feature image in article
- `article.heroStyle` — `"basic"`, `"big"`, `"background"`, `"thumbAndBackground"`
- `article.showBreadcrumbs`
- `article.showTableOfContents`
- `article.showReadingTime` / `showWordCount`
- `article.showTaxonomies`
- `article.showComments`
- `article.sharingLinks` — array of social platforms
- `article.seriesOpened`

**List pages:**
- `list.showBreadcrumbs`
- `list.showTableOfContents`
- `list.groupByYear`
- `list.cardView`
- `list.showSummary`

**Author (languages.en.toml):**
- `author.name`, `author.headline`, `author.bio`, `author.image`
- `author.links` — array of `{name, url}` (see Icons section for valid names)

**Analytics (add to params.toml as needed):**
- Google Analytics: `[services.google.analytics] id = "G-XXXXXXXXXX"`
- Fathom: `[services.fathom] siteCode = "XXXXXXXX"`
- Umami: `[services.umami] websiteID = "..."`, `domain`, `scriptURL`
- BuyMeACoffee: `[services.buymeacoffee]`
- Firebase (views/likes): `[services.firebase]`
- Google AdSense: `[services.googleAdSense] publisherID = "..."`

---

## Front Matter Reference

Every content file can override global settings. These are all available parameters:

```yaml
---
title: "Post Title"
description: "SEO description and summary"
summary: "Custom summary (supports markdown)"
date: 2024-01-15
lastmod: 2024-01-20
draft: false

# Layout
layout: "simple"          # override template ("simple" strips sidebar/meta)
showHero: true
heroStyle: "big"          # basic | big | background | thumbAndBackground
featureimage: "cover.jpg" # or place feature*.jpg in bundle dir
imagePosition: "center"
layoutBackgroundBlur: true

# Metadata display (all override global params)
showDate: true
showDateUpdated: false
showAuthor: true
showBreadcrumbs: true
showTableOfContents: true
showTaxonomies: true
showWordCount: false
showReadingTime: true
showComments: false
showEdit: false

# Categorization
tags: ["go", "backend"]
categories: ["tutorials"]
series: ["My Series"]
series_order: 1
seriesOpened: true
authors: ["waseem"]         # multi-author

# SEO
robots: "noindex, nofollow"
keywords: ["keyword1"]
canonical: "https://..."
socialImage: "social.jpg"

# External article (shows as outbound link in listings)
externalUrl: "https://..."
---
```

---

## Shortcodes Reference

**Use these before writing custom HTML.** All 33 built-in shortcodes:

### Callouts & Notices

```markdown
{{< alert icon="fire" iconColor="#FF4500" cardColor="#1a1a1a" textColor="#fff" >}}
  Your message here.
{{< /alert >}}

{{< admonition type="NOTE" >}}         <!-- NOTE | TIP | IMPORTANT | WARNING | CAUTION -->
  GitHub-style callout box.
{{< /admonition >}}
```

### Text & Typography

```markdown
{{< lead >}}
  Emphasized introductory paragraph text.
{{< /lead >}}

{{< badge >}}New{{< /badge >}}

{{< button href="/contact" target="_blank" >}}Contact Me{{< /button >}}

{{< keyword >}}Go{{< /keyword >}}
{{< keywordList >}}
  {{< keyword icon="github" >}}Open Source{{< /keyword >}}
  {{< keyword >}}Backend{{< /keyword >}}
{{< /keywordList >}}

{{< typeit tag="h2" speed=50 lifeLike=true loop=false waitUntilVisible=true >}}
  Typewriter animation text.
{{< /typeit >}}

{{< icon "github" >}}   <!-- inline SVG icon -->

{{< swatches "#FF0000" "#00FF00" "#0000FF" >}}  <!-- color palette display -->
```

### Layout & Structure

```markdown
{{< accordion mode="auto" separated=false >}}
  {{< accordionItem header="Title" >}}
    Content here.
  {{< /accordionItem >}}
{{< /accordion >}}

{{< tabs groupId="os" >}}
  {{< tab label="Linux" icon="linux" >}}Linux content{{< /tab >}}
  {{< tab label="macOS" >}}macOS content{{< /tab >}}
{{< /tabs >}}

{{< timeline >}}
  {{< timelineItem icon="code" header="Job Title" badge="2020–now" subheader="Company" >}}
    Description of role.
  {{< /timelineItem >}}
{{< /timeline >}}

{{< list limit=5 title="Recent Posts" cardView=true where="Section" value="blog" >}}
```

### Media

```markdown
{{< figure src="image.jpg" alt="Alt text" caption="Caption" href="/link" nozoom=true >}}

{{< gallery >}}
  <img src="img1.jpg" class="grid-w33" />  <!-- grid-w33=33%, grid-w50=50%, default=100% -->
  <img src="img2.jpg" class="grid-w33" />
  <img src="img3.jpg" class="grid-w33" />
{{< /gallery >}}

{{< carousel images="img/*.jpg" captions="Caption 1,Caption 2" aspectRatio="16-9" interval=2500 >}}

{{< video src="video.mp4" poster="thumb.jpg" autoplay=false loop=false controls=true ratio="16x9" >}}

{{< youtube-lite id="dQw4w9WgXcQ" label="Video title" >}}
```

### Code & Technical

```markdown
{{< code-importer url="https://raw.githubusercontent.com/..." type="go" startLine=10 endLine=50 >}}

{{< markdown-importer url="https://raw.githubusercontent.com/..." >}}

{{< gist username gist-id "optional-filename.go" >}}

{{< mermaid >}}
graph TD
    A --> B
{{< /mermaid >}}

{{< katex >}}
\( \int_0^\infty e^{-x} dx = 1 \)
{{< /katex >}}
```

### Repository Cards

```markdown
{{< github-card repo="owner/repo" >}}
{{< gitlab-card projectID="12345" >}}
{{< gitea-card repo="owner/repo" server="https://gitea.example.com" >}}
{{< codeberg-card repo="owner/repo" >}}
{{< huggingface-card model="owner/model" >}}
```

### Content Embedding

```markdown
{{< article link="/blog/my-post" showSummary=true compactSummary=false >}}

{{< ltr >}}Left-to-right forced text{{< /ltr >}}
{{< rtl >}}Right-to-left forced text{{< /rtl >}}
```

---

## Partial Overrides

Override by creating matching file in `layouts/partials/`. Currently overridden:
- `layouts/partials/footer.html` ← custom footer
- `layouts/partials/home/background.html` ← custom homepage (active layout)

**Available override targets:**

| Partial | Purpose |
|---|---|
| `extend-head.html` | Inject into `<head>` (scripts, meta, analytics) |
| `extend-head-uncached.html` | Same but bypasses cache |
| `extend-footer.html` | Inject before `</body>` |
| `extend-article-link.html` | Extend article card display |
| `comments.html` | Comments system (Disqus, etc.) |
| `favicons.html` | Favicon configuration |
| `head.html` | Full head override |
| `header.html` | Full header override |
| `footer.html` | Full footer override |
| `breadcrumbs.html` | Breadcrumb nav |
| `toc.html` | Table of contents |
| `hero.html` | Hero image section |
| `author.html` / `author-extra.html` | Author display |
| `pagination.html` | Page pagination |
| `sharing-links.html` | Social sharing buttons |
| `search.html` | Search interface |
| `schema.html` | Structured data markup |
| `home/profile.html` | Profile homepage layout |
| `home/hero.html` | Hero homepage layout |
| `home/background.html` | Background homepage layout |
| `home/card.html` | Card homepage layout |
| `home/custom.html` | Custom homepage (create new) |
| `article-link/card.html` | Article card component |
| `article-link/simple.html` | Simple article link |

**Minimal extension (preferred over full override):**
```html
<!-- layouts/partials/extend-head.html -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<script src="/js/custom.js"></script>
```

---

## Color Schemes

16 built-in schemes. Change via `colorScheme` in `params.toml`:

`autumn`, `avocado`, `bloody`, `blowfish` *(active)*, `congo`, `fire`, `forest`, `github`, `marvel`, `neon`, `noir`, `ocean`, `one-light`, `princess`, `slate`, `terminal`

**Custom scheme:** Create `assets/css/schemes/<name>.css` with CSS variables for `neutral`, `primary`, `secondary` (10 shades each using Tailwind color naming: 50–900).

---

## Icons

Blowfish ships SVG icons. Use by name in menus (`pre`/`icon` field) and the `{{< icon >}}` shortcode.

**Available icons include:** `github`, `linkedin`, `twitter`, `email`, `phone`, `whatsapp`, `instagram`, `youtube`, `mastodon`, `rss`, `globe`, `link`, `code`, `terminal`, `fire`, `star`, `heart`, `check`, `x`, `warning`, `info`, `lightbulb`, `pencil`, `book`, `graduation-cap`, `briefcase`, `flask`, `cpu`, `server`, `database`, `docker`, `kubernetes`, `go`, `python`, `javascript`, `typescript`, `react`, and many more.

Custom icons: place SVG at `assets/icons/<name>.svg`.

---

## Content Organization

### Page Bundles (always use leaf bundles for posts)

```
content/blog/my-post/
  index.md          # post content
  feature.jpg       # auto-detected as hero/thumbnail image
  background.jpg    # auto-detected as background image
  img/              # images referenced in post
```

### Taxonomies

Default: `tags`, `categories`, `series`. Custom taxonomies via `hugo.toml`:
```toml
[taxonomies]
  tag = "tags"
  category = "categories"
  series = "series"
  skill = "skills"   # example custom taxonomy
```

### Series

```yaml
# In each post's front matter:
series: ["Getting Started with Go"]
series_order: 1
```

### Multiple Authors

Define in `languages.en.toml`:
```toml
[params.authors.waseem]
  name = "Waseem Al-Dmeiri"
  image = "img/author.jpg"
```

Reference in front matter: `authors: ["waseem"]`

---

## Navigation Menus (menus.en.toml)

```toml
[[main]]
  name = "Projects"
  pageRef = "projects"
  weight = 10

[[main]]
  name = "External Link"
  url = "https://example.com"
  weight = 20
  pre = "github"    # icon name shown before label

[[footer]]
  name = "GitHub"
  url = "https://github.com/username"
  weight = 10
  pre = "github"
```

---

## CSS Customization

**Only use `assets/css/custom.css` when Blowfish config/shortcodes cannot achieve the goal.**

Blowfish uses Tailwind CSS. CSS custom properties are available:
- `--color-primary-*` (50–900 shades)
- `--color-secondary-*`
- `--color-neutral-*`
- `font-size` root override for global scale

```css
/* assets/css/custom.css */
/* Minimal targeted overrides only */
```

---

## Hugo Outputs & SEO

The site generates:
- HTML pages
- RSS feeds (`/index.xml`)
- JSON search index (`/index.json`) — required for `enableSearch`
- Sitemap (`/sitemap.xml`)
- `robots.txt`

Ensure `config/_default/hugo.toml` includes:
```toml
[outputs]
  home = ["HTML", "RSS", "JSON"]
```

---

## Build Commands

```bash
hugo server -D          # development (includes drafts)
hugo                    # production build → public/
hugo --gc --minify      # production with cleanup and minification
hugo mod get -u         # update Blowfish to latest version
```

---

## Do's and Don'ts

**Do:**
- Check this file before writing any custom code
- Use front matter overrides before changing global params
- Use leaf bundles (`content/section/post/index.md`) for all content with assets
- Place feature images as `feature.jpg` inside the bundle directory
- Use `extend-head.html` / `extend-footer.html` for small injections
- Use `assets/css/custom.css` for CSS-only tweaks

**Don't:**
- Write raw HTML for things shortcodes can handle (callouts, galleries, cards, buttons, etc.)
- Create new layout templates when a partial override suffices
- Add custom JS for features Blowfish already provides (search, zoom, copy, dark mode)
- Override full partials when `extend-*` variants exist
- Add inline styles — use `custom.css` or Tailwind classes
- Modify files inside `themes/` — always override at project level
