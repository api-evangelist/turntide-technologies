---
name: Browse Turntide site content
description: Read Turntide Technologies' published news, product pages and media over the public
  WordPress REST API at turntide.com/wp-json, using pagination, sparse fieldsets and embedding
  correctly.
api: openapi/turntide-technologies-wordpress-rest-openapi.yml
generated: '2026-08-02'
method: generated
operations:
  - getWpV2Posts
  - getWpV2PostsById
  - getWpV2Pages
  - getWpV2PagesById
  - getWpV2Media
  - getWpV2MediaById
  - getWpV2Categories
  - getWpV2Tags
  - getWpV2Users
---

# Browse Turntide site content

Base URL: `https://turntide.com/wp-json`

This is the corporate website's content API. It is **not** a product API — Turntide publishes no
developer API for its motors, power electronics, energy storage or thermal hardware.

## Authentication

None required for reading published content. Do not send credentials. Write operations and the
`wp-abilities/v1` / `mcp` namespaces return `401 rest_forbidden` anonymously and are out of scope
for this skill.

## Steps

1. **List news / articles** — `getWpV2Posts` (`GET /wp/v2/posts`).
   - Page with `page` and `per_page`. `per_page` is capped at **100**; a larger value returns
     `400 rest_invalid_param` with `data.params.per_page` explaining the bound.
   - Read `X-WP-Total` and `X-WP-TotalPages` from the response headers to size the collection,
     and follow the `Link` header `rel="next"` rather than incrementing `page` blindly.
   - Filter with `search`, `after` / `before` (ISO 8601), `categories`, `tags`, `author`,
     `slug`, `order`, `orderby`.
2. **Trim the payload** — add `_fields=id,slug,link,title,date` so you are not pulling rendered
   HTML for every item. Verified working on this host.
3. **Walk relationships in one request** — add `_embed=1` to inline the author
   (`wp:featuredmedia`, `author`, `wp:term`) under `_embedded` instead of issuing follow-up
   calls to `getWpV2UsersById` / `getWpV2MediaById` / `getWpV2Categories`.
4. **Read one item** — `getWpV2PostsById` (`GET /wp/v2/posts/{id}`). An unknown id returns
   `404 rest_post_invalid_id`; do not retry it.
5. **Product and solution pages** live under `getWpV2Pages` / `getWpV2PagesById`, and are
   hierarchical via the `parent` field.
6. **Assets** — `getWpV2Media` / `getWpV2MediaById` return `source_url`, `mime_type`,
   `media_type` and `media_details` (size variants). Datasheets and installation guides are
   published as attachments and linked from `https://turntide.com/documents/`.

## Rules

- **No idempotency contract exists.** There is no idempotency key. Never blind-retry a write.
- **Errors are not RFC 9457.** Parse `{code, message, data.status}`; validation failures add
  `data.params` and `data.details`. See `errors/turntide-technologies-problem-types.yml`.
- **No rate-limit headers are published.** Nothing signals throttling; stay conservative,
  respect `Cache-Control: max-age=600` and do not hammer collections.
- `context=edit` requires authentication; use the default `view` context.
