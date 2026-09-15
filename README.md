# akta.pro MCP Server

**Private company data, news, and signals API for AI agents**

akta.pro MCP gives AI agents structured access to public and private company data, entity-resolved news, targeted list generation, and alternative signals. Resolve companies, retrieve rich or concise profiles, monitor real-time and historical news, build company lists, and surface headcount, web traffic, hiring, social, employee-review, and product-review signals where the plan permits. Tools return synchronously through deterministic schemas with source attribution.

- **Server URL:** `https://mcp.akta.pro/mcp`
- **Website:** [https://akta.pro](https://akta.pro) · **Docs:** [https://docs.akta.pro](https://docs.akta.pro)
- **Transport:** HTTP (Streamable HTTP / SSE-compatible) — remote server, no local install
- **Auth:** Claude Web/Desktop: native connector authentication. ChatGPT web: OAuth. API-key clients: x-api-key header.

---

## Overview

akta.pro MCP connects Claude, ChatGPT, Cursor, VS Code, Claude Code, and other MCP clients to enterprise-grade private company data and real-time news signals. 20M+ entity-resolved companies, 70+ data points each, 30K+ sub-sectors monitored — pay-as-you-go.

Point Claude, ChatGPT, Cursor, VS Code, or Claude Code at `https://mcp.akta.pro/mcp` and your agent can:

- **Resolve** any company by name, website, or UUID with entity resolution across 20M+ global entities.
- **Enrich** private companies across 70+ structured data points — firmographics, business model, product offering, management, financials, technology stack, industry codes, and (Enterprise) funding & M&A history.
- **Monitor** company, industry, or open-ended topic news in real time with AI summaries, sentiment scores, event-type classification, and source metadata. News is enriched with entity resolution, de-duplication, and named-entity extraction.
- **Surface alternative signals** (Subscription/Enterprise): headcount trends, website traffic, employee reviews, product reviews, live job posts, and company social posts.

All tools are synchronous — data is returned immediately, no polling. Responses use deterministic schemas, compact payloads, and ship with source attribution so every claim is verifiable. `company_data` is billed per section, so agents only pay for what they ask for. Free resolve-first tools (`company_search`, `industry_search`, `account_status`, `news_types`, `list_filters`, `get_filter_values`) cost 0 credits.

SOC 2 Type II certified, ISO 27001 compliant, end-to-end encrypted, and no customer data is used to train AI models.

## Data coverage

- 20M+ globally entity-resolved private companies
- 70+ structured data points per company
- 30K+ sub-sectors monitored
- News enriched with entity resolution, de-duplication, AI summaries, sentiment, event classification, and source metadata
- Industry classifications supported: NAICS, SIC, IPTC, IAB
- Location fields normalized to country codes

## Tools

All tools are synchronous (data returned immediately, no polling). `company_data` returns Markdown; every other tool returns JSON.

### Free resolve-first tools (0 credits)

| Tool | Description |
| --- | --- |
| `company_search` | Resolve a company name or website to akta identifiers |
| `industry_search` | Resolve free-text industry or topic language to ranked industry codes |
| `account_status` | Return plan tier and remaining credit balance |
| `news_types` | Return the event-type taxonomy used to filter news_signals |
| `list_filters` | List available fields for `generate_company_list`, grouped by category |
| `get_filter_values` | Return allowed values for one or more list-generation filters, used to build a valid filters payload |

### Company data

| Tool | Description |
| --- | --- |
| `company_data` | Rich structured company profile as Markdown. Sections selected explicitly and billed per section. |
| `company_data_concise` | Condensed company overview in JSON. Flat rate, no section selection. |

### News

| Tool | Description |
| --- | --- |
| `news_signals` | Discover news filtered by company, industry, query, title, sentiment, event type, date, and other supported fields. Returns summaries and metadata, not full article body. |
| `news_detail` | Full article body for article IDs returned by news_signals (max 10 per call). |

### List generation

| Tool | Description |
| --- | --- |
| `generate_company_list` | Build a targeted company list from structured filters or a natural-language query, with optional per-company enrichment where supported. |

### Alternative signals (Subscription or Enterprise)

| Tool | Description |
| --- | --- |
| `headcount_trends` | Employee-count trends and functional breakdown. |
| `website_traffic` | Traffic and engagement estimates. |
| `employee_reviews` | Overall, dimension-level, and individual employee-review signals. |
| `product_reviews` | Product catalog and product-review signals. |
| `job_posts` | Live job-posting and hiring signals. |
| `social_posts` | Company social posts and engagement metadata. |

## Install

**Server URL for every client:** `https://mcp.akta.pro/mcp`

### Claude (Web / Desktop)

1. Settings (or Customize) → Connectors → Add Connectors
2. Name: `akta-pro` · Server URL: `https://mcp.akta.pro/mcp`
3. Click Add, then complete the authentication.

### ChatGPT

1. Settings → Plugins → Developer Mode → enable.
2. Settings → Plugins → Browse Plugins → +.
3. Name `akta-pro`, paste `https://mcp.akta.pro/mcp`, authentication = OAuth.
4. Create and complete OAuth.

### Claude Code
claude mcp add --transport http akta-pro https://mcp.akta.pro/mcp
--header "x-api-key: <YOUR_API_KEY>"


### Cursor — `~/.cursor/mcp.json`

```json
{
  "mcpServers": {
    "akta-pro": {
      "url": "https://mcp.akta.pro/mcp",
      "headers": { "x-api-key": "YOUR_AKTA_API_KEY" }
    }
  }
}
```

### VS Code — `.vscode/mcp.json`

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "akta-api-key",
      "description": "akta.pro API key",
      "password": true
    }
  ],
  "servers": {
    "akta-pro": {
      "type": "http",
      "url": "https://mcp.akta.pro/mcp",
      "headers": { "x-api-key": "${input:akta-api-key}" }
    }
  }
}
```

### OpenCode — `opencode.json`

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "akta-pro": {
      "type": "remote",
      "url": "https://mcp.akta.pro/mcp",
      "enabled": true,
      "oauth": false,
      "headers": { "x-api-key": "{env:AKTA_API_KEY}" }
    }
  }
}
```

## Authentication

- **Native connector auth** — Claude Web/Desktop (no API key required).
- **OAuth** — ChatGPT.
- **`x-api-key` header** — Cursor, VS Code, Claude Code, OpenCode.
  - API key signup: [https://playground.akta.pro/signup](https://playground.akta.pro/signup)
  - API key management: [https://playground.akta.pro/dashboard/manage/api-keys](https://playground.akta.pro/dashboard/manage/api-keys)

## Pricing

- **Pay-as-you-go** — no minimum, credits purchased upfront, per-tool billing.
- **Subscription** — unlocks alternative-signal tools (`headcount_trends`, `website_traffic`, `employee_reviews`, `product_reviews`, `job_posts`, `social_posts`).
- **Enterprise** — unlocks `funding_detail` and `mna_and_investment` sections plus enterprise SLAs, bulk export, and dedicated support.

Full pricing: [https://akta.pro/pricing](https://akta.pro/pricing) · MCP credits: [https://docs.akta.pro/docs/developer-tools/mcp/credits](https://docs.akta.pro/docs/developer-tools/mcp/credits)

## Compliance & trust

- SOC 2 Type II certified
- ISO 27001 compliant
- End-to-end encrypted in transit and at rest
- No customer data is used to train AI models
- OpenAPI spec: [https://docs.akta.pro/openapi.json](https://docs.akta.pro/openapi.json)
- LLM-friendly docs: [https://docs.akta.pro/llms.txt](https://docs.akta.pro/llms.txt) · [https://docs.akta.pro/llms-full.txt](https://docs.akta.pro/llms-full.txt)

## Links

- Website: [https://akta.pro](https://akta.pro)
- Documentation: [https://docs.akta.pro](https://docs.akta.pro)
- Pricing: [https://akta.pro/pricing](https://akta.pro/pricing)
- Changelog: [https://docs.akta.pro/changelog](https://docs.akta.pro/changelog)
- Contact / Sales: [https://docs.akta.pro/contact](https://docs.akta.pro/contact)
- LinkedIn: [https://www.linkedin.com/company/akta-pro](https://www.linkedin.com/company/akta-pro)
- X / Twitter: [https://x.com/akta_pro](https://x.com/akta_pro)

---

© 2026 Wokelo AI. All Rights Reserved. Commercial (proprietary). Free tier via pay-as-you-go credits.
