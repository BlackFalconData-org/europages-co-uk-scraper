# Europages Scraper

Extract structured data from [europages.co.uk](https://europages.co.uk) — Europages, the European B2B supplier directory: export manufacturers, wholesalers and service providers by product category and country with address, phone, website, headcount band, trade role and product catalogue. Incremental tracking of new and changed companies.

**[Europages Scraper on Apify →](https://apify.com/blackfalcondata/europages-co-uk-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, contact information for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N records (up to 3.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from europages.co.uk on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from europages.co.uk.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "building materials",
  "maxResults": 100,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "includeDetails": false
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | The product or company category to list suppliers for, exactly as europages names it — e.g. "building materials", "industrial valves", "packaging machinery". The site has no all-companies listing, so this (or a start URL) is required. Use a JSON array for several categories in one run, e.g. ["building materials","cement"]. |
| `location` | string | — | Optional. Restrict the results to suppliers in one country, e.g. "germany", "france", "italy". Leave empty to cover every country in the directory. Use a JSON array for several countries. |
| `maxResults` | integer | `100` | Maximum total companies to return (0 = as many as available). A single category and country search returns up to 3,000 companies; use several categories or countries for more. |
| `startUrls` | array | `[]` | Paste company-category listing URLs from europages (e.g. https://www.europages.co.uk/companies/building-materials.html, optionally country-restricted). Each URL becomes its own task; results are merged and deduped by company across all URLs. When provided, startUrls REPLACE the search and country fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `descriptionMaxLength` | integer | `0` | Truncate each company description to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Core company fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single company-description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state and return only new or changed companies. stateKey is optional — defaults to a value derived from your search inputs (categories, countries, startUrls) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked set of companies. Leave empty to auto-generate from the search inputs. |
| `emitUnchanged` | boolean | `false` | Also emit companies that have not changed since the last run (Incremental Mode only). |
| `emitExpired` | boolean | `false` | Also emit companies that have disappeared since the last run (Incremental Mode only). Requires a complete run, so it scans the whole result set. |
| `skipReposts` | boolean | `false` | When incremental, skip companies whose content matches an entry that disappeared in a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED companies. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of companies included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `includeDetails` | boolean | `false` | Company records already carry every field the directory publishes, so this setting does not change the output. |
| `enrichEmails` | boolean | `false` | Best-effort discovery of public contact emails from each company's own website (homepage plus a few contact pages). Adds time per company; only companies with a website are checked. Results appear in the email and emails fields. |

---

## Output fields

Every record returns the same 46-field schema. Missing values are `null` — never omitted.

- `listingId`
- `listingKey`
- `companyId`
- `uuid`
- `epId`
- `title`
- `companyName`
- `acronym`
- `description`
- `descriptionText`
- `descriptionHtml`
- `descriptionMarkdown`
- `location`
- `street`
- `city`
- `zipcode`
- `countryCode`
- `latitude`
- `longitude`
- `phoneNumber`
- `website`
- `emailAvailable`
- `foundingYear`
- `employeeCount`
- `supplierTypes`
- `distributionArea`
- `certificatesCount`
- `responseRate`
- `averageResponseTime`
- `isVerifiedSupplier`
- `productCount`
- `portfolioProductCount`
- `logoUrl`
- `pictures`
- `portfolioProducts`
- `productKeywords`
- `url`
- `portalUrl`
- `slug`
- `searchQuery`
- `contentQuality`
- `contentHash`
- `detailFetched`
- `scrapedAt`
- `source`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "listingId": "64b971b7d4ea1b8231c094e2c50f81ff1c4f81831959816b4463e8a4bfcb2d8d",
  "listingKey": "20047000",
  "companyId": "20047000",
  "uuid": "7e96c9d2-2895-3fab-9267-7300ec747986",
  "epId": "00000005269623-001",
  "title": "MLA FIXINGS",
  "companyName": "MLA FIXINGS",
  "acronym": "MF",
  "description": "MLA Fixings specializes in fasteners for facades and roofs. We have developed our own Dualfix system, which consists of a combination of insulation dowels and screw anchors. We pro…",
  "descriptionText": "MLA Fixings specializes in fasteners for facades and roofs. We have developed our own Dualfix system, which consists of a combination of insulation dowels and screw anchors. We pro…",
  "descriptionHtml": null,
  "descriptionMarkdown": "MLA Fixings specializes in fasteners for facades and roofs. We have developed our own Dualfix system, which consists of a combination of insulation dowels and screw anchors. We pro…"
}
```

*Truncated — full records contain 46 fields. See Output fields for the complete schema.*

**[Try Europages Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/europages-co-uk-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.00072 |

See the [actor on Apify](https://apify.com/blackfalcondata/europages-co-uk-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape europages.co.uk?**
Use this actor on Apify to extract structured data from europages.co.uk. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get europages.co.uk data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape europages.co.uk?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check europages.co.uk's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Europages Scraper](https://apify.com/blackfalcondata/europages-co-uk-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
