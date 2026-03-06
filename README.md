# Agent 1st Ads MCP

> **One tool call. Full ad campaign. Live in seconds.**

Launch and manage Meta (Facebook/Instagram) and TikTok ad campaigns from any AI agent — no dashboards, no manual setup.

---

## 30-Second Setup

```bash
# 1. Install
npx meta-tiktok-ads-from-agent1st

# 2. Add to Claude Desktop config
# Mac: ~/Library/Application Support/Claude/claude_desktop_config.json
# Win: %APPDATA%\Claude\claude_desktop_config.json
```

```json
{
  "mcpServers": {
    "agent1st-ads": {
      "command": "npx",
      "args": ["meta-tiktok-ads-from-agent1st"],
      "env": {
        "AGENT1ST_LICENSE_KEY": "your_key_here",
        "META_ADS_ACCESS_TOKEN": "your_meta_token",
        "META_ADS_ACCOUNT_ID": "act_XXXXXXXXXX",
        "META_PAGE_ID": "XXXXXXXXXX"
      }
    }
  }
}
```

**3. Restart Claude Desktop. Done.**

---

## Quick Reference

### First Call (Always)

```
check_setup
```

Returns license status and connected platforms.

### Create a Campaign

```json
create_meta_campaign {
  "name": "Summer Sale — US",
  "daily_budget_usd": 25,
  "destination_url": "https://yoursite.com/sale",
  "ad_headline": "50% Off Everything",
  "ad_body": "Limited time. Free shipping.",
  "targeting_countries": ["US"]
}
```

**Returns:** `campaign_id` (starts PAUSED)

### Go Live

```json
enable_meta_campaign { "campaign_id": "120210001234567" }
```

### Check Performance

```json
get_meta_campaign_stats { "campaign_id": "120210001234567", "date_preset": "last_7d" }
```

### Stop Spending

```json
pause_meta_campaign { "campaign_id": "120210001234567" }
```

---

## All Tools

| Tool | What It Does | Required Params |
|------|--------------|-----------------|
| `check_setup` | Verify license + connections | — |
| `get_ad_account_info` | Balance, spend, status | — |
| **Meta** | | |
| `create_meta_campaign` | Create full campaign | name, daily_budget_usd, destination_url, ad_headline, ad_body |
| `enable_meta_campaign` | Start spending | campaign_id |
| `pause_meta_campaign` | Stop spending | campaign_id |
| `get_meta_campaign_stats` | Performance data | campaign_id |
| `update_meta_campaign_budget` | Change budget | campaign_id, daily_budget_usd |
| `list_meta_campaigns` | List all campaigns | — |
| `delete_meta_campaign` | Permanently delete | campaign_id |
| **TikTok** (Operator+) | | |
| `create_tiktok_campaign` | Create full campaign | name, budget_usd, destination_url, ad_text |
| `enable_tiktok_campaign` | Start running | campaign_id |
| `pause_tiktok_campaign` | Stop running | campaign_id |
| `get_tiktok_campaign_stats` | Performance data | campaign_id |
| `update_tiktok_campaign_budget` | Change budget | campaign_id, budget_usd |
| `list_tiktok_campaigns` | List all campaigns | — |

---

## Environment Variables

| Variable | Required | Get It From |
|----------|----------|-------------|
| `AGENT1ST_LICENSE_KEY` | ✅ | [agent1st.io/ads](https://agent1st.io/ads/) |
| `META_ADS_ACCESS_TOKEN` | For Meta | [developers.facebook.com/tools/explorer](https://developers.facebook.com/tools/explorer) |
| `META_ADS_ACCOUNT_ID` | For Meta | Ads Manager → Settings (format: `act_123456`) |
| `META_PAGE_ID` | For Meta | Facebook Page → About → Page ID |
| `TIKTOK_ADS_ACCESS_TOKEN` | For TikTok | [business-api.tiktok.com](https://business-api.tiktok.com) |
| `TIKTOK_ADVERTISER_ID` | For TikTok | TikTok Ads Manager → Account Info |

---

## Pricing

| Plan | Price | Platforms |
|------|-------|-----------|
| **Scout** | $29/mo | Meta OR TikTok |
| **Operator** | $69/mo | Meta + TikTok |
| **Commander** | $149/mo | Both + Advanced |
| **Agency** | $399/mo | Unlimited |

→ Get your key: [agent1st.io/ads](https://agent1st.io/ads/)

---

## Common Workflows

### Launch Campaign End-to-End

```
1. check_setup                    → verify connected
2. get_ad_account_info            → check balance
3. create_meta_campaign           → create (PAUSED)
4. enable_meta_campaign           → go live
5. get_meta_campaign_stats        → check after 24h
```

### Daily Review

```
1. list_meta_campaigns            → get IDs
2. get_meta_campaign_stats        → check each
3. pause_meta_campaign            → pause losers
4. update_meta_campaign_budget    → scale winners
```

### Launch on Both Platforms

```
1. create_meta_campaign           → Meta version
2. enable_meta_campaign           → activate
3. create_tiktok_campaign         → TikTok version
4. enable_tiktok_campaign         → activate
```

---

## Objectives

### Meta

| Value | Use When |
|-------|----------|
| `OUTCOME_TRAFFIC` | Drive website clicks |
| `OUTCOME_LEADS` | Collect leads |
| `OUTCOME_SALES` | Drive purchases |
| `OUTCOME_ENGAGEMENT` | Boost engagement |
| `OUTCOME_AWARENESS` | Maximize reach |

### TikTok

| Value | Use When |
|-------|----------|
| `TRAFFIC` | Drive website clicks |
| `LEAD_GENERATION` | Collect leads |
| `WEBSITE_CONVERSIONS` | Drive purchases |
| `ENGAGEMENT` | Boost interactions |
| `APP_PROMOTION` | App installs |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| `License required` | Set `AGENT1ST_LICENSE_KEY` |
| `Invalid OAuth access token` | Regenerate at [developers.facebook.com/tools/explorer](https://developers.facebook.com/tools/explorer) |
| `META_PAGE_ID required` | Add your Facebook Page ID |
| `TikTok minimum is $20/day` | Set `budget_usd` ≥ 20 |
| `Your Scout plan supports one platform` | Upgrade to Operator at [agent1st.io/ads](https://agent1st.io/ads/) |

---

## Links

- **License Key:** [agent1st.io/ads](https://agent1st.io/ads/)
- **npm:** [npmjs.com/package/meta-tiktok-ads-from-agent1st](https://www.npmjs.com/package/meta-tiktok-ads-from-agent1st)
- **Support:** Info@Agent1st.io
