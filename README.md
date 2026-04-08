# Claude x Meta Ads

This repository tracks the setup of [meta-ads-mcp](https://github.com/attainmentlabs/meta-ads-mcp) — an MCP server that lets Claude create and manage Meta (Facebook/Instagram) ad campaigns.

## Available Tools

| Tool | Description |
|------|-------------|
| `create_meta_campaign` | Build complete campaigns (ad sets, creatives, ads) |
| `get_campaign_status` | Retrieve campaign performance metrics |
| `pause_campaign` | Halt active campaigns |
| `activate_campaign` | Launch paused campaigns |
| `delete_campaign` | Permanently remove campaigns |

> **Note:** `create_meta_campaign` defaults to `dry_run=True` — simulates API calls without spending. Set `dry_run=False` for live deployment.

## Setup

### Requirements

- Python 3.9+
- `uv` installed (`curl -LsSf https://astral.sh/uv/install.sh | sh`)
- Meta Business Manager account with an Ad Account

### Configuration

Copy `mcp.json.example` to `~/.mcp.json` and fill in your credentials:

```json
{
  "mcpServers": {
    "meta-ads": {
      "command": "uvx",
      "args": ["meta-ads-manager-mcp"],
      "env": {
        "META_ACCESS_TOKEN": "...",
        "META_AD_ACCOUNT_ID": "...",
        "META_PAGE_ID": "..."
      }
    }
  }
}
```

### Credentials

| Variable | Description | Where to find |
|----------|-------------|---------------|
| `META_ACCESS_TOKEN` | Long-lived Graph API token (60-day expiry) | [Graph API Explorer](https://developers.facebook.com/tools/explorer/) — generate with `ads_management`, `ads_read`, `pages_read_engagement`, `pages_show_list` permissions, then exchange for long-lived token |
| `META_AD_ACCOUNT_ID` | Numeric Ad Account ID (no `act_` prefix) | business.facebook.com → Ad Accounts |
| `META_PAGE_ID` | Numeric Facebook Page ID | Your Page → About → Page Transparency |

### Token Refresh

Tokens expire after 60 days. Re-generate and exchange via the Graph API Explorer, then update `META_ACCESS_TOKEN` in `~/.mcp.json`.
