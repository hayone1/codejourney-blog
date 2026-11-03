# codejourney-blog

Content for the codejourney site. The site (`codejourney` repo) reads Markdown from `content/blog/` with Nuxt Content.

## Writing a post

Add `content/blog/<slug>.md`:

```md
---
title: My post
description: One-sentence summary (used for SEO and cards).
date: 2026-11-03
updated: 2026-11-10        # optional, boosts freshness ranking
category: DevOps
tags: [kubernetes, docker]
image: /images/my-post.png # optional, files go in public/images
featured: false            # featured posts rank higher on the home page
draft: false               # drafts are hidden in production
minRead: 6
author:
  name: Boluwatife Morawo
  avatar: https://github.com/<you>.png
---
```

Body supports Markdown plus Nuxt UI prose components (`::callout`, `::tip`, `::code-group`, etc.).

## Publishing flows

1. **Git:** merge to `main`. The site rebuilds and pulls the new content.
2. **Nuxt Studio:** edit visually at `https://<site>/_studio`. Studio commits to the `studio` branch, and the `studio-pr` workflow opens (or updates) a pull request into `main`.
