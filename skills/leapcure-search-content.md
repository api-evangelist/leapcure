---
name: Search across the Leapcure blog
description: Run a unified search across Leapcure blog posts and pages using the WordPress REST search endpoint, then hydrate the matches.
api: openapi/leapcure-blog-content-openapi.yml
operations: [listWpV2Search, getWpV2PostsById, getWpV2PagesById]
---

# Search across the Leapcure blog

The WordPress REST search endpoint returns lightweight, cross-type matches (posts and pages) that you then hydrate with the full resource. Anonymous — no credentials required.

## Base
`https://blog.leapcure.com/wp-json`

## Steps

1. **Search** — `listWpV2Search` (`GET /wp/v2/search`) with `search=<terms>`. Optionally constrain `type` (`post`) and `subtype`, and paginate with `page`/`per_page`. Each result carries `id`, `title`, `url`, `type` and `subtype`.
2. **Hydrate a post match** — for results where `subtype` is `post`, call `getWpV2PostsById` (`GET /wp/v2/posts/{id}`) with the result `id` to get the full content, excerpt, author and terms.
3. **Hydrate a page match** — for `subtype` `page`, call `getWpV2PagesById` (`GET /wp/v2/pages/{id}`).
4. **Enrich** — add `_embed` to the hydrate calls to inline author and featured media, or `_fields` to trim the payload.

## Conventions
- **Pagination**: `page`/`per_page` with `X-WP-Total` / `X-WP-TotalPages` headers.
- **Errors**: flat WordPress envelope `{ code, message, data: { status } }` — see `errors/leapcure-problem-types.yml`.
- Search is read-only and safe to retry.
