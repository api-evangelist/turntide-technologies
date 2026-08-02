---
name: Connect to the Turntide MCP server
description: Discover and authenticate against the OAuth-protected Model Context Protocol server
  Turntide publishes at turntide.com, using the RFC 9728 / RFC 8414 well-known metadata it
  actually serves.
api: openapi/turntide-technologies-wordpress-rest-openapi.yml
generated: '2026-08-02'
method: generated
operations:
  - getMcp
  - createMcpMcpOauthServer
  - getMcpMcpOauthServer
  - deleteMcpMcpOauthServer
---

# Connect to the Turntide MCP server

## What exists

Turntide runs a WordPress MCP Adapter on its corporate site. Verified live on 2026-08-02:

| Surface | URL | Status |
|---|---|---|
| MCP endpoint (OAuth) | `https://turntide.com/wp-json/mcp/mcp-oauth-server` | 200 route, `401 mcp_unauthorized` anonymously |
| MCP endpoint (session) | `https://turntide.com/wp-json/mcp/mcp-adapter-default-server` | `401 rest_forbidden` anonymously |
| Protected resource metadata | `https://turntide.com/.well-known/oauth-protected-resource` | 200 (RFC 9728) |
| Authorization server metadata | `https://turntide.com/.well-known/oauth-authorization-server` | 200 (RFC 8414) |

## Steps

1. **Discover the resource** — `GET https://turntide.com/.well-known/oauth-protected-resource`.
   It returns `resource: https://turntide.com/wp-json/mcp/mcp-oauth-server`,
   `authorization_servers: ["https://turntide.com"]`, `bearer_methods_supported: ["header"]`,
   `scopes_supported: ["mcp"]`.
2. **Discover the authorization server** —
   `GET https://turntide.com/.well-known/oauth-authorization-server`. It returns
   `authorization_endpoint https://turntide.com/oauth/authorize`,
   `token_endpoint https://turntide.com/oauth/token`,
   `revocation_endpoint https://turntide.com/oauth/revoke`,
   `code_challenge_methods_supported ["S256"]`,
   `token_endpoint_auth_methods_supported ["none"]` and
   `client_id_metadata_document_supported: true`.
3. **Register the client** — there is **no** dynamic client registration endpoint. The server
   advertises `client_id_metadata_document_supported`, so present a client-id metadata document
   URL as your `client_id`. Clients are public (`token_endpoint_auth_methods_supported: none`),
   so **PKCE with S256 is mandatory** — no client secret is used.
4. **Authorize** — authorization-code flow against `/oauth/authorize` with `scope=mcp`,
   `code_challenge_method=S256`. Calling it without parameters returns `400`.
5. **Exchange** — POST the code to `/oauth/token` with the `code_verifier`. `refresh_token` is a
   supported grant type; `/oauth/revoke` revokes.
6. **Speak MCP** — POST JSON-RPC 2.0 to
   `https://turntide.com/wp-json/mcp/mcp-oauth-server` with
   `Authorization: Bearer <token>` and
   `Accept: application/json, text/event-stream`. Start with
   `{"jsonrpc":"2.0","id":1,"method":"tools/list"}`.

## Rules

- **The tool set is not publicly known.** Anonymous `tools/list` returns
  `{"code":"mcp_unauthorized","message":"MCP authentication required.","data":{"status":401}}`.
  Enumerate tools at runtime from the authenticated `tools/list` — never assume a tool exists.
- The adapter projects the site's registered **WordPress Abilities**; the same registry is
  readable over REST at `wp-abilities/v1` (`getWpAbilitiesV1Abilities`,
  `getWpAbilitiesV1AbilitiesByName`), which is also `401` anonymously.
- This is a **content/CMS** agent surface for turntide.com. It is not an interface to Turntide's
  motors, power electronics, energy storage or thermal products.
- Errors carry `{code, message, data.status}` — not RFC 9457 problem details.
