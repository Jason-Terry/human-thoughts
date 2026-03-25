# Human Thoughts

A personal blog at [human-thoughts.blog](https://human-thoughts.blog), built with Astro and deployed to GitHub Pages.

## Stack

- **Framework:** Astro 6 (static site generation)
- **Typography:** Fraunces (body), JetBrains Mono (code)
- **Analytics:** Umami (self-hosted on Railway)
- **Contact:** FastAPI + Resend ([ht-service](https://github.com/Jason-Terry/ht-service))
- **Captcha:** Cloudflare Turnstile
- **Comments:** Giscus (GitHub Discussions)

## Local Development

```sh
npm install
npm run dev
```

Runs at `localhost:4321`.

## Writing Posts

Add markdown files to `src/content/blog/`. Frontmatter:

```md
---
title: "Post Title"
date: 2026-03-25
---
```

Use `<!-- preview -->` markers in the body to define which section appears as the preview on the home page. Multiple markers = random selection per build.

## Deployment

Pushes to `main` trigger a GitHub Actions workflow that builds and deploys to GitHub Pages. Custom domain is configured via `public/CNAME`.
