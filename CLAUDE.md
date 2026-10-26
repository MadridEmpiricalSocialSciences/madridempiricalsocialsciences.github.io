# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The website of the Madrid Empirical Social Sciences (MESS) network, served by GitHub Pages at https://madridempiricalsocialsciences.github.io from the `master` branch. It is a fork of the **Beautiful Jekyll** theme (Dean Attali), with a MESS brand layer added on top. `README.md`, `CHANGELOG.md`, `beautiful-jekyll-theme.gemspec`, `Appraisals`, `staticman.yml` and `.github/` are leftover theme files, not MESS documentation.

The site is published by pushing to `master`: GitHub Pages builds it, with no separate deploy step. Pushing therefore makes changes public, so get the user's go-ahead first. To confirm a deploy finished:

```
gh api repos/MadridEmpiricalSocialSciences/madridempiricalsocialsciences.github.io/pages/builds/latest --jq '.status + " " + .commit[0:7]'
```

## Local preview

`bundle exec jekyll serve` does not work here. Ruby 3.2 is installed but its development headers are not, so the native gems `eventmachine` and `http_parser.rb`, which are only needed for live reload, fail to compile. The workaround is to install Jekyll into a scratch `GEM_HOME` without those gems and bypass Bundler:

```
export GEM_HOME=<scratch>/gems JEKYLL_NO_BUNDLER_REQUIRE=true
gem install --no-document jekyll-paginate kramdown-parser-gfm webrick addressable colorator mercenary liquid:'~>4.0' kramdown rouge safe_yaml terminal-table pathutil i18n jekyll-sass-converter jekyll-watch
gem install --no-document --ignore-dependencies jekyll
printf 'plugins:\n  - jekyll-paginate\n' > <scratch>/local.yml   # skip jekyll-sitemap (its gem activation fails)
J=$(ls -d $GEM_HOME/gems/jekyll-4*)
ruby -I$J/lib $J/exe/jekyll serve --config _config.yml,<scratch>/local.yml -d <scratch>/site --port 4123
```

To check a page visually, take screenshots with `google-chrome --headless=new --screenshot=out.png --window-size=1440,2000 <url>`, and also at a phone width of 390. URLs without a trailing slash, such as `/mess2026`, redirect, so use `curl -L`. There are no tests or linters. CI (`.github/workflows/ci.yml`) is the theme's own build check.

## Design

The look follows the 2026 print materials (call for papers and program). The font is **Lato**, and the colours come from the overlapping-circles logo: teal `#1a7f9c`, light teal `#62a6a8`, orange `#f0a32d`, ink `#132326`.

- `assets/css/mess.css` holds all MESS styling. `_config.yml` loads it via `site-css`, and it overrides the theme's CSS variables. Keep theme files (`assets/css/beautifuljekyll.css`, the theme layouts and includes) unmodified and add styles here instead.
- The navbar logo is `assets/img/mess-logo-web.png` (`title-img`). `favicon.ico`, `apple-touch-icon.png` and `assets/img/mess-mark.png` hold the circles-only mark. The partner-logo strip, `assets/img/partner-logos.png`, was extracted from the 2026 program PDF.

## Where content lives

- **Home page:** `index.html` uses the custom `_layouts/landing.html`.
  - The hero text, buttons, figures and photo are set in the front matter of `index.html`.
  - The cards come from `_data/upcoming.yml`, which is the main thing to update between events. Their section heading is `cards-title` in the front matter of `index.html` (default "What's next").
  - "Latest news" shows the newest 3 posts.
- **News:** `news.html` (`layout: home`) lists all posts. Pagination is disabled (no `paginate` in `_config.yml`).
- **Navbar:** set by `navbar-links` in `_config.yml`. Conferences are a dropdown with the newest first.
- **Conference pages:** `workshop.md` (2024, with the full program inline), `mess2025.md` (a recap of a past event) and `mess2026.md` (the current conference). Conventions for these pages:
  - Key details go in a `<div class="mess-facts" markdown="1">` box at the top.
  - Schedules use HTML tables with `class="mess-schedule"`, where break rows use `colspan`.
  - Maps are `<figure class="mess-map">` elements linking to the PDF.
  - After an event, the page is rewritten in the past tense and the logistics sections are removed.
  - Participant instructions (paper upload, time limits, discussants, chairs, venue) are copied from the last page of that year's program PDF. Don't invent them.
- **Photo slideshows:** `{% include gallery.html %}` renders a Bootstrap carousel from a `gallery:` list (`img`, optional `caption`) in the page's front matter.
  - Slideshow images are 4:3 web copies in `assets/img/gallery/` (about 1400px wide, JPEG quality 80).
  - The full-size conference photos live in `assets/img/conferences/`, which also holds other conference photo copies such as the home page hero (`MESS2025-01-group-web.jpg`).
  - The carousel crops every photo to 4:3, so use landscape photos only.
- **Other pages:** `aboutus.md` lists the scientific committee as alternating photo-left and photo-right Bootstrap rows, with the name linked to the member's website. `gettogether.md` is overwritten for each meetup. There is also `mailinglist.md`.
- **Announcements:** `_posts/YYYY-MM-DD-slug.md` (`layout: post`). The two 2020 sample posts are theme demos kept with `published: false`.
- **Assets:** files sit flat in `assets/img/` and are linked by absolute path. Year-specific files are prefixed `MESS<year>`, for example `MESS2026_Program.pdf`. Programs are updated by replacing the PDF under the same filename.
  - Avoid spaces in filenames.
  - Give large photos a web-sized copy.
  - Use JPG for photos and PNG for line art such as floor plans.
- **Contact email:** `madridempiricalsocialsciences@gmail.com`. Registration and sign-up forms are external Google Forms links.

Old calls for papers and similar sections are often kept as HTML comments (`<!-- ... -->`) in the page instead of being deleted.

Site-wide settings (colours, Google Analytics `gtag`, timezone `Europe/Madrid`, permalink format `/:year-:month-:day-:title/`) are in `_config.yml`. See the theme `README.md` for the front-matter parameters that pages and posts support.
