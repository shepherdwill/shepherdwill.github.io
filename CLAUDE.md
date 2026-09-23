# CLAUDE.md

Public personal website for an economist (Will Shepherd, CEP/LSE), served by GitHub Pages from `main`. Research code lives in a separate private repository; do not add analysis code or data here.

- Plain static HTML/CSS; no build step, no Jekyll (`.nojekyll` is present).
- Single page (`index.html`) with a sleek, minimal design. Each section is a `<section>` with an `<h2>` label and `.entry` blocks; add content by copying an existing entry.
- All styling is in `assets/css/style.css`; colours are CSS variables on `:root`; the site is always light, with no dark mode.
- Preview locally with `python3 -m http.server 8000`.
