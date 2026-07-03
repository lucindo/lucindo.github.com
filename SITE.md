# lucindo.github.io — site guide

Read this file before changing the site. It records what the page is, the rules
it must keep following, and the design decisions behind it, so future updates
stay coherent.

## What this is

A single-page index of the small web apps Renato Lucindo publishes, served by
GitHub Pages at <https://lucindo.github.io/> from the `master` branch of the
`lucindo/lucindo.github.com` repo. Pushing to `master` deploys.

The previous site (a 2015 Jekyll blog) is preserved for historical reasons on
the `backup/old-site` branch. Nothing from it carries forward.

## Files

- `index.html` — the entire site: content, CSS, and theme JS in one file.
- `404.html` — served by GitHub Pages for unknown URLs; same design tokens,
  no theme toggle button (it still respects the stored/system theme).
- `.nojekyll` — tells GitHub Pages to skip the Jekyll build.
- `SITE.md` — this file.
- `CLAUDE.md` — pointer here so Claude Code loads this context automatically.

## Hard rules

1. **One self-contained file.** Everything inline in `index.html`. No external
   requests of any kind: no webfonts, no CDNs, no analytics, no images hosted
   elsewhere. The page must work offline once loaded.
2. **No build step, no frameworks.** Plain HTML/CSS/vanilla JS, editable by hand.
3. **System fonts only** (stacks below), so the file stays small.
4. **Nothing tracked.** The colophon in the footer promises this; keep it true.
5. **Content is an index, not a portfolio essay.** One-line descriptions,
   plain language, no marketing voice.

## Design: "Editorial" (Tufte-informed)

Chosen July 2026 from three candidates (Tufte margin-notes, editorial,
monospace catalogue). Serif for reading, Gill Sans-style sans for small labels
— the pairing Tufte himself uses. Restraint is the aesthetic: whitespace and
hairlines, one accent color, the only motion a 0.3em arrow nudge on hover.

### Type

- Serif (body, headings): `"Iowan Old Style", "Palatino Linotype", Palatino, Charter, Georgia, serif`
- Sans (kickers, meta, footer links, theme toggle): `Seravek, "Gill Sans", "Gill Sans MT", "Trebuchet MS", Calibri, sans-serif`
- Sans labels are uppercase with 0.14–0.22em letter-spacing, 0.68–0.78rem.
- Headings are weight 400 — size and face carry the hierarchy, not boldness.
- Base 1.125rem / line-height 1.55; column `max-width: 40rem`, centered.

### Color tokens (CSS custom properties on `:root`)

| Token        | Light     | Dark      | Role                          |
|--------------|-----------|-----------|-------------------------------|
| `--paper`    | `#fcfcfa` | `#15181b` | background                    |
| `--ink`      | `#16181a` | `#e4e6e6` | text                          |
| `--muted`    | `#70757a` | `#8d939a` | secondary text, footer        |
| `--hairline` | `#dcdcd6` | `#32373c` | rules between entries         |
| `--accent`   | `#22577c` | `#85b5d4` | Prussian blue: eyebrow, arrows, hovers |

Use the accent sparingly — eyebrow, arrows, hover states, focus rings. Never
for body text.

### Theme switching contract

- Default follows the system via `prefers-color-scheme`; `color-scheme: light dark`
  is set on `:root`.
- Explicit override = `data-theme="light|dark"` on `<html>`, persisted in
  `localStorage` key `theme`. Absent key = auto.
- The toggle (fixed top-right, sans, lowercase value) cycles auto → light → dark.
- An inline `<script>` in `<head>` applies the stored theme **before** CSS
  paints — keep it there to avoid a flash on load.
- `404.html` runs the same pre-paint script but has no toggle button.

### Accessibility floor (do not regress)

- Visible `:focus-visible` outlines on every interactive element.
- `prefers-reduced-motion: reduce` disables all transitions.
- Semantic structure: one `h1`, apps as a `ul` of entries with `h2` titles.
- Muted-on-paper contrast stays ≥ 4.5:1 for body-size text.

## Adding a new app

Add an `<li class="entry">` inside `<ul class="works">` (newest first or
whatever order reads best — order is curation, not chronology):

```html
<li class="entry">
  <p class="kicker">Web app · CATEGORY</p>
  <h2><a href="https://lucindo.github.io/SLUG/">App Name <span class="arrow">→</span></a></h2>
  <p class="desc">One plain sentence saying what it does.</p>
  <p class="meta"><a href="https://lucindo.github.io/SLUG/">open app</a><span class="sep">|</span><a href="https://github.com/lucindo/SLUG">source</a></p>
</li>
```

- Kicker: `Web app · <one-word category>` (existing: Breathwork, Meditation).
- Description: one sentence, ≤ ~15 words, describes rather than sells. An em
  dash for a second beat is fine ("— follow the circle, slow the breath").
- Every entry links the app *and* its source repo.

## Footer

Two lines: sans uppercase links (location · GitHub · X · Instagram · LinkedIn),
then the italic serif colophon: "One HTML file, no frameworks, nothing tracked
· source". If a rule above ever changes, update the colophon to stay honest.

## Deploying

```sh
git push origin master
```

Then check <https://lucindo.github.io/> (Pages usually rebuilds in under a
minute). There is no staging: open `index.html` locally in a browser first —
both themes, narrow viewport, keyboard tab order.
