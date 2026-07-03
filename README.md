# shelnet-resources

Data repo for [shelnet-site](https://github.com/lui-gi/shelnet-site): the
manifest (`manifest.json`), certification question banks (`bytes/`, `certs/`),
lab writeups (`writeups/`), and visualization modules (`visualizations/`) that
the site fetches at runtime.

Published to GitHub Pages; the site reads from
`https://lui-gi.github.io/shelnet-resources` (overridable at build time via
`VITE_RESOURCES_BASE_URL`).

## Layout

- `manifest.json` — top-level index, consumed by the site's manifest service.
- `bytes/` — per-cert rapid-fire question banks (JSON).
- `certs/` — PBQs, mock exams, and quizzes grouped by cert.
- `visualizations/` — interactive concept modules.
- `writeups/` — lab and project writeups.

The site owns URL choices; this repo is content only.
