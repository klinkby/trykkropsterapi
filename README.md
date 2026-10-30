# trykmassage

Statisk website bygget med [Hugo](https://gohugo.io/) og udgivet på GitHub Pages.

## Kom i gang

Kræver Hugo 0.152 eller nyere, **extended**-udgaven — den bruges til at kode
billeder som WebP; der bruges ikke SCSS.

```bash
hugo server -D     # udviklingsserver på http://localhost:1313
hugo --gc --minify # byg til public/
```

## Struktur

| Sti                   | Indhold                                          |
| --------------------- | ------------------------------------------------ |
| `content/`            | Alle sider som Markdown                          |
| `data/`               | Strukturerede data — redigér her, ikke i skabeloner |
| `layouts/`            | Egne skabeloner. Der bruges intet eksternt tema  |
| `assets/css/main.css` | Hele designsystemet i én fil                     |
| `assets/img/`         | Billeder                                         |

### Billeder

Billeder lægges som JPEG/PNG i `assets/img/`. `layouts/partials/picture.html`
kører dem gennem Hugos billedpipeline til et `<picture>` med WebP og
original-formatet som fallback:

```gotemplate
{{ partial "picture.html" (dict "path" "img/nyt-billede.jpg" "alt" "…") }}
```

Markdown-billeder (`![alt](/img/x.jpg)`) går automatisk gennem samme partial via
`layouts/_default/_markup/render-image.html`.

## Udgivelse

`.github/workflows/deploy.yml` bygger og udgiver ved push til `main`.
`.github/workflows/build.yml` bygger hver pull request og fejler ved advarsler.

Før første udrulning skal **Settings → Pages → Source** sættes til
**GitHub Actions**.

Sitet er sat til **ikke at blive indekseret** (`noindex = true` i `hugo.toml`
giver `Disallow: /` i robots.txt og et `noindex`-metatag).

**Ved lancering:**

1. Opret `static/CNAME` med domænet
2. `baseURL` i `hugo.toml` → domænet
3. `noindex` → `false`
4. Slå **Enforce HTTPS** til i Settings → Pages, når certifikatet er udstedt

## Mangler afklaring

- **Domæne.** Ukendt — `static/CNAME` mangler, og `baseURL` peger på
  `klinkby.github.io/trykmassage/`.
- **Indhold og design.** Kommer i næste trin.

## Sikkerhed

- Content-Security-Policy med `default-src 'self'` sættes som `<meta>`-tag.
  Inline CSS og JSON-LD tillades via SHA-256-hashes, der beregnes ved bygning.
- Rå HTML i Markdown er slået fra (`markup.goldmark.renderer.unsafe = false`).
- Ingen inline event-handlere.

**Kendt begrænsning:** `frame-ancestors` og `X-Frame-Options` kan ikke sættes,
fordi GitHub Pages ikke tillader egne HTTP-headere.
