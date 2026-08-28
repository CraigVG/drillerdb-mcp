# DrillerDB MCP Server

Remote [Model Context Protocol](https://modelcontextprotocol.io) server for
[DrillerDB](https://drillerdb.com), the operating system for water-well and
drilling contractors. Connect your DrillerDB account to Microsoft Copilot,
Claude, ChatGPT, Codex, Cursor, VS Code, or any MCP client and work with your
real jobs, customers, invoices, crews, and well logs in plain English.

```
https://mcp.drillerdb.com
```

| | |
|---|---|
| **Transport** | Streamable HTTP |
| **Authentication** | OAuth 2.0 (Authorization Code + PKCE, Dynamic Client Registration) |
| **Hosting** | Managed by DrillerDB, LLC. Nothing to install or self-host. |
| **Account** | Requires an active DrillerDB account ([app.drillerdb.com](https://app.drillerdb.com)) |
| **Registry name** | `com.drillerdb/drillerdb` |
| **Full docs** | [drillerdb.com/connectors/mcp](https://drillerdb.com/connectors/mcp) |

> This repository holds the public documentation and the
> [`server.json`](server.json) registry manifest for the hosted DrillerDB MCP
> server. The server itself is a managed service and its source is not open.

---

## Quick start

Add the server URL as a custom connector in your MCP client. OAuth sign-in
happens automatically on first use, and access is scoped to your own DrillerDB
company.

### Claude Code

```bash
claude mcp add --transport http drillerdb https://mcp.drillerdb.com
```

### Claude (web and desktop)

Settings -> Connectors -> Add custom connector, then paste
`https://mcp.drillerdb.com`.

### Microsoft Copilot

DrillerDB is preparing the server for Microsoft MCP certification. After
Microsoft publishes the certified connector, administrators can enable it from
Microsoft's agent and connector catalog. The same DrillerDB OAuth sign-in and
company-level permissions apply.

### Cursor / VS Code / other clients

```json
{
  "mcpServers": {
    "drillerdb": {
      "type": "http",
      "url": "https://mcp.drillerdb.com"
    }
  }
}
```

Your client discovers the OAuth endpoints automatically from
`/.well-known/oauth-protected-resource` and the `WWW-Authenticate` header on a
401, so no client ID or secret needs to be configured by hand.

---

## What you can ask

- "Which jobs are ready to schedule this week?"
- "What does Acme Farms still owe us, and how old is it?"
- "Show me the well log for the Johnson project."
- "How is revenue pacing this quarter compared to last?"
- "Which rigs have maintenance coming due?"
- "Draft the field report for today's crew and attach it to the job."

---

## Permission scopes

| Scope | Grants |
|-------|--------|
| `mcp:read` | Read projects, customers, invoices, proposals, work orders, crews, schedule, field reports, equipment, timecards, communications, inventory, and well logs |
| `mcp:write:low` | Trivially reversible writes, such as attaching a note or submitting a field report |
| `mcp:write:high` | State-changing writes, such as scheduling or dispatching a job or sending an invoice |
| `mcp:proposals:write` | Proposal generation and delivery |

Higher-risk write tools are approval-gated: the tool returns an
`approval-required` result first, and the action only runs once you approve it
in the DrillerDB console. Nothing that contacts a customer or moves money
happens without an explicit confirmation.

---

## Security

- **Tenant isolation.** Every query is filtered by the `company_id` bound to
  your OAuth token, never by tool input. There is no way to reach another
  contractor's data.
- **Audit log.** Every tool call is recorded: who called it, which tool, when,
  and the outcome.
- **Revocable.** You can revoke the connector's access at any time from your
  DrillerDB account, which immediately invalidates its tokens.
- **Rate limited.** Per-consumer limits protect the service and your account.
- **First-party.** DrillerDB owns the API and the data. No connector data is
  sold or used for advertising.

See the [DrillerDB Privacy Policy](https://drillerdb.com/privacy) for how data
is collected, used, and retained.

---

## Tool catalog

### Read tools (37)

| Tool | Title |
|------|-------|
| `ar_aging_report` | Accounts Receivable Aging Report |
| `customer_ltv_report` | Customer Lifetime Value Report |
| `get_communication_detail` | Get Communication Detail |
| `get_company_proposal_habits` | Get Company Proposal Habits |
| `get_compliance_form` | Get Compliance Form |
| `get_equipment_detail` | Get Equipment Detail |
| `get_geology_at_location` | Get Geology at Location |
| `get_invoice_detail` | Get Invoice Detail |
| `get_job_profitability` | Get Job Profitability |
| `get_maintenance_due` | Get Maintenance Due |
| `get_project_compliance_status` | Get Project Compliance Status |
| `get_project_detail` | Get Project Detail |
| `get_project_financials` | Get Project Financials |
| `get_proposal_detail` | Get Proposal Detail |
| `get_quote_detail` | Get Quote Detail |
| `get_quote_engagement` | Get Quote Engagement |
| `get_timecard_summary` | Get Timecard Summary |
| `get_work_order_detail` | Get Work Order Detail |
| `list_communications` | List Communications |
| `list_contacts` | List Contacts |
| `list_crews` | List Crews |
| `list_customers` | List Customers |
| `list_equipment` | List Equipment |
| `list_field_reports` | List Field Reports |
| `list_inventory` | List Inventory |
| `list_invoices` | List Invoices |
| `list_my_projects` | List My Projects |
| `list_project_files` | List Project Files |
| `list_projects_by_customer` | List Projects by Customer |
| `list_proposals` | List Proposals |
| `list_timecards` | List Timecards |
| `list_work_orders` | List Work Orders |
| `query_schedule` | Query Schedule |
| `query_well_logs` | Query Well Logs |
| `revenue_pipeline` | Revenue Pipeline |
| `run_report` | Run Report |
| `total_billed_by_contact` | Total Billed by Contact |

### Write tools (8)

| Tool | Title | Scope | Approval-gated |
|------|-------|-------|:--------------:|
| `attach_note_to_project` | Attach Note to Project | `mcp:write:low` | no |
| `dispatch_project` | Dispatch Project | `mcp:write:high` | yes |
| `import_vendor_invoice` | Import Vendor Invoice | `mcp:write:high` | yes |
| `plan_crew_route` | Plan Crew Route | `mcp:write:low` | no |
| `schedule_project` | Schedule Project | `mcp:write:high` | yes |
| `send_invoice` | Send Invoice | `mcp:write:high` | yes |
| `submit_field_report` | Submit Field Report | `mcp:write:low` | no |
| `update_project_status` | Update Project Status | `mcp:write:high` | yes |
All tools declare `openWorldHint: false` — each one operates only on the calling
company's own DrillerDB tenant data.

---

## Support

- Documentation: <https://drillerdb.com/connectors/mcp>
- Email: <support@drillerdb.com>
- Issues with this documentation: open an issue on this repository.

## About DrillerDB

DrillerDB is project management, well logging, scheduling, proposals,
invoicing, equipment tracking, and customer history for water-well and drilling
contractors, in one system. Learn more at [drillerdb.com](https://drillerdb.com).

## License

The documentation in this repository is MIT licensed. Use of the hosted
DrillerDB MCP server is governed by the DrillerDB Terms of Service.
