# MANIAC Lab website

Source for [maniaclab.uchicago.edu](https://maniaclab.uchicago.edu), a Jekyll site
published with GitHub Pages from the `master` branch.

## Editing the team page

Edit `_data/team.yml`. It has two lists, `current` and `former`; move a person
between them when they join or leave, and add a `tenure` such as `2019–2026`
to former members. Portraits live in `assets/team/` (square images, 500 px is
plenty). See the comments at the top of the file for the fields.

## Local build

Requires Ruby 3.3 (see `.ruby-version`) and Bundler.

```bash
bundle install
LANG=en_US.UTF-8 bundle exec jekyll serve
```

## Front-end bundle

`assets/js/main.js` is built from UIkit, Simple-Jekyll-Search and
`assets/js/custom.js` with `npm run build` (uglify-js). UIkit is pinned to
`3.0.0-rc.5` because the SCSS vendored under `_sass/uikit/` is that version;
upgrade both together. jQuery and FlexSlider are loaded in `_includes/head.html`
(jQuery from cdnjs with a Subresource Integrity hash).
