# DrillerDB MCP certification evaluation evidence

## Runtime contract

- Endpoint: `https://mcp.drillerdb.com`
- Transport: Streamable HTTP
- Authentication: OAuth 2.0 Authorization Code with PKCE, refresh tokens, and
  revocation
- Discovery: OAuth protected-resource and authorization-server metadata
- Catalog: 45 externally visible tools, comprising 37 reads and 8 writes

## Security controls

- Company scope is bound from the verified token and never accepted from tool
  input.
- Every tool is also filtered by the user's current DrillerDB role.
- Higher-risk writes require durable human approval before dispatch.
- Every tool call records actor, tool, time, request correlation data, latency,
  and outcome.
- OAuth tokens are revocable and access-token revocation is enforced on every
  request.
- Present browser Origin values are accepted only when their raw serialized
  value exactly matches a trusted origin. Malformed, noncanonical, opaque, and
  untrusted origins return HTTP 403 before authentication or tool dispatch.
- Originless native MCP clients remain supported.
- Rate limits are enforced per client and tenant authority.

## Public verification checks

| Check | Expected result |
|---|---|
| `GET /.well-known/oauth-authorization-server` | HTTP 200 with authorization, token, registration, and revocation endpoints |
| `GET /.well-known/oauth-protected-resource` | HTTP 200 with DrillerDB MCP as the protected resource |
| Originless `POST /` without a bearer token | HTTP 401 with OAuth discovery information |
| Request with an untrusted or malformed Origin | HTTP 403 before authentication |
| Trusted browser Origin preflight | HTTP 204 with the allowed Origin |
| Authenticated `tools/list` | 45 tools with titles and read/destructive annotations |

## Functional coverage

Representative tests cover project reads, invoice and accounts-receivable
reports, schedules, well logs, role filtering, tenant isolation, scope denial,
token revocation, approval creation and resume, origin handling, rate limiting,
and audit writes. Higher-risk actions are tested both before approval (handler
must not run) and after an authorized approval resume.

## Reviewer notes

The service contains no autonomous money-movement tool. Sending invoices or
other customer-facing documents is approval-gated and uses the customer's
configured DrillerDB data. Microsoft reviewers can use a dedicated,
pre-populated DrillerDB validation tenant; credentials and setup notes should be
provided only through Partner Center's secure certification fields.
