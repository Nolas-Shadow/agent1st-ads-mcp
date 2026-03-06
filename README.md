<p align="center">
  <img src="https://raw.githubusercontent.com/agent1st/agent1st-ads-mcp/main/mcp-icon.png" alt="Agent 1st Ads MCP" width="120" />
</p>

# agent1st-ads-mcp

**Launch and manage Meta (Facebook/Instagram) and TikTok ad campaigns in seconds — from any AI agent.**

No dashboards. No manual setup. One tool call creates a complete campaign: targeting, creative, budget, and ad — ready to run.

[![npm version](https://badge.fury.io/js/agent1st-ads-mcp.svg)](https://www.npmjs.com/package/agent1st-ads-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

---

## What This Does

This MCP server gives AI agents (Claude, GPT, Cursor, etc.) the ability to:

- **Create full campaigns** on Meta and TikTok in one tool call — campaign + targeting + ad creative bundled together
- **Control spend** — enable, pause, delete, or update budgets instantly
- **Read performance** — get impressions, clicks, spend, CTR, and conversions
- **Work autonomously** — no human needs to touch the ad platform

Works with Meta (Facebook + Instagram) and TikTok. Connect one or both.

---

## Quick Start

### Step 1 — Install

```bash
npm install -g agent1st-ads-mcp
```

Or run without installing:

```bash
npx agent1st-ads-mcp
```

### Step 2 — Get Your Credentials

**Meta (Facebook/Instagram):**
1. Go to [business.facebook.com](https://business.facebook.com)
2. Get your **Ad Account ID** from Ads Manager → Account Settings (format: `act_XXXXXXXXXX`)
3. Get a **Page ID** from your Facebook Page → About → Page ID
4. Get an **Access Token** from [developers.facebook.com/tools/explorer](https://developers.facebook.com/tools/explorer) — select your app, add `ads_management` and `pages_read_engagement` permissions

**TikTok:**
1. Go to [ads.tiktok.com](https://ads.tiktok.com)
2. Get your **Advertiser ID** from Account → Basic Information
3. Get an **Access Token** from [business-api.tiktok.com](https://business-api.tiktok.com/portal/docs) → My Apps

### Step 3 — Configure

Create a `.env` file or set environment variables:

```env
# Meta (Facebook/Instagram) — all three required for Meta
META_ADS_ACCESS_TOKEN=your_meta_access_token
META_ADS_ACCOUNT_ID=act_XXXXXXXXXX
META_PAGE_ID=XXXXXXXXXX

# TikTok — both required for TikTok
TIKTOK_ADS_ACCESS_TOKEN=your_tiktok_access_token
TIKTOK_ADVERTISER_ID=XXXXXXXXXX
```

You can connect Meta only, TikTok only, or both.

### Step 4 — Add to Your AI Agent

**Claude Desktop** (`~/Library/Application Support/Claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "agent1st-ads": {
      "command": "npx",
      "args": ["agent1st-ads-mcp"],
      "env": {
        "META_ADS_ACCESS_TOKEN": "your_token",
        "META_ADS_ACCOUNT_ID": "act_XXXXXXXXXX",
        "META_PAGE_ID": "XXXXXXXXXX",
        "TIKTOK_ADS_ACCESS_TOKEN": "your_token",
        "TIKTOK_ADVERTISER_ID": "XXXXXXXXXX"
      }
    }
  }
}
```

**Cursor / Windsurf / Other MCP Clients:** Add to your MCP settings using the same format above.

---

## Tools Reference

### Setup & Account

#### `check_setup`
Verify which platforms are connected and ready. **Always call this first.**

```json
// Input
{}

// Output
{
  "meta": { "connected": true, "account_id": "act_123456", "has_page": true },
  "tiktok": { "connected": true, "advertiser_id": "789012" },
  "ready": true,
  "message": "Both Meta and TikTok are connected."
}
```

#### `get_ad_account_info`
Get current balance, total spend, and account status for all connected platforms.

```json
// Input
{}

// Output
{
  "meta": {
    "name": "My Business Ad Account",
    "account_status": 1,
    "currency": "USD",
    "balance": "500.00",
    "amount_spent": "1243.50"
  },
  "tiktok": {
    "data": { "name": "My TikTok Ads", "status": "STATUS_ENABLE", "balance": 320.0 }
  }
}
```

---

### Meta Campaigns

#### `create_meta_campaign`
Create a full Meta campaign in one call. Returns IDs for all created objects.

```json
// Input
{
  "name": "Spring Sale — Traffic — US",
  "objective": "OUTCOME_TRAFFIC",
  "daily_budget_usd": 25,
  "destination_url": "https://mysite.com/spring-sale",
  "ad_headline": "50% Off This Week Only",
  "ad_body": "Shop our biggest sale of the year. Free shipping on all orders.",
  "targeting_countries": ["US", "CA"],
  "age_min": 25,
  "age_max": 54
}

// Output
{
  "success": true,
  "status": "PAUSED",
  "next_step": "Call enable_meta_campaign with the campaign_id below to start running.",
  "campaign_id": "120210001234567",
  "adset_id": "120210001234568",
  "creative_id": "120210001234569",
  "ad_id": "120210001234570",
  "daily_budget_usd": 25,
  "objective": "OUTCOME_TRAFFIC"
}
```

**Objectives:**
| Value | Use When |
|-------|----------|
| `OUTCOME_TRAFFIC` | Drive clicks to a website |
| `OUTCOME_LEADS` | Collect leads via lead forms |
| `OUTCOME_SALES` | Drive purchases (requires pixel) |
| `OUTCOME_ENGAGEMENT` | Boost post likes and comments |
| `OUTCOME_AWARENESS` | Maximize reach |

#### `enable_meta_campaign`
Start a paused campaign.

```json
// Input
{ "campaign_id": "120210001234567" }

// Output
{ "success": true, "campaign_id": "120210001234567", "status": "ACTIVE", "message": "Campaign is now live and spending." }
```

#### `pause_meta_campaign`
Stop a running campaign. Settings preserved.

```json
// Input
{ "campaign_id": "120210001234567" }
```

#### `get_meta_campaign_stats`
Get performance data for a campaign.

```json
// Input
{ "campaign_id": "120210001234567", "date_preset": "last_7d" }

// Output
{
  "data": [{
    "campaign_name": "Spring Sale — Traffic — US",
    "impressions": "14320",
    "clicks": "423",
    "spend": "47.82",
    "ctr": "2.954",
    "cpm": "3.34"
  }]
}
```

**Date presets:** `today`, `yesterday`, `last_7d`, `last_14d`, `last_30d`, `this_month`

#### `update_meta_campaign_budget`
Change daily budget. Takes effect immediately.

```json
// Input
{ "campaign_id": "120210001234567", "daily_budget_usd": 50 }
```

#### `list_meta_campaigns`
List all campaigns with IDs and status.

```json
// Input
{ "limit": 10, "status": "ACTIVE" }
```

#### `delete_meta_campaign`
Permanently delete a campaign. Use pause instead if you want to keep it.

```json
// Input
{ "campaign_id": "120210001234567" }
```

---

### TikTok Campaigns

#### `create_tiktok_campaign`
Create a full TikTok campaign in one call.

```json
// Input
{
  "name": "Product Launch — TikTok — US",
  "objective": "TRAFFIC",
  "budget_usd": 30,
  "destination_url": "https://mysite.com/product",
  "ad_text": "This changed everything. Tap to see why.",
  "targeting_countries": ["US"],
  "age_groups": ["AGE_18_24", "AGE_25_34"]
}

// Output
{
  "success": true,
  "status": "DISABLE",
  "next_step": "Call enable_tiktok_campaign with the campaign_id below to start running.",
  "campaign_id": "1234567890123",
  "adgroup_id": "9876543210987",
  "ad_id": "1122334455667",
  "budget_usd": 30,
  "objective": "TRAFFIC"
}
```

**Objectives:**
| Value | Use When |
|-------|----------|
| `TRAFFIC` | Drive clicks to a website |
| `LEAD_GENERATION` | Collect leads |
| `WEBSITE_CONVERSIONS` | Drive purchases |
| `ENGAGEMENT` | Boost video views/interactions |
| `APP_PROMOTION` | App installs |

**Age groups:** `AGE_13_17`, `AGE_18_24`, `AGE_25_34`, `AGE_35_44`, `AGE_45_54`, `AGE_55_100`

#### `enable_tiktok_campaign`
Start a disabled campaign.

```json
// Input
{ "campaign_id": "1234567890123" }

// Output
{ "success": true, "campaign_id": "1234567890123", "status": "ENABLE", "message": "Campaign is now live on TikTok." }
```

#### `pause_tiktok_campaign`
Stop a running TikTok campaign.

```json
// Input
{ "campaign_id": "1234567890123" }
```

#### `get_tiktok_campaign_stats`
Get performance data.

```json
// Input
{ "campaign_id": "1234567890123", "start_date": "2025-01-01", "end_date": "2025-01-07" }

// Output
{
  "data": {
    "list": [{
      "dimensions": { "campaign_id": "1234567890123", "stat_time_day": "2025-01-07" },
      "metrics": { "spend": "28.40", "impressions": "9821", "clicks": "312", "ctr": "3.18" }
    }]
  }
}
```

#### `update_tiktok_campaign_budget`
Change daily budget. TikTok minimum is $20/day.

```json
// Input
{ "campaign_id": "1234567890123", "budget_usd": 60 }
```

#### `list_tiktok_campaigns`
List all TikTok campaigns.

```json
// Input
{ "limit": 20 }
```

---

## Common Workflows

### Launch a campaign end-to-end

```
1. check_setup                    → confirm platform is connected
2. get_ad_account_info            → confirm balance is sufficient
3. create_meta_campaign           → create the campaign (starts PAUSED)
4. enable_meta_campaign           → activate it
5. get_meta_campaign_stats        → check performance after 24h
6. update_meta_campaign_budget    → scale budget if performing well
```

### Daily performance review

```
1. list_meta_campaigns            → get all active campaign IDs
2. get_meta_campaign_stats        → check each (date_preset: yesterday)
3. pause_meta_campaign            → pause any underperforming ones
4. update_meta_campaign_budget    → increase budget on winners
```

### Launch same campaign on both platforms

```
1. create_meta_campaign           → create Meta version
2. enable_meta_campaign           → activate Meta
3. create_tiktok_campaign         → create TikTok version
4. enable_tiktok_campaign         → activate TikTok
```

---

## Environment Variables

| Variable | Platform | Required | Description |
|----------|----------|----------|-------------|
| `META_ADS_ACCESS_TOKEN` | Meta | Yes | Meta Marketing API access token |
| `META_ADS_ACCOUNT_ID` | Meta | Yes | Ad account ID (format: `act_XXXXXXXXXX`) |
| `META_PAGE_ID` | Meta | Yes | Facebook Page ID (needed to create ads) |
| `TIKTOK_ADS_ACCESS_TOKEN` | TikTok | Yes | TikTok Business API access token |
| `TIKTOK_ADVERTISER_ID` | TikTok | Yes | TikTok advertiser account ID |

---

## Error Handling

All tools return structured errors so agents can handle them gracefully:

```json
{ "error": true, "message": "Meta API: Invalid OAuth access token. (code 190)" }
```

Common errors and fixes:
- **`Invalid OAuth access token`** — Token expired. Regenerate at developers.facebook.com/tools/explorer
- **`META_PAGE_ID is required`** — Add your Facebook Page ID to environment variables
- **`TikTok minimum is $20/day`** — Increase `budget_usd` to at least 20
- **`Meta not connected`** — Verify `META_ADS_ACCESS_TOKEN` and `META_ADS_ACCOUNT_ID` are set

---

## Requirements

- Node.js 18+
- Meta Business account with Ads Manager access (for Meta tools)
- TikTok Business account with API access (for TikTok tools)

---

## Links

- **Website:** [agent1st.io/ads](https://agent1st.io/ads/)
- **npm:** [npmjs.com/package/agent1st-ads-mcp](https://www.npmjs.com/package/agent1st-ads-mcp)
- **Issues:** [github.com/agent1st/agent1st-ads-mcp/issues](https://github.com/agent1st/agent1st-ads-mcp/issues)

---

## License

MIT © [Agent 1st](https://agent1st.io)
