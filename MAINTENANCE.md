# Website Maintenance Guide

> Personal academic website maintenance manual. Bookmark this file.

**Live site:** <https://xavier-hxh.github.io>
**CV repo:** <https://github.com/Xavier-hxh/cv-latex>
**Website repo:** <https://github.com/Xavier-hxh/Xavier-hxh.github.io>

---

## File Map

| Want to change... | Edit this file |
|------------------|----------------|
| Homepage intro / Research Interests / News / Contact | `_pages/about.md` |
| Publications list | `_publications/*.md` (one file per paper) |
| Working papers | `_working_papers/*.md` |
| Awards | `_pages/awards.md` |
| Conference presentations | `_pages/talks.html` |
| CV page (intro + selected highlights) | `_pages/cv.md` |
| Navigation menu order | `_data/navigation.yml` |
| Author sidebar (photo / bio / email / Scholar link) | `_config.yml` (author: block) |
| Color theme / fonts | `_sass/custom.scss` |
| Photo | `images/profile.jpg` (just replace the file) |

---

## Common Tasks

### Add a new publication

1. Copy an existing one as a template:
   ```bash
   cp _publications/2025-01-01-huang-carbon-market-MAC.md \
      _publications/2026-09-01-NEW-short-name.md
   ```
2. Edit the new file. Six fields to update:
   - `title` — paper title
   - `permalink` — `/publication/2026-short-name` (must be unique)
   - `date` — `YYYY-MM-DD` (controls sort order on the list page)
   - `venue` — journal name
   - `paperurl` — DOI link (preferred) or journal page
   - `citation` — full citation in HTML, with your name in `<strong>` and `<sup>+</sup>` / `<sup>*</sup>` markers
3. Commit and push:
   ```bash
   git add -A && git commit -m "add: [Venue] paper title" && git push
   ```
4. Wait ~2 minutes; new entry appears at <https://xavier-hxh.github.io/publications/>.

### Update News on homepage

Edit `_pages/about.md`. The `## News` section is just a Markdown list; add new items at the top, prune older items as needed.

```bash
git add _pages/about.md && git commit -m "news: <event>" && git push
```

### Change photo

Replace `images/profile.jpg` with the new file (same name, JPG format, recommended 400×400 or larger). Push.

### Add a conference talk

Edit `_pages/talks.html`. The structure is grouped by year (`<h2>2026</h2>`). Each talk is one `<p>` block:

```html
<p>
  <strong>Conference name</strong><br>
  City, Country · Date<br>
  <em>"Paper title"</em>
</p>
```

### Add an award

Edit `_pages/awards.md`. It's a Markdown table — add a row.

### Update CV (PDF)

Do not edit CV here. Go to the `cv-latex` repo:

```bash
cd /path/to/Website/cv-latex
# Edit sections/*.tex
xelatex cv.tex      # local preview
open cv.pdf
git add -A && git commit -m "update: ..." && git push
```

GitHub Actions auto-rebuilds and the website's "Download CV (PDF)" button always points to the latest version. No website change needed.

---

## When Things Break

### Site doesn't update after push

- Check <https://github.com/Xavier-hxh/Xavier-hxh.github.io/actions> — look for a red X on the latest run.
- Click into the failed run → expand the failing step → read the last 30 lines.

### Common error: YAML front matter

Symptoms: build fails with a Liquid or kramdown error mentioning a specific `.md` file.

Causes:
- Forgot the closing `---` after front matter
- Unescaped `:` inside a value (wrap value in `"..."`)
- Mismatched quotes

Fix: revert the file with `git checkout HEAD~1 -- path/to/file.md`, then redo the edit carefully.

### Emergency rollback

```bash
git log --oneline | head -5      # find last good commit
git revert HEAD                  # undo most recent commit
git push
```

---

## Build / Deploy Mechanics

- **Trigger:** Any push to `master` branch.
- **Workflow:** `.github/workflows/jekyll.yml` runs Ruby 3.3 + Jekyll, builds `_site/`, deploys via `actions/deploy-pages@v4`.
- **Build time:** ~30 seconds. **Total push-to-live:** ~2 minutes.
- **CV PDF link** on `/cv/` page is a static redirect to `cv-latex/releases/latest/download/cv.pdf` — never needs updating.

---

## Conventions

- Commit message: `<verb>: <short description>` (e.g. `add: Applied Energy paper`, `update: 2026 news`, `fix: typo in cv page`).
- `+` after surname = first / co-first author; `*` = corresponding author. Note this convention is explained inline on the Publications and Working Papers pages.
- Paper title color (dark blue `#00008B`) and contact link color (medium blue `#1F4E9C`) are intentionally different. See `_sass/custom.scss` for the palette.

---

## What NOT to Touch

- `Gemfile.lock` — auto-managed; conflicts are easier resolved by deleting + `bundle install`.
- `_site/` directory — build output, gitignored.
- `_includes/` and `_layouts/` — Minimal Mistakes theme internals; edit only if you know what you're doing.
- Files under `vendor/`, `assets/js/vendor/` — third-party code.

---

*Last updated: 2026-05-27*
