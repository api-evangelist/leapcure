---
name: Browse Leapcure blog content
description: Read published posts, pages, categories and tags from the Leapcure blog (WordPress REST API) without authentication.
api: openapi/leapcure-blog-content-openapi.yml
operations: [listWpV2Posts, getWpV2PostsById, listWpV2Categories, listWpV2Tags, listWpV2Comments]
---

# Browse Leapcure blog content

Read-only access to the Leapcure blog (`https://blog.leapcure.com/wp-json`, WordPress REST API `wp/v2`). All operations below respond anonymously — no credentials required.

## Base
`https://blog.leapcure.com/wp-json`

## Steps

1. **List recent posts** — `listWpV2Posts` (`GET /wp/v2/posts`). Use `per_page` (max 100) and `page` to paginate; results default to `orderby=date`, `order=desc`. Read `X-WP-Total` and `X-WP-TotalPages` response headers for the total count and page count.
2. **Filter** — narrow with `search` (free text), `categories`, `tags`, `author`, `after`/`before` (ISO 8601), or `slug`. Request `_embed` to inline author, terms and featured media under the `_embedded` envelope; use `_fields` for a sparse response.
3. **Fetch one post** — `getWpV2PostsById` (`GET /wp/v2/posts/{id}`) with the `id` from the list step.
4. **Resolve taxonomy** — `listWpV2Categories` and `listWpV2Tags` map the `categories[]`/`tags[]` ids on a post to names/slugs. Filter posts by them via the `categories`/`tags` query parameters.
5. **Read comments** — `listWpV2Comments` (`GET /wp/v2/comments`) with `post={id}` returns approved comments for a post.

## Conventions
- **Pagination**: page-number (`page`/`per_page`), plus RFC 8288 `Link` header (`rel="next"`/`"prev"`).
- **Errors**: flat WordPress envelope `{ code, message, data: { status } }` — see `errors/leapcure-problem-types.yml`. A 404 is `rest_no_route` / `rest_post_invalid_id`.
- **No idempotency keys**; these are safe idempotent GETs regardless.
