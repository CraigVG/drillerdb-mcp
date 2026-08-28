# DrillerDB MCP Server

DrillerDB is the operating system for water-well and drilling contractors. The
DrillerDB MCP server connects a contractor's live company account to Microsoft
Copilot and other MCP-compatible agents.

## What the connector does

Use natural language to work with:

- drilling projects, customers, contacts, proposals, and invoices;
- work orders, crews, schedules, field reports, and timecards;
- equipment, inventory, maintenance, and job profitability;
- compliance forms and project compliance status; and
- well logs and geology at a project location.

The connector publishes 45 tools: 37 read tools and 8 write tools. The exact
tools available to a user are filtered by the user's current DrillerDB role and
granted OAuth scopes.

## Requirements

- An active DrillerDB account.
- Permission to access the relevant company data and feature in DrillerDB.
- An MCP-compatible Microsoft agent experience with the connector enabled by
  the organization's administrator.

## Sign in and authorization

DrillerDB uses OAuth 2.0 Authorization Code with PKCE. During connection, sign
in with the same credentials used at https://app.drillerdb.com. The connector
never asks for a DrillerDB API key.

Available OAuth scopes:

- `mcp:read` for projects, customers, invoices, schedules, equipment,
  communications, compliance data, and well logs.
- `mcp:write:low` for low-risk additive operations such as notes and field
  reports.
- `mcp:write:high` for higher-risk operational or customer-facing actions.
- `mcp:proposals:write` for proposal workflows.

## Security and user control

- Every request is scoped to the signed-in user's DrillerDB company. The
  company identifier is taken from the verified access token, not tool input.
- The current DrillerDB role is checked in addition to the OAuth scope.
- Higher-risk and externally visible actions pause for explicit human
  approval before they run.
- Every tool call is audit-logged with actor, operation, time, request
  correlation data, and outcome.
- Access can be revoked at any time and revoked tokens stop working
  immediately.
- Browser-originated MCP requests must match the exact trusted-origin
  allowlist. Originless native MCP clients remain supported.

## Example prompts

- "Which active jobs need follow-up this week?"
- "Show accounts receivable aging for my company."
- "Summarize today's crew schedule and equipment assignments."
- "Find well logs near this project and compare the formations."

## Known issues and limitations

- An active network connection is required. The managed MCP server does not
  provide an offline mode.
- Results are limited to data and features available to the signed-in user's
  company and DrillerDB role.
- Higher-risk actions return an approval-required response and do not execute
  until the user approves them in DrillerDB.
- Customer-facing delivery actions depend on the company's configured contact
  data and delivery settings.
- The connector does not move money or execute financial transactions.

## Support and policies

- Documentation: https://drillerdb.com/connectors/mcp
- Support: support@drillerdb.com
- Privacy policy: https://drillerdb.com/privacy
- Terms of use: https://drillerdb.com/terms
- Public MCP metadata: https://github.com/CraigVG/drillerdb-mcp
