# Microsoft Teams MCP server

Find a team joined by your delegated account and inspect its channels before preparing a status report.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `MICROSOFT_TEAMS_ACCESS_TOKEN`

For local OAuth, use node local-auth/cli.mjs login ./connector.json with your own provider app; see AUTH.md. Your tokens remain in the native OS credential store.

Credential help: https://raw.githubusercontent.com/microsoftgraph/msgraph-metadata/master/openapi/v1.0/openapi.yaml

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call list_joined_teams '{}'
```

## Use with an MCP client

Launch `node /absolute/path/microsoft-teams-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| list_joined_teams | Read | List teams joined by the delegated user; requires Team.ReadBasic.All. |
| list_team_channels | Read | List channels in one authorized team; requires Channel.ReadBasic.All. |

## Concrete workflow

Find a team joined by your delegated account and inspect its channels before preparing a status report.

1. list_joined_teams
2. list_team_channels

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-microsoft-teams-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/microsoft-teams/).

## Provenance and maintenance

- https://learn.microsoft.com/en-us/graph/api/overview
- https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow
- https://learn.microsoft.com/en-us/graph/api/user-list-joinedteams?view=graph-rest-1.0
- https://learn.microsoft.com/en-us/graph/api/channel-list?view=graph-rest-1.0

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
