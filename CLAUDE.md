# Czechitas Slidev Template — Claude Code Context

## What this repository is

Template repository for Czechitas Slidev presentations. Each course gets its own repo created from this template. Output is a presentation deployed to GitHub Pages with a PDF export.

**This is the Digital Academy: Cybersecurity course template — slide content is in English.**

## First steps when creating a new repo from this template

Replace all occurrences of `REPO_NAME` with the actual repository name in:
- `package.json` — `name` field and `build` script
- `.github/workflows/deploy-slides.yml` — PDF export step

Then update `slides.md`:
- `title`, `author`, `date` in frontmatter
- Cover slide heading
- Bio slide details (name, subtitle, bullets, QR link)

Enable GitHub Pages in the new repo:
- Repo must be **public** (Pages is not available for private repos on the free plan)
- Go to **Settings → Pages → Source** → set to **GitHub Actions**
- Without this the deploy job will fail with HTTP 404
- The template repo itself is private — deploy fails there by design, that is expected and OK

---

## Repository structure

```
slides.md              ← main file: frontmatter, slide order via src:, lecturer bio
slides/                ← individual sections (00-agenda.md, 01-topic.md, …)
public/                ← images used in slides (ondrej.png is always Ondřej's bio photo)
theme/                 ← Czechitas theme (DO NOT modify without good reason)
  layouts/             ← Vue layouts: bio, cover, section, default, image-right, center
  components/          ← CzechitasLogo.vue, QRCode.vue
  styles/index.css     ← brand CSS variables and global styles
  assets/              ← Czechitas logos and illustrations
.github/workflows/     ← deploy-slides.yml (build + PDF export + GitHub Pages deploy)
```

## What CHANGES between courses
- `slides.md` — frontmatter (title, author, date), `src:` file order, lecturer bio
- `slides/*.md` — all slide content
- `public/` — course-specific images (klara.png, topic screenshots, etc.)

## What NEVER changes
- `theme/` — shared Czechitas theme, do not modify
- `public/ondrej.png` — always Ondřej Šrámek's bio photo, never rename or delete

---

## Czechitas brand

### Colors (CSS variables)
```css
--czechitas-pink: #E6007E    /* primary, headings on gradient slides */
--czechitas-blue: #2D2E83   /* headings on white background */
--czechitas-cyan: #00BFE7
--czechitas-yellow: #FFCB04
--czechitas-orange: #F36F21
--czechitas-green: #8CC63E
--czechitas-purple: #91268F
--czechitas-gradient: linear-gradient(135deg, #E6007E 0%, #6B1FA0 50%, #00BFE7 100%)
```

### Fonts
- Sans: **Open Sans** (300, 400, 700, 800)
- Mono: **Source Code Pro**

---

## Available layouts

### `cover` — title slide
```md
---
layout: cover
subtitle: Digital Academy — Cybersecurity
author: Ondřej Šrámek
date: Month YYYY
---
# Course Title
```
White text on Czechitas gradient, automatically inserts the woman-with-laptop illustration.
Props `subtitle`, `author`, and `date` are all optional but should always be filled in — they appear below the title.
The cover slide must have `layout: cover` as its own slide-level frontmatter (separate from the global headmatter).

### `section` — chapter divider
```md
---
layout: section
subtitle: Optional subtitle
---
# Section Title
```
Czechitas gradient background, large white heading. Use before every new chapter.

### `bio` — lecturer slide
```md
---
layout: bio
image: /ondrej.png
name: "Ondřej Šrámek"
subtitle: "GMON, GNFA, GCTI"
---
- bullet describing experience
::qr::
<QRCode url="https://linktr.ee/ondrejsramek" :size="120">Linktree</QRCode>
```
The `::qr::` slot is optional — layout handles its absence gracefully.

### `default` — standard content slide
White background, blue heading. Used for ~90% of content.

### `image-right` — slide with image on the right
```md
---
layout: image-right
image: /image-name.png
---
# Heading
Text on the left…
```
Column ratio is **3:2** (text : image) — text side is wider.
Defined in `theme/layouts/image-right.vue` as `grid-template-columns: 3fr 2fr`.

**Image-only slides** (no body text): always keep `# Heading` for slide annotations in the overview. The Czechitas logo in the bottom-right corner is added automatically by the layout footer.
```md
---
layout: image-right
image: /screenshot.png
---
# Description of what is shown
```

### `center` — centered content
For closing slides (Thank You, Feedback).

---

## Components

### `<QRCode>`
```vue
<QRCode url="https://example.com" :size="160">Label below QR</QRCode>
```
Generates QR via api.qrserver.com. Use only inside the `::qr::` slot in the `bio` layout.

### `<CzechitasLogo>`
```vue
<CzechitasLogo size="small" />
<CzechitasLogo size="medium" :invert="true" />
```
Automatically included in layout footers — do not add manually to slide content.

---

## HTML utility classes (inline in Markdown)

### Icon grid (agenda, topic overview)
```html
<div class="icon-grid">  <!-- 3 columns by default, add class="cols-2" for 2 -->
  <div class="icon-card">
    <div class="icon">🔍</div>
    <div class="label">Description</div>
  </div>
</div>
```

### Callout boxes
```html
<div class="callout">Info box (cyan)</div>
<div class="callout warning">Warning box (orange)</div>
```

### Chat / conversation pattern
For demonstrating AI prompts and responses (useful in cybersecurity/AI topics). Use with `v-click` to reveal turns one by one.
```html
<div class="chat-prompt">User message (blue bubble, right-aligned)</div>
<div class="chat-response">AI response (grey bubble, left-aligned)</div>
```

### Gradient background on a custom div
```html
<div class="czechitas-gradient-bg">...</div>
```

---

## slides.md and section file structure

### slides.md — main file
```md
---
theme: ./theme
title: Course Name
author: Lecturer Name
date: Month YYYY
mdc: true
shiki:
  theme: github-light
fonts:
  sans: Open Sans
  mono: Source Code Pro
---
---
layout: cover
subtitle: Digital Academy — Cybersecurity
author: Lecturer Name
date: Month YYYY
---
# Course Title
---
layout: bio
image: /ondrej.png
name: "Ondřej Šrámek"
subtitle: "GMON, GNFA, GCTI"
---
- bullet
::qr::
<QRCode url="https://linktr.ee/ondrejsramek" :size="120">Linktree</QRCode>
---
src: ./slides/00-agenda.md
---
---
src: ./slides/01-topic.md
---
---
layout: center
---
# Thank You

[linktr.ee/ondrejsramek](https://linktr.ee/ondrejsramek)
```

### File naming in /slides
Convention: `NN-name.md` where NN is a zero-padded sequence number (00, 01, 02, …).

Every file in `slides/` must start with its own `---` frontmatter block defining the layout.

---

## Tables
Use standard Markdown tables — header row automatically gets blue background, even rows are light grey.

```md
| Tool          | Usage               |
|---------------|---------------------|
| **Kali Linux**| Pentesting distro   |
```

---

## Running, deploying and exporting

### Local dev
```bash
npm install
npm run dev      # starts Slidev dev server
```

### Deploy — GitHub Pages
Deploy triggers automatically on push to `main` via GitHub Actions (`.github/workflows/deploy-slides.yml`).
URL pattern: `https://srameko.github.io/<repo-name>/`

The base path is baked into the `build` script in `package.json`:
```json
"build": "slidev build --base /REPO_NAME/ --out dist"
```
Replace `REPO_NAME` with the actual repo name.

**Prerequisites for deploy to work:**
1. Repo must be **public**
2. GitHub Pages must be enabled: Settings → Pages → Source → **GitHub Actions**

> The template repo (`czechitas-cybersecurity-slidev-template`) is private — the deploy job fails there by design. The build and PDF export steps still pass and can be verified in CI.

The full workflow (`.github/workflows/deploy-slides.yml`):
```yaml
name: Deploy Slides

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

env:
  FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true

jobs:

  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Build Slidev
        run: npm run build

      - name: Export PDF
        run: npx slidev export --output dist/REPO_NAME.pdf

      - name: Upload Pages Artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

When creating a repo from this template, replace `REPO_NAME` on the `Export PDF` line with the actual repo name (same value as in `package.json` build script).

### PDF export
PDF is exported automatically in CI via:
```yaml
run: npx slidev export --output dist/REPO_NAME.pdf
```
Do NOT add `playwright install-deps` — it is not needed.
`playwright-chromium` in devDependencies is sufficient.

After deploy the PDF is accessible at:
`https://srameko.github.io/<repo-name>/<repo-name>.pdf`

---

## README structure

After creating a repo from this template, update `README.md` to:

```markdown
# <Presentation Title>

<One paragraph describing what the presentation covers and which Czechitas course it belongs to.>

## Slides

Latest version available online:
https://srameko.github.io/<repo-name>/

## Download PDF

[<repo-name>.pdf](https://srameko.github.io/<repo-name>/<repo-name>.pdf)
```

---

## Feedback slide

Every presentation ends with a feedback slide before "Thank You". The placeholder URL must be replaced with the actual feedback form link before the course:

```md
---
layout: center
---
# Feedback

<!-- TODO: Replace URL with the actual feedback form link -->
<QRCode url="https://FEEDBACK_FORM_URL" :size="200">Feedback form</QRCode>
```

The `<QRCode>` component works outside the `bio` layout too — it renders inline wherever placed.

---

## Syncing other presentations with this template

This repository (`czechitas-cybersecurity-slidev-template`) is the **source of truth** for the theme and slide structure. When updating existing course presentations to match this template, check and align:

### What to sync (copy from template)
- `theme/` — entire directory, always copy verbatim; never diff individual files
- `.github/workflows/deploy-slides.yml` — CI/CD pipeline structure (keep `REPO_NAME` already replaced in the target repo)
- `package.json` — dependency versions (`@slidev/cli`, `playwright-chromium`); keep the target repo's `name` and `build` base path

### What NOT to overwrite
- `slides.md` — content, lecturer bio, and `src:` order are course-specific
- `slides/*.md` — all course content
- `public/` — course-specific images (only `ondrej.png` must be present)
- `README.md` — course-specific description and URLs

### Checklist when syncing a course repo
1. Replace `theme/` with the version from this template
2. Check `package.json` — align dependency versions, preserve `name` and `build` script base path
3. Check `.github/workflows/deploy-slides.yml` — align structure, verify `REPO_NAME` is already replaced
4. Check `slides.md` cover slide — has `layout: cover` with `subtitle`, `author`, `date` props
5. Check feedback slide is present before "Thank You" with a real QR URL (not `example.com`)
6. Verify `public/ondrej.png` exists
7. Verify repo is **public** and GitHub Pages is enabled: Settings → Pages → Source → **GitHub Actions**

---

## Common issues

- **Image not showing** — paths in `public/` start with `/image.png` (no `public` prefix). The `bio` and `image-right` layouts handle `BASE_URL` automatically during build.
- **bio layout without QR** — simply omit the `::qr::` slot, the layout works fine without it.
- **Gradient missing on section slide** — the `section` layout applies `.czechitas-gradient-bg` automatically. On `default` layout add it manually via `<div class="czechitas-gradient-bg">`.
- **Frontmatter in src files** — every slide file in `slides/*.md` must start with its own `---` frontmatter block defining the layout.
- **Wrong layout name** — chapter divider slides use `layout: section`, not `layout: subtitle`. There is no `subtitle` layout in the theme.
- **PDF export failing** — do not add `playwright install-deps` to CI; `playwright-chromium` in devDependencies is sufficient.
- **REPO_NAME not replaced** — `package.json` and `.github/workflows/deploy-slides.yml` both contain `REPO_NAME` placeholder; replace both before first deploy.
- **Deploy fails with HTTP 404** — GitHub Pages is not enabled or the repo is private. Go to Settings → Pages → Source → GitHub Actions. Repo must be public.
- **Deploy fails in the template repo** — expected, the template repo is private. Build and PDF export still run and pass.
