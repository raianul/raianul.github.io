# raianul.github.io

Bengali-language CS education blog deployed at **blog.raian.xyz** via GitHub Pages + Jekyll.

## Stack

- Jekyll 4.3 with GitHub Actions CI/CD (auto-deploy on push to `main`)
- Ruby 3.3 (via `actions/setup-ruby` — do NOT install Jekyll locally)
- Layout: `_layouts/post.html` — includes sidebar, GA4, Hind Siliguri font, highlight.js 11.9, Mermaid@10 ESM
- Footer: `_includes/footer.html` — SVG social icons + "© 2026 raian.xyz"
- Homepage: `index.html` — dynamic article cards with client-side category filtering (Liquid + JS)

## Content

All articles are Markdown with Jekyll front matter. Categories:

| Dir | Topic |
|-----|-------|
| `oop/` | OOP series (Bengali) |
| `system-design/` | System Design series (Bengali) |
| `dsa/` | DSA series |
| `networking/` | Networking series |
| `databases/` | Databases series |

Front matter template for new articles:

```yaml
---
layout: post
title: "শিরোনাম"
description: "এক লাইনের বিবরণ"
date: YYYY-MM-DD
author: রাইয়ান
category: oop   # or system-design, dsa, networking, databases
---
```

## Custom Skills

Skills live in `.claude/skills/`. The `bangla-tech-writer` skill is for writing Bengali CS tutorial articles in this blog's style.

## Licensing

- Code (site infrastructure): MIT
- Blog content (articles): CC BY-NC 4.0
