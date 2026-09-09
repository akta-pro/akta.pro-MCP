# akta.pro MCP Server

**Private company data and signals API**

Give your AI agent structured intelligence on 20M+ private companies plus entity-resolved, real-time news signals — via a single MCP server.

akta.pro MCP connects Claude, ChatGPT, Cursor, VS Code, and other MCP clients to enterprise-grade private company data and real-time news signals. 20M+ entity-resolved companies, 70+ data points each, 30K+ sub-sectors monitored, and ~80% news noise filtered — pay-as-you-go.

- **Server URL:** `https://mcp.akta.pro/mcp`
- **Website:** [https://akta.pro](https://akta.pro) · **Docs:** [https://docs.akta.pro](https://docs.akta.pro)
- **Transport:** HTTP (streamable HTTP / SSE-compatible) — remote server, no local install
- **Auth:** Native connector auth (Claude) · OAuth (ChatGPT) · `x-api-key` header (Cursor, VS Code, Claude Code, OpenCode)

---

## Overview

akta.pro MCP connects Claude, ChatGPT, Cursor, VS Code, and other MCP clients to enterprise-grade private company data and real-time news signals. 20M+ entity-resolved companies, 70+ data points each, 30K+ sub-sectors monitored, and ~80% news noise filtered — pay-as-you-go.

Point Claude, ChatGPT, Cursor, VS Code, or Claude Code at `https://mcp.akta.pro/mcp` and your agent can:

- **Resolve** any company by name, website, or UUID with patent-pending entity resolution across 20M+ global entities.
- **Enrich** private companies across 70+ structured data points — firmographics, business model, product offering, management, financials, technology stack, industry codes, and (Enterprise) funding & M&A history.
- **Monitor** company, industry, or open-ended topic news in real time with AI summaries, sentiment scores, event-type classification (77 tag codes across 11 categories), and named-entity extraction. ~80% of noise is filtered before it hits your workflow.
- **Surface alternative signals** (Subscription/Enterprise): LinkedIn headcount trends, website traffic, Glassdoor-style employee reviews, G2-style product reviews, live LinkedIn/Indeed job posts, and company social posts.

All tools are synchronous — data is returned immediately, no polling. Responses use deterministic schemas, compact payloads, and ship with source attribution so every claim is verifiable. `company_data` is billed per section, so agents only pay for what they ask for. Free resolve-first tools (`company_search`, `industry_search`, `account_status`, `news_types`) cost 0 credits.

SOC 2 Type II certified, ISO 27001 compliant, end-to-end encrypted, and no customer data is used to train AI models.

## Data coverage

- 20M+ globally entity-resolved private companies
- 70+ structured data points per company
- 30K+ sub-sectors monitored
- ~80% of news noise filtered before delivery
- News classification: 77 event-type tag codes across 11 categories
- Industry classifications supported: NAICS, SIC, IPTC, IAB
- Location fields normalized to ISO country codes

## Tools

All tools are synchronous (data returned immediately, no polling). `company_data` returns Markdown; every other tool returns JSON.

### Free resolve-first tools (0 credits)

| Tool | Description |
| --- | --- |
| `company_search` | Resolve a company name or website to Akta identifiers (`uuid`, website, status) |
| `industry_search` | Resolve a free-text industry or topic to ranked industry codes, used to filter news by industry |
| `account_status` | Your plan tier (`is_enterprise`, `package_type`) and remaining credit balance |
| `list_filters` | List the available filter fields for `generate_company_list` (e.g. `location.hq.country`, `firmographic.founded_year`), grouped by filter category |
| `get_filter_values` | Get the allowed values for one or more filters (e.g. valid country codes, funding stage enums), used to build a valid `filters` payload before calling `generate_company_list` |

### Company data

| Tool | Description |
| --- | --- |
| `company_data` | Rich structured company profile as Markdown. Sections selected explicitly and billed per section. |
| `company_data_concise` | Single condensed company overview in JSON. No section selection, flat rate, cheapest fast read. |

### News

| Tool | Description |
| --- | --- |
| `news_signals` | List news filtered by company, industry, query, or title with sentiment and AI summaries. No full article body. |
| `news_detail` | Full article body for specific article IDs from news\_signals (max 10 per call). |

### List Generation:

| Tool | Description |
| --- | --- |
| `generate_company_list` | Build a targeted list of companies using structured filters or a natural language query, with optional per-company data enrichment |

### Alternative signals (Subscription or Enterprise)

| Tool | Description |
| --- | --- |
| `headcount_trends` | LinkedIn-sourced employee-count trends over time + functional breakdown. |
| `website_traffic` | Engagement metrics, monthly visits, traffic-source breakdown. |
| `employee_reviews` | Glassdoor-style overall + 8 dimension-level ratings, plus individual reviews. |
| `product_reviews` | Product catalog and per-product reviews (G2, etc.). Call without a product ID first to list the catalog. |
| `job_posts` | Live LinkedIn/Indeed job listings — title, location, description, compensation, experience level, skills. |
| `social_posts` | Company social posts — content type, text, date, paid/repost flags, AI classification, engagement. |

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

```
claude mcp add --transport http akta-pro https://mcp.akta.pro/mcp \
  --header "x-api-key: <YOUR_API_KEY>"
```

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
  "servers": {
    "akta-pro": {
      "type": "http",
      "url": "https://mcp.akta.pro/mcp",
      "headers": { "x-api-key": "YOUR_AKTA_API_KEY" }
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
      "headers": { "x-api-key": "YOUR_AKTA_API_KEY" }
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
- **Subscription** — unlocks alternative-signal tools (headcount\_trends, website\_traffic, employee\_reviews, product\_reviews, job\_posts, social\_posts).
- **Enterprise** — unlocks funding\_detail and mna\_and\_investment sections plus enterprise SLAs, bulk export, and dedicated support.

Full pricing: [https://akta.pro/pricing](https://akta.pro/pricing) · Rate limits: [https://docs.akta.pro/getting-started/rate-limits](https://docs.akta.pro/getting-started/rate-limits) · Error codes: [https://docs.akta.pro/getting-started/error-codes](https://docs.akta.pro/getting-started/error-codes)

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
- X / Twitter: [https://x.com/akta\_pro](https://x.com/akta_pro)

---

© 2026 Wokelo AI. All Rights Reserved. Commercial (proprietary). Free tier via pay-as-you-go credits.
