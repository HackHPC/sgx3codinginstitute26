# SGX3 Coding Institute 2026 — Project Context

## What This Is

A Jekyll site hosted on GitHub Pages for the **SGX3 Coding Institute & Hackathon 2026**,
an immersive virtual training program for undergrad/grad students focused on HPC, AI,
and science gateway development. Run by the SGX3 / Science Gateways Community Institute.

**Live URL:** https://hackhpc.github.io/sgx3codinginstitute26/
**GitHub org:** hackhpc
**Dates:** Training June 1-19, Hackathon June 22-26, 2026
**Stipend:** $2,000 per participant
**Apply:** https://sciencegateways.org/coding_institute

---

## CRITICAL: File Write Workaround

The working directory path contains a Unicode RIGHT SINGLE QUOTATION MARK (U+2019) in the
folder name inside `Documents`. The `Write` and `Edit` tools resolve this as a straight
apostrophe (U+0027), silently creating a phantom duplicate directory instead of writing
into the actual repo.

**ALL file writes must go through Bash using Python to locate the real repo path:**

```python
import subprocess, os
result = subprocess.run(
    ['find', '/Users/jeaimehp/Documents', '-maxdepth', '2',
     '-name', 'sgx3codinginstitute26', '-type', 'd'],
    capture_output=True, text=True
)
paths = result.stdout.strip().split('\n')
repo = next(p for p in paths if os.path.isdir(os.path.join(p, '.git')))
```

The real repo is whichever result has a `.git` directory. Once `repo` is set,
write files with `open(os.path.join(repo, 'filename'), 'w')` or use a `cat` heredoc
in bash after setting `REPO` with the Python snippet above.

**Never use the Write or Edit tools directly** - they will write to the wrong location.

---

## Tech Stack

- **Jekyll** (GitHub Pages supported build, no custom plugins beyond jekyll-seo-tag)
- **Theme:** `jekyll-theme-slate` (dark Navy/Blue gradient header, white content area)
- **CSS:** `assets/css/style.scss` - imports Slate then applies brand overrides
- **Markdown:** kramdown with GFM input
- **Local dev:** `bundle install && bundle exec jekyll serve`

---

## Brand Colors

```
SGX3 Blue  #1B5BA8   core brand - headings, nav active state, primary links
Navy       #0D3A5C   header gradient start, H2 headings, footer background
Sky Blue   #4A90E2   H1 underline accent, tagline text in header
Teal       #1D9E75   benefit cards, training date badge, highlight box accent
Coral      #D85A30   CTA buttons, hackathon date badge, hackathon section tag
Purple     #534AB7   "Applications Open" tag, Coding Institute resource group border
Mid Gray   #888      footer note text
Light Gray #F5F7FA   card and box backgrounds
Border     #D0D9E8   dividers, nav button borders
```

---

## File Structure

```
sgx3codinginstitute26/
|-- CLAUDE.md               <- this file (auto-loaded by Claude at session start)
|-- _config.yml             <- Jekyll config (theme, baseurl, url)
|-- index.md                <- Home page (layout: page)
|-- resources.md            <- Resources page (layout: page, permalink: /resources/)
|-- _layouts/
|   `-- page.html           <- Extends Slate default; injects nav.html before content
|-- _includes/
|   `-- nav.html            <- Home / Resources nav with active-state Liquid logic
|-- assets/
|   `-- css/
|       `-- style.scss      <- Brand overrides after @import "{{ site.theme }}"
|-- Gemfile                 <- github-pages + webrick gems
|-- .gitignore              <- excludes _site/, .jekyll-cache/, vendor/, Gemfile.lock
`-- README.md               <- Original plain-text description (excluded from build)
```

---

## Page Layout Architecture

Both pages use `layout: page` (not `layout: default`).

`_layouts/page.html` has front matter `layout: default`, making it extend the Slate
theme's built-in default layout. It injects `{% include nav.html %}` before
`{{ content }}`. This is the correct Jekyll pattern for adding shared UI to a
third-party theme without copying the entire theme layout HTML.

To add a new page: create `pagename.md` with `layout: page` in front matter, then
add a link to `_includes/nav.html`.

---

## Pages Built

### Home (`index.md` -> `/`)
Sections in order:
1. "Applications Open" section tag
2. Hero heading + description
3. Coral CTA button -> sciencegateways.org/coding_institute
4. Program Dates - three color-coded date badges (SGX3 Blue / Teal / Coral)
5. Participant Benefits - 5-card grid, Teal top border
6. What You'll Learn - 6-item curriculum grid, SGX3 Blue left border
7. Training Experience - Teal highlight box
8. Hackathon Experience - Coral section tag
9. Who Should Apply - bullet list + second CTA button
10. Resources - link to Task Sheet Google Doc
11. Footer note

### Resources (`resources.md` -> `/resources/`)
All tools, links, and platforms from Coding Institute materials and chat logs.
7 categories, each with a distinct brand-color left border:

| Category                  | Left Border | CSS modifier              |
|---------------------------|-------------|---------------------------|
| Coding Institute & SGX3   | Purple      | .resource-group.purple    |
| Learning & Certifications | SGX3 Blue   | .resource-group.blue      |
| Development & AI Tools    | Teal        | .resource-group.teal      |
| APIs & Data Sources       | Sky Blue    | .resource-group.sky       |
| Scholarships & Internships| Coral       | .resource-group.coral     |
| Outreach & NCAR Resources | Navy        | .resource-group.navy      |
| Miscellaneous             | Gray        | .resource-group.gray      |

Row structure: resource name on the left, action link button or `<code>` API endpoint
on the right. Link button color is inherited from the parent group color modifier.
Rows have alternating background (zebra stripe) for scannability.

---

## Custom CSS Classes Reference

| Class                       | Description                                       |
|-----------------------------|---------------------------------------------------|
| .cta-button                 | Coral filled CTA button with hover lift effect    |
| .section-tag                | Small uppercase purple pill label                 |
| .section-tag.curriculum     | SGX3 Blue variant                                 |
| .section-tag.hackathon-tag  | Coral variant                                     |
| .date-badge                 | SGX3 Blue inline date pill                        |
| .date-badge.training        | Teal variant                                      |
| .date-badge.hackathon       | Coral variant                                     |
| .date-row                   | Flex wrapper for .date-badge items                |
| .benefit-grid               | Auto-fit CSS grid for benefit cards               |
| .benefit-card               | Card with Teal top border                         |
| .curriculum-grid            | Auto-fit CSS grid for curriculum items            |
| .curriculum-item            | Card with SGX3 Blue left border                   |
| .highlight-box              | Left-bordered callout box (SGX3 Blue default)     |
| .highlight-box.teal         | Teal border variant                               |
| .site-nav                   | Nav bar wrapper (injected by _layouts/page.html)  |
| .nav-link / .nav-link.active| Individual nav pill links                         |
| .resource-group + modifier  | Resource category container with color left border|
| .resource-item              | Single resource row (zebra-striped)               |
| .resource-link              | Action link button (inherits group color)         |
| .resource-code              | Monospace chip for raw API endpoint strings       |

---

## What Has NOT Been Done Yet

- No logo or image assets - header shows text title and tagline only
- No favicon
- No social/OG meta beyond what jekyll-seo-tag generates automatically
- No schedule or agenda page
- No instructor or team bios page
- The phantom directory (straight-apostrophe path) in Documents can be deleted
