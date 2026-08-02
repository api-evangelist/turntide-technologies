# Turntide Technologies

Turntide Technologies is an electrification company that designs and manufactures axial flux
electric motors, power electronics and inverters, modular battery energy storage, and thermal
management systems for electric and hybrid platforms across agriculture, automotive, commercial
vehicles, construction, gensets, industrial equipment, marine, rail and recreation. Founded in
2013 as Software Motor Corporation and rebranded to Turntide in 2020.

- Website: https://turntide.com/
- Documents: https://turntide.com/documents/
- Support: https://support.turntide.com/hc/en-us
- GitHub: https://github.com/turntidetechnologies
- Secondary market: https://forgeglobal.com/turntide-technologies_stock/

## API posture

**Turntide publishes no public developer API for its hardware products.** No developer portal,
no API reference, no SDKs, no CLI, no sandbox, no webhooks, no changelog, no status page, no
`security.txt`, and no A2A agent card were found on any Turntide host.

What the enrichment pipeline did find on `turntide.com` is a real, live machine-readable surface
belonging to the corporate website:

| Surface | Location | Notes |
|---|---|---|
| WordPress REST API | `https://turntide.com/wp-json` | 274 routes / 485 operations across 14 namespaces |
| WordPress Abilities API | `wp-abilities/v1` | `401` anonymously |
| MCP server | `https://turntide.com/wp-json/mcp/mcp-oauth-server` | WordPress MCP Adapter; `401 mcp_unauthorized` anonymously |
| OAuth AS metadata (RFC 8414) | `/.well-known/oauth-authorization-server` | `200` — PKCE S256, scope `mcp`, public clients |
| OAuth protected resource (RFC 9728) | `/.well-known/oauth-protected-resource` | `200` — names the MCP endpoint |

`openapi/turntide-technologies-wordpress-rest-openapi.yml` was **derived** mechanically from the
live route index at `https://turntide.com/wp-json/`, with component schemas harvested from live
HTTP `OPTIONS` responses on the `wp/v2` collections. It describes the website CMS surface, not a
product API.

`app.turntide.com` hosts the legacy Turntide-for-Buildings application, which now serves
BrainBox AI branding (`OEM: brainboxai`) following the sale of the building automation division.

## Certifications

ISO 9001 (quality), ISO 14001 (environmental) and ISO 45001 (occupational health and safety),
certified by BSI under UKAS accreditation, published on the homepage. **No information-security
certification** (SOC 2, ISO 27001, PCI DSS, HIPAA, FedRAMP) is published and no trust center
exists.
