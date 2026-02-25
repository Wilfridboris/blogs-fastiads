# Story 1.1: Jekyll Blog — GitHub Pages Setup with SEO & Clean URLs

Status: ready-for-dev

## Story

As a **content creator for fastiads**,
I want **a standalone Jekyll blog repository on GitHub Pages with jekyll-seo-tag and clean URLs**,
so that **blog posts written as Markdown files are automatically published to blogs.fastiads with proper SEO and no `.html` extensions**.

---

## Acceptance Criteria

1. **Jekyll repository exists** — A new GitHub repository named `blogs-fastiads` is created and initialized with a working Jekyll 4.4+ project structure.

2. **GitHub Pages deploys via GitHub Actions** — Every push to `main` triggers the Actions workflow and successfully deploys the `_site/` output to GitHub Pages.

3. **Custom domain served** — A `CNAME` file containing `blogs.fastiads` is present in the repository root, and the GitHub Pages settings have the custom domain configured with HTTPS enforced.

4. **Clean URLs** — All blog posts are accessible without `.html` extensions (e.g., `/2026/02/20/hello-world/` not `/2026/02/20/hello-world.html`). `permalink: pretty` is set in `_config.yml`.

5. **jekyll-seo-tag integrated** — The `jekyll-seo-tag` gem is listed in `Gemfile` and `_config.yml`, and the `{% seo %}` tag is present in the default layout `<head>`. Each post renders correct `<title>`, meta description, OG tags, and Twitter Card tags.

6. **At least one sample post** — A sample post `_posts/YYYY-MM-DD-hello-world.md` exists with complete front matter (layout, title, date, categories, tags, description) demonstrating correct usage.

7. **Local dev server works** — Running `bundle exec jekyll serve --livereload` builds and serves the site locally without errors on Ruby 3.3.

8. **Gemfile.lock committed** — `Gemfile.lock` is committed so GitHub Actions builds reproducibly without version drift.

9. **Blog → Main site linking** — Every page on `blogs.fastiads` has a header link and footer link back to `fastiads.com`. Every post layout includes a "Back to fastiads.com" or branded nav item. Google must be able to crawl from the blog to the main site on every page.

10. **Schema.org Organization JSON-LD** — `_layouts/default.html` includes a `<script type="application/ld+json">` block declaring `@type: Organization`, `name: fastiads`, `url: https://fastiads.com`, `sameAs: [https://blogs.fastiads.com]`. This tells Google both domains belong to the same entity.

11. **Main site → Blog link (coordination task)** — The main `fastiads.com` site adds a "Blog" link in its footer or navigation pointing to `https://blogs.fastiads.com`. *(This is a muself project task — documented here for coordination, implemented in muself.)*

---

## Tasks / Subtasks

- [ ] **Task 1 — Initialize repository structure** (AC: #1)
  - [ ] 1.1 Create `Gemfile` with jekyll ~> 4.4, minima ~> 2.5, jekyll-seo-tag, jekyll-sitemap, jekyll-feed, webrick
  - [ ] 1.2 Run `bundle install` and commit `Gemfile.lock`
  - [ ] 1.3 Create `_config.yml` with site metadata, `permalink: pretty`, and plugin list
  - [ ] 1.4 Create standard Jekyll folder scaffold: `_posts/`, `_drafts/`, `_layouts/`, `_includes/`, `assets/`
  - [ ] 1.5 Create `index.md` (or `index.html`) as blog index page
  - [ ] 1.6 Create `.gitignore` excluding `_site/`, `.sass-cache/`, `.jekyll-cache/`, `vendor/`

- [ ] **Task 2 — Configure GitHub Actions deployment** (AC: #2)
  - [ ] 2.1 Create `.github/workflows/jekyll.yml` using `ruby/setup-ruby@v1`, `actions/configure-pages@v4`, `actions/upload-pages-artifact@v3`, `actions/deploy-pages@v4`
  - [ ] 2.2 Set Ruby version to `3.3` and enable `bundler-cache: true`
  - [ ] 2.3 Set `JEKYLL_ENV: production` during build step
  - [ ] 2.4 Verify build/deploy jobs have correct permissions (`contents: read`, `pages: write`, `id-token: write`)
  - [ ] 2.5 In GitHub repo Settings → Pages, set Source to **"GitHub Actions"** (not branch)
  - [ ] 2.6 Push to `main` and confirm workflow passes with green checkmark

- [ ] **Task 3 — Configure custom domain** (AC: #3)
  - [ ] 3.1 Create `CNAME` file in repo root containing `blogs.fastiads` (single line, no protocol)
  - [ ] 3.2 In GitHub repo Settings → Pages, enter `blogs.fastiads` as custom domain
  - [ ] 3.3 Enable "Enforce HTTPS" checkbox after DNS propagation
  - [ ] 3.4 Configure DNS at domain registrar/CDN:
    - Subdomain CNAME: `blogs.fastiads` → `<github-username>.github.io`
    - OR A records pointing to GitHub's IPs: `185.199.108.153`, `.109.153`, `.110.153`, `.111.153`
  - [ ] 3.5 Verify HTTPS certificate issued (usually within 1 hour of DNS propagation)

- [ ] **Task 4 — Implement clean URLs** (AC: #4)
  - [ ] 4.1 Set `permalink: pretty` in `_config.yml` (generates `/YYYY/MM/DD/title/` URLs)
  - [ ] 4.2 Verify locally that post URLs have trailing slash and no `.html`
  - [ ] 4.3 Confirm Jekyll generates `index.html` inside each permalink directory

- [ ] **Task 5 — Integrate jekyll-seo-tag** (AC: #5)
  - [ ] 5.1 Add `gem "jekyll-seo-tag", "~> 2.8"` to `Gemfile` and `jekyll_plugins` group
  - [ ] 5.2 Add `jekyll-seo-tag` to `plugins:` list in `_config.yml`
  - [ ] 5.3 Add site-level SEO fields to `_config.yml`: `title`, `description`, `url`, `lang`, `social`
  - [ ] 5.4 Add `{% seo %}` tag inside `<head>` in `_layouts/default.html` (before `</head>`)
  - [ ] 5.5 Verify rendered HTML contains `<title>`, `<meta name="description">`, OG `og:title`, `og:url`, `og:description`, Twitter Card `twitter:card`

- [ ] **Task 6 — Create sample post** (AC: #6)
  - [ ] 6.1 Create `_posts/2026-02-22-hello-world.md` with full front matter
  - [ ] 6.2 Front matter must include: `layout`, `title`, `date`, `categories`, `tags`, `description`
  - [ ] 6.3 Verify post URL is `/2026/02/22/hello-world/` (clean, no `.html`)
  - [ ] 6.4 Verify post page has correct SEO meta tags from jekyll-seo-tag

- [ ] **Task 7 — Validate local dev** (AC: #7, #8)
  - [ ] 7.1 Run `bundle exec jekyll serve --livereload` and confirm no build errors
  - [ ] 7.2 Visit `http://localhost:4000` and confirm blog renders
  - [ ] 7.3 Check `Gemfile.lock` is committed to repo

- [ ] **Task 8 — Cross-link blog ↔ main site (sibling SEO)** (AC: #9, #10)
  - [ ] 8.1 Add `main_site_url: "https://fastiads.com"` and `main_site_name: "fastiads"` to `_config.yml` so all templates reference a single variable
  - [ ] 8.2 In `_includes/header.html` — add branded nav link: `<a href="{{ site.main_site_url }}">← fastiads.com</a>` visible on every page
  - [ ] 8.3 In `_includes/footer.html` — add footer link: `Part of <a href="{{ site.main_site_url }}" rel="publisher">fastiads.com</a>`
  - [ ] 8.4 In `_layouts/default.html` — embed Organization JSON-LD in `<head>`:
    ```html
    <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "Organization",
      "name": "fastiads",
      "url": "{{ site.main_site_url }}",
      "sameAs": [
        "{{ site.url }}"
      ]
    }
    </script>
    ```
  - [ ] 8.5 In post layout `_layouts/post.html` — add `rel="publisher"` link in `<head>`: `<link rel="publisher" href="{{ site.main_site_url }}">`
  - [ ] 8.6 Verify crawl path: open blog post → header link resolves to `fastiads.com` → footer link resolves to `fastiads.com`
  - [ ] 8.7 Validate JSON-LD with Google's Rich Results Test on a deployed post URL

- [ ] **Task 9 — Main site back-link (muself coordination)** (AC: #11)
  - [ ] 9.1 *(In muself repo)* Add "Blog" link in `fastiads.com` site footer or main navigation pointing to `https://blogs.fastiads.com`
  - [ ] 9.2 *(In muself repo)* Ensure the link is a plain `<a href>` (not `rel="nofollow"`, not JS-rendered) so Google crawls it
  - [ ] 9.3 *(Optional but strong signal)* Add `Blog` entry to the main site's `Organization` or `WebSite` schema as `subOrganization` or `sameAs`
  - [ ] 9.4 After both deploys — run Google Search Console "URL Inspection" on both domains and request indexing

---

## Dev Notes

### Technology Stack
- **Jekyll** ~> 4.4 (NOT `github-pages` gem — direct jekyll is the 2026 standard for Actions deployments)
- **Ruby** 3.3 LTS (current stable)
- **Theme:** minima ~> 2.5 (default; swap for custom theme if desired later)
- **Key plugins:** `jekyll-seo-tag` ~> 2.8, `jekyll-sitemap` ~> 1.4, `jekyll-feed` ~> 0.17
- **`webrick`** ~> 1.8 must be in Gemfile for local dev on modern Ruby (stdlib webrick removed in Ruby 3.0+)
- **Deployment:** GitHub Actions (NOT classic branch-based Pages)

### Why NOT `github-pages` gem?
The `github-pages` gem pins old dependency versions and is increasingly outdated. The modern approach (2025+) is to use the direct `jekyll` gem and deploy via GitHub Actions, giving full control over Ruby version and gem versions.

### Permalink / Clean URLs
`permalink: pretty` in `_config.yml` is the canonical way to achieve clean URLs. This produces:
- Input: `_posts/2026-02-22-hello-world.md`
- Output URL: `/2026/02/22/hello-world/` (served from `_site/2026/02/22/hello-world/index.html`)

No web server rewrite rules needed — Jekyll generates a directory with `index.html` inside.

### jekyll-seo-tag Usage
The plugin needs `{% seo %}` tag in the `<head>` section of the base layout. It reads from:
- Site-level: `_config.yml` fields (`title`, `description`, `url`, `lang`, `social`)
- Post-level: front matter (`title`, `description`, `image`, `author`, `date`)

Canonical URL generation is automatic based on `url` + `permalink`.

### GitHub Actions Workflow — Critical Details
- **Permissions** block is REQUIRED: `pages: write` and `id-token: write` must be explicit
- **Source** in GitHub Settings → Pages must be set to **"GitHub Actions"** not a branch
- The `configure-pages@v4` action dynamically sets `base_path` — pass it to jekyll build: `--baseurl "${{ steps.pages.outputs.base_path }}"`
- Use `concurrency.cancel-in-progress: false` to prevent mid-deploy cancellations

### Custom Domain DNS
For subdomain `blogs.fastiads` (not apex domain), use a CNAME DNS record:
```
blogs.fastiads  CNAME  <github-username>.github.io
```
GitHub's current IPs (for A records, apex domain only):
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

### Folder Structure
```
blogs-fastiads/
├── .github/
│   └── workflows/
│       └── jekyll.yml          ← GitHub Actions deployment
├── _posts/
│   └── 2026-02-22-hello-world.md
├── _drafts/                     ← Not deployed, local only
├── _layouts/
│   └── default.html             ← Must include {% seo %} in <head>
├── _includes/                   ← Partial HTML fragments
├── assets/
│   ├── css/
│   └── images/
├── Gemfile                      ← jekyll ~>4.4, jekyll-seo-tag, webrick
├── Gemfile.lock                 ← MUST be committed
├── _config.yml                  ← permalink: pretty, plugins, url, title
├── CNAME                        ← Single line: blogs.fastiads
├── index.md                     ← Blog home page
└── .gitignore                   ← _site/, .sass-cache/, vendor/
```

### Project Structure Notes
- This is a **greenfield standalone repository** — no muself codebase involvement
- New GitHub repo: `blogs-fastiads` (matches custom subdomain `blogs.fastiads`)
- No backend, no database — pure static site
- All content in `_posts/` as `YYYY-MM-DD-slug.md` files

### .gitignore Essentials
```
_site/
.sass-cache/
.jekyll-cache/
.jekyll-metadata
vendor/
Gemfile.lock  # KEEP THIS IN — do NOT add Gemfile.lock to .gitignore
```
> **Note:** Commit `Gemfile.lock` for reproducible builds. This is different from Ruby gem development practice.

### Cross-Linking Strategy — Siblings Not Cousins

**The problem:** Two separate domains with no links between them = Google treats them as unrelated entities.

**The goal:** Google understands both `fastiads.com` and `blogs.fastiads.com` are the same brand.

**The signals that achieve this:**

| Signal | Where | Impact |
|---|---|---|
| `<a href="fastiads.com">` in header | Every blog page | Crawl path + PageRank flow |
| `<a href="fastiads.com">` in footer | Every blog page | Reinforces brand relationship |
| `rel="publisher"` link in `<head>` | Every blog page | Declares authorship/ownership |
| Organization JSON-LD with `sameAs` | Blog `<head>` | Machine-readable entity link |
| `<a href="blogs.fastiads.com">` in footer | Main site (muself) | Bidirectional = strong signal |
| Consistent `name`, `logo` in both schemas | Both sites | Same entity confirmation |

**Why bidirectional matters:** A one-way link from blog → main is good. Both sites linking to each other is the "siblings" signal. Google's entity graph recognizes `fastiads.com` and `blogs.fastiads.com` as co-owned when the links are mutual and the schema confirms the same `Organization`.

**What NOT to do:**
- Do not use `rel="nofollow"` on cross-site links — that breaks the sibling signal
- Do not use JS-only navigation for these links — Googlebot must crawl them as plain HTML `<a href>`
- Do not put the blog on a completely separate registrar domain (e.g., `fastiadsblog.com`) — subdomain is far stronger

**`_config.yml` variables to add:**
```yaml
main_site_url: "https://fastiads.com"
main_site_name: "fastiads"
blog_description: "The fastiads blog — tips, guides, and insights for performance advertisers"
```

### References
- Jekyll Docs: https://jekyllrb.com/docs/
- jekyll-seo-tag README: https://github.com/jekyll/jekyll-seo-tag
- GitHub Pages GitHub Actions deployment: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- GitHub Pages custom domain: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site

---

## Dev Agent Record

### Agent Model Used

claude-sonnet-4-6

### Debug Log References

_None — clean implementation_

### Completion Notes List

- All file-based tasks (1–6, 8) implemented. Task 7 (local dev / Gemfile.lock) requires running `bundle install` locally with Ruby 3.3. Task 9 is a muself coordination task.
- Custom CSS written from scratch (no minima gem dependency at runtime) to keep the layout self-contained.
- `_layouts/home.html` added (not in original file list) — required because `index.md` uses `layout: home` and minima's home layout wouldn't be available without the theme gem loaded; explicit layout avoids dependency ambiguity.
- `CNAME` contains `blogs.fastiads.com` (with `.com`). Verify against your actual registered domain.

### File List

**Files CREATED:**
- `Gemfile`
- `Gemfile.lock` — ⚠️ PENDING: run `bundle install` locally with Ruby 3.3, then commit
- `_config.yml`
- `CNAME`
- `index.md`
- `.gitignore`
- `_posts/2026-02-22-hello-world.md`
- `_layouts/default.html`
- `_layouts/home.html`
- `_layouts/post.html`
- `_layouts/page.html`
- `_includes/header.html`
- `_includes/footer.html`
- `assets/css/style.css`
- `.github/workflows/jekyll.yml`

**GitHub repo settings to configure (manual):**
- Settings → Pages → Source: **GitHub Actions**
- Settings → Pages → Custom domain: `blogs.fastiads.com`
- Settings → Pages → Enforce HTTPS: enabled

**DNS to configure (manual):**
- `blogs.fastiads.com` CNAME → `<your-github-username>.github.io`
