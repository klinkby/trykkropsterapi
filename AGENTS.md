# AGENTS.md

Hugo site for trykmassage. Static, deployed to GitHub Pages.

## Commands

```bash
hugo server -D                      # dev, localhost:1313
hugo --gc --minify                  # build to public/
hugo --gc --minify --printPathWarnings   # must be warning-free before commit
```

Hugo 0.152+, **extended edition** — needed for its native WebP image encoding
(`layouts/partials/picture.html`). Still no SCSS/Dart Sass: plain CSS only, that's a
separate feature this project doesn't use.

## Layout

| Path | What |
| --- | --- |
| `content/` | Pages, Markdown, Danish |
| `data/` | Structured data (prices etc.) — edit here, never in templates |
| `layouts/` | Own templates. No external theme, no submodules |
| `assets/css/main.css` | Entire design system, one file |
| `assets/img/` | Images, run through `layouts/partials/picture.html` |

## Rules

**Never invent facts.** Prices, phone, hours, testimonials, credentials must trace to a real
source. No source, no claim — leave a TODO and say so. Never fabricate a client quote.

**Danish.** Idiomatic Danish, `du` form, correct æ/ø/å. No generic wellness copy.

**No medical promises.** Complementary treatment, legally cannot promise results.

**Colour.** All text pairs must pass WCAG AA 4.5:1; verify, don't assume.

**Zero third-party requests.** System fonts only, local images only. No Google Fonts, no CDN, no
analytics. CSP is `default-src 'self'` and GDPR depends on it.

**No inline scripts or handlers.** The CSP allows inline JSON-LD via a build-time SHA-256 hash
computed in `layouts/partials/head.html`; anything else inline will be blocked.

**Markdown is not HTML.** `markup.goldmark.renderer.unsafe = false`. Use a shortcode instead.

## Front matter

All pages: `title`, `description` (≤155 chars, becomes the meta description), `draft`.

## Before committing

Build clean with no warnings. Check the change at 390px and 1280px. Conventional commit messages
(`feat:`, `fix:`, `chore:`).

Open items are listed under "Mangler afklaring" in README.md.
