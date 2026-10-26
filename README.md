# Turntide Technologies

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
