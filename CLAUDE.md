# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The website of the Madrid Empirical Social Sciences (MESS) network, served by GitHub Pages at https://madridempiricalsocialsciences.github.io from the `master` branch. It is a fork of the **Beautiful Jekyll** theme (Dean Attali). `README.md`, `CHANGELOG.md`, `beautiful-jekyll-theme.gemspec`, `Appraisals`, `staticman.yml` and `.github/` are leftover theme files, not MESS documentation. The site is maintained by pushing straight to `master`, and GitHub Pages builds it. There is no separate deploy step.

## Build / preview

Ruby, Bundler and Jekyll are not installed on this machine, so in practice changes are not previewed locally. If a Ruby toolchain is available:

```
bundle install
bundle exec jekyll serve      # local preview at http://localhost:4000
bundle exec jekyll build      # output goes to _site/ (gitignored)
```

CI (`.github/workflows/ci.yml`) is the theme's own build check, which uses `appraisal`. There are no tests or linters.

## Where content lives

Almost all edits are to Markdown content, not to the theme:

- **Navbar**: `navbar-links` in `_config.yml`. A new top-level page or conference must be added here to appear in the menu. Conferences are a dropdown, with the newest first.
- **Standalone pages** (root-level `.md`, `layout: page` by default): `aboutus.md` (organizers and members), `gettogether.md` (the next informal meetup; overwritten for each event), `mailinglist.md`, and one page per conference: `workshop.md` (2024, with the full program inline) and `mess2025.md` (2025, which links to a PDF program instead).
- **Announcements**: `_posts/YYYY-MM-DD-slug.md` with `layout: post` front matter (`cover-img`, `thumbnail-img`, `share-img`, `tags`, `author`). These appear in the paginated feed on the home page (`index.html`, `layout: home`). The two 2020 sample posts are theme demos kept with `published: false`.
- **Assets**: all images and PDFs (calls for papers, programs, logos, people photos) go flat in `assets/img/` and are linked by absolute path, e.g. `/assets/img/MESS2025-Program.pdf`. Year-specific files are prefixed `MESS<year>-`. Updating a program usually means replacing the PDF in place under the same filename.
- **Contact email** used throughout: `madridempiricalsocialsciences@gmail.com`. Registration and sign-up forms are external Google Forms links.

Old calls for papers and similar sections are often kept as HTML comments (`<!-- ... -->`) in the page instead of being deleted.

## Theme internals

`_layouts/`, `_includes/` and `assets/css|js/` are the unmodified Beautiful Jekyll theme. Site-wide settings (colours, avatar `/assets/img/mess.jpg`, Google Analytics `gtag`, permalink format `/:year-:month-:day-:title/`) are in `_config.yml`. See the theme `README.md` for the front-matter parameters that pages and posts support.
