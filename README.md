# Sociality.io MCP

Connect AI agents to Sociality.io social media intelligence through a remote Model Context Protocol server.

Sociality.io MCP gives MCP-compatible clients structured OAuth access to owned social account analytics, published content, public competitor intelligence, competitor benchmarking, post insights, and reporting workflows across Facebook, X, Instagram, YouTube, LinkedIn, and TikTok.

It is built for teams, builders, and AI workflows that need social media intelligence outside a dashboard: reporting assistants, internal copilots, competitor monitoring, cross-channel analysis, and custom products powered by Sociality.io data.

## Remote MCP Server

```text
https://api.sociality.io/mcp
```

Transport: `streamable-http`

Authentication: OAuth

OAuth scope: `mcp:use`

## What Sociality.io MCP Does

Sociality.io MCP exposes social media account and social media competitor intelligence through MCP tools, resources, and prompts.

With owned social accounts, clients can:

- List connected social accounts available to the authenticated user.
- Fetch account-level analytics for selected metrics and date ranges.
- Fetch published posts and stories for account-level analysis.
- Compare owned account performance across platforms and periods.

With competitor workflows, clients can:

- List tracked public competitors in the current workspace.
- Add a competitor tracking record from a social profile URL when explicitly requested.
- Fetch public competitor stats for benchmarking.
- Fetch competitor published posts for content and engagement analysis.
- Compare owned accounts with tracked competitors across supported channels.

For workflow planning and safety, clients can:

- Read workspace context and MCP credit status.
- Discover supported channels, features, metrics, and aggregation behavior.
- Inspect available tools, billing status, and read/write behavior.
- Use built-in prompts for tool guidance and readiness checks where the MCP client supports prompts.

## Documentation

- Product page: https://sociality.io/mcp
- MCP docs: https://sociality.io/docs
- Tool reference: https://sociality.io/docs/reference/tools
- Client setup: https://sociality.io/docs/getting-started/connect-your-client
- Safe usage: https://sociality.io/docs/security/safe-mcp-usage

## Supported Tools

| Tool | Access | Credit usage | Purpose |
| --- | --- | --- | --- |
| `social_accounts_list` | Read | Free | List social media accounts available to the authenticated user. |
| `social_account_stats_list` | Read | Billable | Fetch owned account analytics for a date range and selected metrics. |
| `social_account_posts_list` | Read | Billable | Fetch published owned account posts and stories for a date range. |
| `social_competitors_list` | Read | Free | List tracked competitors in the current workspace. |
| `social_competitors_create` | Write | Free | Add a tracked competitor from a social profile URL. |
| `social_competitor_stats_list` | Read | Billable | Fetch public competitor stats for a date range and selected metrics. |
| `social_competitor_posts_list` | Read | Billable | Fetch competitor published posts for a date range. |

## Supported Channels

- Facebook
- X
- Instagram
- YouTube
- LinkedIn
- TikTok

For the most accurate channel, feature, metric, and aggregation coverage, read the `social_platform_capabilities` resource from the MCP server.

## Supported Resources

| Resource | URI | Purpose |
| --- | --- | --- |
| `social_workspace_context` | `sociality://workspace/context` | Returns current team and user context, plus MCP credit `usage`, `limit`, and `resets_at`. |
| `social_tools_catalog` | `sociality://tools/catalog` | Returns the registered tool catalog, including credit usage and read-only/write status. |
| `social_platform_capabilities` | `sociality://platform/capabilities` | Returns supported channels, feature coverage, metric names, metric descriptions, and aggregation behavior. |

## Supported Prompts

| Prompt | Purpose |
| --- | --- |
| `social_guide_tool_usage` | Explains recommended tool usage, credit behavior, pagination, supported channels, and metric selection patterns. |
| `social_guide_readiness_check` | Returns a workspace readiness check with credit status, connected account counts, and warnings such as expired tokens. |

Prompt support depends on the MCP client. Some clients expose prompts directly, while others may only expose tools and resources.

## Use Cases

- Generate recurring social media performance summaries from live Sociality.io data.
- Compare owned account performance across channels, campaigns, and reporting periods.
- Benchmark competitors by follower, engagement, post, and publishing patterns.
- Analyze competitor content to identify posting cadence, creative formats, and engagement signals.
- Build internal copilots that answer account, post, and competitor questions from structured data.
- Feed social media intelligence into reporting workflows, monitoring systems, and customer-facing products.
- Run readiness checks before larger workflows to confirm workspace access, available data sources, supported metrics, and credit status.

## Client Setup

For clients that support remote HTTP MCP servers:

```json
{
  "mcpServers": {
    "sociality": {
      "url": "https://api.sociality.io/mcp"
    }
  }
}
```

For ChatGPT, Claude, Gemini Enterprise, Gemini CLI, and other supported clients, see the setup docs:

https://sociality.io/docs/getting-started/connect-your-client

For Claude Code:

```bash
claude mcp add --transport http sociality https://api.sociality.io/mcp
```

For Codex:

```bash
codex mcp add --url https://api.sociality.io/mcp sociality
```

If a client only supports stdio MCP servers, use a bridge such as `mcp-remote`:

```json
{
  "mcpServers": {
    "sociality": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://api.sociality.io/mcp"]
    }
  }
}
```

## Verify Setup

After connecting a client, use this no-credit setup check before running larger workflows:

1. Read `social_workspace_context` to confirm OAuth, active workspace, user context, and credit status.
2. Call `social_accounts_list` and `social_competitors_list` to confirm accessible owned accounts and tracked competitors.
3. Read `social_platform_capabilities` to confirm supported channels, metrics, and aggregation behavior.
4. Read `social_tools_catalog` to distinguish free tools, billable tools, read-only tools, and write actions.

This is especially useful before recurring reporting, competitor benchmarking, internal copilot, or production agent workflows.

## Credits, Limits, and Safety

- Billable tools consume MCP credits based on returned rows.
- Free setup and discovery tools do not consume MCP credits.
- List tools return up to 50 rows per page.
- Stats and posts tools return up to 50 rows per call.
- Most current tools are read-only.
- `social_competitors_create` is a write action and should only be used when the user explicitly wants to add a tracked competitor.
- MCP access follows the connected Sociality.io user and workspace permissions.
- Use least-privilege Sociality.io accounts for production or automated workflows.

## MCP vs Sociality.io Dashboard

Sociality.io remains the platform for people working through product interfaces to publish, engage, listen, and report in one place.

Sociality.io MCP is for agent-driven access through tool calls. It lets AI clients and custom systems work with Sociality.io account and competitor intelligence in structured workflows.

Teams can use MCP-only workflows for direct agent access, or use Dashboard + MCP when they want human workflows in Sociality.io alongside agent-powered analysis and automation.

## Repository Purpose

This repository is the public discovery and registry home for the remote Sociality.io MCP server. It contains the Official MCP Registry metadata and usage guidance. The hosted MCP endpoint runs at `https://api.sociality.io/mcp`.

## Official MCP Registry

This repository contains the `server.json` metadata used to publish Sociality.io MCP to the Official MCP Registry.

Registry server name:

```text
io.sociality/sociality-mcp
```
