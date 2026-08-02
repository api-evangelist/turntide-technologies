---
name: Search the Turntide site
description: Answer questions about Turntide Technologies' products, solutions, case studies and
  news by querying the site-wide WordPress search endpoint and resolving the hits to full
  records.
api: openapi/turntide-technologies-wordpress-rest-openapi.yml
generated: '2026-08-02'
method: generated
operations:
  - getWpV2Search
  - getWpV2PostsById
  - getWpV2PagesById
  - getWpV2Types
  - getWpV2Taxonomies
---

# Search the Turntide site

Base URL: `https://turntide.com/wp-json`

## Steps

1. **Query** — `getWpV2Search` (`GET /wp/v2/search`) with `search=<terms>`.
   Supported parameters on this host: `context`, `page`, `per_page`, `search`, `type`,
   `subtype`, `exclude`, `include`.
2. **Read the hit shape** — each result is `{id, title, url, type, subtype}` only. `type` is
   `post` or `term`; `subtype` names the concrete post type or taxonomy.
3. **Resolve** — fetch the full record with `getWpV2PostsById` when `subtype` is `post`, or
   `getWpV2PagesById` when `subtype` is `page`. Use `_fields` to trim and `_embed=1` to inline
   the featured image and terms.
4. **Discover what is searchable** — `getWpV2Types` lists the post types registered on this
   site (with `rest_base` and `rest_namespace`, which tell you the collection endpoint for each);
   `getWpV2Taxonomies` lists the taxonomies and the types they apply to.
5. **Cite** — every hit carries a public `url` on turntide.com. Cite that, not the REST URL.

## Worked example

To find Turntide's axial flux motor material: `getWpV2Search` with
`search=axial flux&per_page=20`, then resolve `subtype: page` hits to
`getWpV2PagesById`. Product pages live under `/products/` and `/solutions/`; articles live under
`/community/` and `/news/`.

## Rules

- `per_page` max is 100 — a larger value returns `400 rest_invalid_param`.
- Search is a read-only surface; there is no write path and no idempotency contract.
- Do not treat the search endpoint as an inventory of Turntide's hardware. It indexes published
  website content only.
